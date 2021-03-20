# 进程

| 名称  |          意义           |
| ----- | :---------------------: |
| UID   |   执行该进程的用户 id   |
| PID   |        进程编号         |
| PPID  |   该进程的父进程编号    |
| C     | 该进程所在的 CPU 利用率 |
| STIME |      进程执行时间       |
| STIME |   进程相关的终端类型    |
| TIME  |  进程所占用的 CPU 时间  |
| CMD   |    创建该进程的指令     |

> process.nextTick

在 Event loop 的每一个阶段结束后, 直接执行 nextTickQueue 中插入的 "Tick", 并且直到整个 Queue 处理完.

> 标准流

process.stderr, process.stdout 以及 process.stdin

### Child Process

- spawn
- exec() 启动一个子进程来执行命令, 带回调参数获知子进程的情况, 可指定进程运行的超时时间
- execFile() 启动一个子进程来执行一个可执行文件, 可指定进程运行的超时时间
- fork, 加强版的 spawn(), 返回值是 ChildProcess 对象可以与子进程交互

> child.kill & child.send

> 父进程或子进程的死亡是否会影响对方? 什么是孤儿进程?

子进程死亡不会影响父进程, 不过子进程死亡时（线程组的最后一个线程，通常是“领头”线程死亡时），会向它的父进程发送死亡信号. 反之父进程死亡, 一般情况下子进程也会随之死亡, 但如果此时子进程处于可运行态、僵死状态等等的话, 子进程将被进程 1（init 进程）收养，从而成为孤儿进程. 另外, 子进程死亡的时候（处于“终止状态”），父进程没有及时调用 wait() 或 waitpid() 来返回死亡进程的相关信息，此时子进程还有一个 PCB 残留在进程表中，被称作僵尸进程.

# Cluster

基于 child_process.fork() 实现的, 所以 cluster 产生的进程之间是通过 IPC 来通信的, 并且它也没有拷贝父进程的空间, 而是通过加入 cluster.isMaster 这个标识, 来区分父进程以及子进程

```
const cluster = require('cluster');            // | |
const http = require('http');                  // | |
const numCPUs = require('os').cpus().length;   // | |    都执行了
                                               // | |
if (cluster.isMaster) {                        // |-|-----------------
  // Fork workers.                             //   |
  for (var i = 0; i < numCPUs; i++) {          //   |
    cluster.fork();                            //   |
  }                                            //   | 仅父进程执行 (a.js)
  cluster.on('exit', (worker) => {             //   |
    console.log(`${worker.process.pid} died`); //   |
  });                                          //   |
} else {                                       // |-------------------
  // Workers can share any TCP connection      // |
  // In this case it is an HTTP server         // |
  http.createServer((req, res) => {            // |
    res.writeHead(200);                        // |   仅子进程执行 (b.js)
    res.end('hello world\n');                  // |
  }).listen(8000);                             // |
}                                              // |-------------------
                                               // | |
console.log('hello');                          // | |    都执行了
```

# 进程间通信

> 在 IPC 通道建立之前, 父进程与子进程是怎么通信的? 如果没有通信, 那 IPC 是怎么建立的?

在通过 child_process 建立子进程的时候, 是可以指定子进程的 env (环境变量) 的. 所以 Node.js 在启动子进程的时候, 主进程先建立 IPC 频道, 然后将 IPC 频道的 fd (文件描述符) 通过环境变量 (NODE_CHANNEL_FD) 的方式传递给子进程, 然后子进程通过 fd 连上 IPC 与父进程建立连接.

> 什么情况下需要 IPC, 以及使用 IPC 处理过什么业务场景等

# 守护进程

> 守护进程是不依赖终端(tty)的进程, 不会因为用户退出终端而停止运行的进程

```
// 守护进程实现 (C语言版本)
void init_daemon()
{
    pid_t pid;
    int i = 0;

    if ((pid = fork()) == -1) {
        printf("Fork error !\n");
        exit(1);
    }

    if (pid != 0) {
        exit(0);        // 父进程退出
    }

    setsid();           // 子进程开启新会话, 并成为会话首进程和组长进程
    if ((pid = fork()) == -1) {
        printf("Fork error !\n");
        exit(-1);
    }
    if (pid != 0) {
        exit(0);        // 结束第一子进程, 第二子进程不再是会话首进程
                        // 避免当前会话组重新与tty连接
    }
    chdir("/tmp");      // 改变工作目录
    umask(0);           // 重设文件掩码
    for (; i < getdtablesize(); ++i) {
       close(i);        // 关闭打开的文件描述符
    }

    return;
}
```

[Nodejs 编写守护进程](https://cnodejs.org/topic/57adfadf476898b472247eac)
