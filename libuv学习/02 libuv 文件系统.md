---
title: 02 libuv 文件系统
tags: libuv
grammar_cjkRuby: true
---

## 1. 概述
libuv可通过uv_fs_\*系列函数和 uv_fs_t 结构体进行操作。所有文件均提供同步和异步两种操作方式，在API函数使用过程中主要区别是函数回调是否为NULL,如果为NULL，则使用同步模式，其返回值则是文件描述符，否则则以异步方式调用，其返回值为libev错误码。

*特别注意*
* libev 文件异步操作与socket不同，socket异步操作是依靠操作系统提供的非阻塞接口，文件操作则通过线程池实现 *

## 2. Reading/Writing Files
2.1 打开及关闭文件文件
```c++
	int uv_fs_open(uv_loop_t* loop, uv_fs_t* req, const char* path, int flags, int mode, uv_fs_cb cb)
	int uv_fs_close(uv_loop_t* loop, uv_fs_t* req, uv_file file, uv_fs_cb cb)
	void callback(uv_fs_t* req);	 //文件回调签名
```
flags 和 mode与unix中文件操作的flag和mode一致，libuv内部负责将其转化成与windows兼容的windows flags

2.2 读写文件
```c++
int uv_fs_read(uv_loop_t* loop,
			 uv_fs_t* req,
               uv_file fd,
               const uv_buf_t bufs[],
               unsigned int nbufs,
               int64_t offset,
               uv_fs_cb cb) 
			   
 int uv_fs_write(uv_loop_t* loop,
                uv_fs_t* req,
                uv_file fd,
                const uv_buf_t bufs[],
                unsigned int nbufs,
                int64_t offset,
                uv_fs_cb cb)
```
在读文件之前，应该传递一个初始化过的buffer参数，他将在callback调用之前被填充，libuv将POSIX函数与uv_fs_\*函数建立映射，在文件操作中，如果触发EOF，uv_fs_t结构中result将会被置为0,若是操作stream或者pipe，UV_EOF将被用于传输操作的状态。

示例demo
```c++
#include <assert.h>
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <uv.h>

void on_read(uv_fs_t *req);

uv_fs_t open_req;
uv_fs_t read_req;
uv_fs_t write_req;

static char buffer[1024];

static uv_buf_t iov;

void on_write(uv_fs_t *req) {
    if (req->result < 0) {
        fprintf(stderr, "Write error: %s\n", uv_strerror((int)req->result));
    }
    else {
        uv_fs_read(uv_default_loop(), &read_req, open_req.result, &iov, 1, -1, on_read);
    }
}

void on_read(uv_fs_t *req) {
    if (req->result < 0) {
        fprintf(stderr, "Read error: %s\n", uv_strerror(req->result));
    }
    else if (req->result == 0) {
        uv_fs_t close_req;
        // synchronous
        uv_fs_close(uv_default_loop(), &close_req, open_req.result, NULL);
    }
    else if (req->result > 0) {
        iov.len = req->result;
        uv_fs_write(uv_default_loop(), &write_req, 1, &iov, 1, -1, on_write);
    }
}

void on_open(uv_fs_t *req) {
    // The request passed to the callback is the same as the one the call setup
    // function was passed.
    assert(req == &open_req);
    if (req->result >= 0) {
        iov = uv_buf_init(buffer, sizeof(buffer));
        uv_fs_read(uv_default_loop(), &read_req, req->result,
                   &iov, 1, -1, on_read);
    }
    else {
        fprintf(stderr, "error opening file: %s\n", uv_strerror((int)req->result));
    }
}

int main(int argc, char **argv) {
    uv_fs_open(uv_default_loop(), &open_req, argv[1], O_RDONLY, 0, on_open);
    uv_run(uv_default_loop(), UV_RUN_DEFAULT);

    uv_fs_req_cleanup(&open_req);
    uv_fs_req_cleanup(&read_req);
    uv_fs_req_cleanup(&write_req);
    return 0;
}

```

## filesystem 操作
所有unix标准文件你操作例如：unlink，rmdir，stat都支持异步操作并且和read/write有着相同的调用模式，返回值都是通过uv_fs_t.result获取的。
libuv提供的文件操作接口如下：
```c++
struct uv_dirent_s {
  const char* name;
  uv_dirent_type_t type;
};

UV_EXTERN char** uv_setup_args(int argc, char** argv);
UV_EXTERN int uv_get_process_title(char* buffer, size_t size);
UV_EXTERN int uv_set_process_title(const char* title);
UV_EXTERN int uv_resident_set_memory(size_t* rss);
UV_EXTERN int uv_uptime(double* uptime);
UV_EXTERN uv_os_fd_t uv_get_osfhandle(int fd);
UV_EXTERN int uv_open_osfhandle(uv_os_fd_t os_fd);

typedef struct {
  long tv_sec;
  long tv_usec;
} uv_timeval_t;

typedef struct {
  int64_t tv_sec;
  int32_t tv_usec;
} uv_timeval64_t;

typedef struct {
   uv_timeval_t ru_utime; /* user CPU time used */
   uv_timeval_t ru_stime; /* system CPU time used */
   uint64_t ru_maxrss;    /* maximum resident set size */
   uint64_t ru_ixrss;     /* integral shared memory size */
   uint64_t ru_idrss;     /* integral unshared data size */
   uint64_t ru_isrss;     /* integral unshared stack size */
   uint64_t ru_minflt;    /* page reclaims (soft page faults) */
   uint64_t ru_majflt;    /* page faults (hard page faults) */
   uint64_t ru_nswap;     /* swaps */
   uint64_t ru_inblock;   /* block input operations */
   uint64_t ru_oublock;   /* block output operations */
   uint64_t ru_msgsnd;    /* IPC messages sent */
   uint64_t ru_msgrcv;    /* IPC messages received */
   uint64_t ru_nsignals;  /* signals received */
   uint64_t ru_nvcsw;     /* voluntary context switches */
   uint64_t ru_nivcsw;    /* involuntary context switches */
} uv_rusage_t;

UV_EXTERN int uv_getrusage(uv_rusage_t* rusage);

UV_EXTERN int uv_os_homedir(char* buffer, size_t* size);
UV_EXTERN int uv_os_tmpdir(char* buffer, size_t* size);
UV_EXTERN int uv_os_get_passwd(uv_passwd_t* pwd);
UV_EXTERN void uv_os_free_passwd(uv_passwd_t* pwd);
UV_EXTERN uv_pid_t uv_os_getpid(void);
UV_EXTERN uv_pid_t uv_os_getppid(void);

#define UV_PRIORITY_LOW 19
#define UV_PRIORITY_BELOW_NORMAL 10
#define UV_PRIORITY_NORMAL 0
#define UV_PRIORITY_ABOVE_NORMAL -7
#define UV_PRIORITY_HIGH -14
#define UV_PRIORITY_HIGHEST -20

UV_EXTERN int uv_os_getpriority(uv_pid_t pid, int* priority);
UV_EXTERN int uv_os_setpriority(uv_pid_t pid, int priority);

UV_EXTERN int uv_cpu_info(uv_cpu_info_t** cpu_infos, int* count);
UV_EXTERN void uv_free_cpu_info(uv_cpu_info_t* cpu_infos, int count);

UV_EXTERN int uv_interface_addresses(uv_interface_address_t** addresses,
                                     int* count);
UV_EXTERN void uv_free_interface_addresses(uv_interface_address_t* addresses,
                                           int count);

UV_EXTERN int uv_os_getenv(const char* name, char* buffer, size_t* size);
UV_EXTERN int uv_os_setenv(const char* name, const char* value);
UV_EXTERN int uv_os_unsetenv(const char* name);

#ifdef MAXHOSTNAMELEN
# define UV_MAXHOSTNAMESIZE (MAXHOSTNAMELEN + 1)
#else
  /*
    Fallback for the maximum hostname size, including the null terminator. The
    Windows gethostname() documentation states that 256 bytes will always be
    large enough to hold the null-terminated hostname.
  */
# define UV_MAXHOSTNAMESIZE 256
#endif

UV_EXTERN int uv_os_gethostname(char* buffer, size_t* size);

UV_EXTERN int uv_os_uname(uv_utsname_t* buffer);

typedef enum {
  UV_FS_UNKNOWN = -1,
  UV_FS_CUSTOM,
  UV_FS_OPEN,
  UV_FS_CLOSE,
  UV_FS_READ,
  UV_FS_WRITE,
  UV_FS_SENDFILE,
  UV_FS_STAT,
  UV_FS_LSTAT,
  UV_FS_FSTAT,
  UV_FS_FTRUNCATE,
  UV_FS_UTIME,
  UV_FS_FUTIME,
  UV_FS_ACCESS,
  UV_FS_CHMOD,
  UV_FS_FCHMOD,
  UV_FS_FSYNC,
  UV_FS_FDATASYNC,
  UV_FS_UNLINK,
  UV_FS_RMDIR,
  UV_FS_MKDIR,
  UV_FS_MKDTEMP,
  UV_FS_RENAME,
  UV_FS_SCANDIR,
  UV_FS_LINK,
  UV_FS_SYMLINK,
  UV_FS_READLINK,
  UV_FS_CHOWN,
  UV_FS_FCHOWN,
  UV_FS_REALPATH,
  UV_FS_COPYFILE,
  UV_FS_LCHOWN,
  UV_FS_OPENDIR,
  UV_FS_READDIR,
  UV_FS_CLOSEDIR
} uv_fs_type;
```
以上罗列了部分，更多课参考 uv.h（https://github.com/libuv/libuv/blob/v1.x/include/uv.h）  第1089行至1466行