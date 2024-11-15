## Supervisor

Supervisor 是一个用 Python 编写的进程管理工具，用于管理和监控 Unix 系统上的进程。它能够自动启动、停止和重新启动进程，并且可以用来管理那些需要长时间运行的后台进程，比如 Web 应用服务器、定时任务等。

以下是使用 Supervisor 的基本步骤：

### 安装 Supervisor

在大多数 Linux 发行版中，可以通过包管理器来安装 Supervisor。例如，在 Debian 或 Ubuntu 上，可以使用以下命令：

```bash
sudo apt-get update
sudo apt-get install supervisor
```

对于 CentOS 或 RHEL 系统，可以使用 yum：

```bash
sudo yum install epel-release
sudo yum install supervisor
```

### 配置 Supervisor

安装完成后，Supervisor 的配置文件通常位于 `/etc/supervisord.conf` 或 `/etc/supervisor/supervisord.conf`。你可以直接编辑这个文件，也可以在 `/etc/supervisor/conf.d/` 目录下创建单独的配置文件来定义要管理的程序。

一个简单的程序配置示例如下：

```ini
[program:myapp]
command=/path/to/myapp    ; 启动命令
directory=/var/www/myapp  ; 工作目录（可选）
autostart=true            ; 在 Supervisor 启动时自动启动
autorestart=true          ; 当程序退出时自动重启
stderr_logfile=/var/log/myapp.err.log  ; 错误日志文件路径
stdout_logfile=/var/log/myapp.out.log  ; 标准输出日志文件路径
user=nobody               ; 运行该程序的用户
```

### 控制 Supervisor

- **启动 Supervisor**：
  ```bash
  sudo systemctl start supervisord
  ```
- **重启 Supervisor**：
  ```bash
  sudo systemctl restart supervisord
  ```
- **停止 Supervisor**：
  ```bash
  sudo systemctl stop supervisord
  ```

### 更新配置

当你修改了配置文件后，需要通知 Supervisor 重新加载配置并更新其管理的进程列表：

```bash
sudo supervisorctl reread
sudo supervisorctl update
```

- `reread` 命令会读取新的配置文件或更改的配置文件，但不会对已经存在的进程进行任何操作。
- `update` 命令则会根据新的配置文件启动新添加的程序，并更新已经存在的程序。

### 查看状态

你可以随时查看 Supervisor 管理的所有进程的状态：

```bash
sudo supervisorctl status
```

这将显示每个程序的当前状态，包括是否正在运行、退出代码等信息。

### 查看运行日志

```bash
sudo less /var/log/supervisor/node_manage-stderr---supervisor-4khe3o8_.log
```



### 停止和启动特定程序

你可以通过 Supervisor 的命令行工具来控制单个程序：

- **启动程序**：
  ```bash
  sudo supervisorctl start myapp
  ```
  
- **停止程序**：
  ```bash
  sudo supervisorctl stop myapp
  ```
  
- **重启程序**：
  
  切记不要使用 supervisorctl restart，restart是重启所有项目服务，可能导致整个集群服务挂了
  
  ```bash
  sudo supervisorctl restart myapp
  ```

以上就是使用 Supervisor 的基本指南。希望这些信息对你有所帮助！如果你有更具体的问题或者需要进一步的帮助，请告诉我。



1. 查看监听端口：netstat -tnlp



