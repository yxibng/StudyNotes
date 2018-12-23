# Dockerfile reference
Dockerfile 定义了构建镜像的过程. 通过执行`docker build`用户可以自动化执行一系列的指令来构建镜像.

## Usage
> The docker build command builds an image from a `Dockerfile` and a `context`. The build’s context is the set of files at a specified location `PATH` or `URL`.The `PATH` is a directory on your local filesystem. The URL is a Git repository location.
>
>A context is processed recursively. So, a `PATH` includes any subdirectories and the URL includes the repository and its submodules. 


两个关键的因素:

- Dockerfile
- Context
	- 可以是PATH,一个目录,会递归它的子目录
	- 可以是URL,指向一个Git仓库,会递归Git仓库的子模块

注意:

- build命令是由Docker daemon运行的,而不是CLI. 
- 一个构建过程最先做的就是把context(递归遍历)发送到daemon.
- 最好把context指定为一个空的目录,并且把Dockerfile放到这个目录.只添加那些Dockerfile构建所需的文件.

> 千万不要指定`context`为根目录`/`,这会造成构建过程将整硬盘上的整个文件系统拷贝到Docker deamon
	
1. 指定Dockerfile路径,指定context问当前目录`.`
	
	```
	$ docker build -f /path/to/a/Dockerfile .
	```
2. 指定构建成功后保存的`repository`和`tag`,通过`-t`来指定
	
	```
	$ docker build -t shykes/myapp .
	```
3. 指定多个`repositories`,用多个`-t`
	
	```
	$ docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
	```

## Fromat

Dockerfile基本格式:

```
# Comment
INSTRUCTION arguments
```

Instructions list:

- ADD
- COPY
- ENV
- EXPOSE
- FROM
- LABEL
- STOPSIGNAL
- USER
- VOLUME
- WORKDIR

### From

```docker
FROM <image> [AS <name>]
```
Or

```docker
FROM <image>[:<tag>] [AS <name>]
```
Or

```docker
FROM <image>[@<digest>] [AS <name>]
```
> `ARG` is the only instruction that may precede FROM in the Dockerfile

#### Understand how ARG and FROM interact
> FROM instructions support variables that are declared by any ARG instructions that occur before the first FROM.

```docker
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```
> An ARG declared before a FROM is outside of a build stage, so it can’t be used in any instruction after a FROM. To use the default value of an ARG declared before the first FROM use an ARG instruction without a value inside of a build stage:

```docker
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```
### RUN
> The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.

基于当前的镜像,在新的一层执行命令,提交成结果镜像,这个结果镜像将在后续的构建过程中被使用.

个人理解:一些准备工作,安装软件,配置环境等.


- ```RUN  <command> (shell form, the command is run in a shell, which by default is `/bin/sh -c` on Linux)```
- ```RUN  ["executable", "param1", "param2"] (exec form)```

1. 每执行一个RUN语句产生一层新的layer,所以尽可能把多个RUN合为一个去执行,这样会减少产生的layer的层次.
2. shell form,可以使用`\`来换行

	```docker
	RUN /bin/bash -c 'source $HOME/.bashrc; \
	echo $HOME'
	```
	相当于
	
	```docker
	RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
	```
3. exec form
	
	>The exec form is parsed as a JSON array, which means that you must use double-quotes (“) around words not single-quotes (‘).

	```docker
	RUN ["/bin/bash", "-c", "echo hello"]
	```
	> Note: Unlike the shell form, the exec form does not invoke a command shell. This means that normal shell processing does not happen. For example, `RUN [ "echo", "$HOME" ]` will not do variable substitution on `$HOME`. If you want shell processing then either use the shell form or execute a shell directly, for example: RUN `[ "sh", "-c", "echo $HOME" ]`. When using the exec form and executing a shell directly, as in the case for the shell form, it is the shell that is doing the environment variable expansion, not docker.

### CMD

> The main purpose of a `CMD` is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an `ENTRYPOINT` instruction as well.

Docker不是虚拟机,容器就是进程.既然进程,在启动容器的时候,需要指定所运行的程序及参数.CMD指令就是指定默认的容器主进程的启动命令的.

- `CMD ["executable","param1","param2"] (exec form, this is the preferred form)`
- `CMD ["param1","param2"] (as default parameters to ENTRYPOINT)`
- `CMD command param1 param2 (shell form)`

注意:

1. 一个Dockerfire里面只能有一个CMD, 如果列举了多个CMD,只有最后一个会起作用.
2. CMD的主要目的是用于指定默认的容器主进程的启动命令.
3. 如果CMD被用作给ENTRYPOINT提供默认参数, CMD 和 ENTRYPOINT 都必须被指定为 json arary 类型
4. exec form 被解析成json array, 所以必须使用`""`而不是用`''`
5. 与shell form不同,exec form 并不会调用shell 命令.
	
	- `CMD ["echo", "$HOME"] `并不会输出$HOME替换后的结果
	- `CMD [ "sh", "-c", "echo $HOME" ]` 或者 `CMD echo $HOME` 才可以

	shell from
	
	```docker
	CMD echo $HOME
	```
	实际执行过程,回变更为exec form(用双引号,会被解析成json数组)
	
	```docker
	CMD ["sh","-c","echo $HOME"]
	```
6. 可以在docker run的时候指定新的命令来提换这个命令
	
	- ubuntu 默认的 CMD 是 /bin/bash. 如果直接执行 `docker run -it ubutntu` 会直接进入 bash
	- 可以指定为其他的命令 `docker run -it ubuntu cat /etc/os-release`. 这样我们就用 `cat /etc/os-release` 命令替换了 默认的 `/bin/bash` 命令,输出了系统版本信息.
	
	


### ENTRYPOINT
格式如下:

- ```ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)```
- ```ENTRYPOINT command param1 param2 (shell form)```

1. exec form, json array style
   
   ```
   ENTRYPOINT ["top", "-b", "-H"]
   ```
	   
	任何docker run设置的命令参数或 CMD 指定的命令, 都将作为ENTRYPOINT指令的参数,追加到ENTRYPOINT指定的命令的后面
	`docker run <container_name> -v`, 则容器启动执行的命令为 `top -b -H -v`. 而`-v` 为top的追加参数.
2. shell 格式
	
	```
	ENTRYPOINT top -b -H
	```
	这种格式禁止追加任何参数,即CMD指令或 docker run ...<command>后的参数都将被忽略. </br>
	采用shell格式,在容器执行时,自动调用shell, 自动在命令前追加`/bin/sh -c`. 上述指令更改为 `/bin/sh top -b -H`
	这样，ENTRYPOINT指令设置的top命令就不是容器中的第一个进程PID 1，这样在容器停止的时候就无法收到系统的SIGTERM信号。
   要想收到SIGTERM信号，务必使用exec命令，定义ENTRYPOINT指令如下：
	
	```
   ENTRYPOINT exec top -b -H
   ```
#### 比较ENTRYPOINT与CMD指令
ENTRYPOINT指令，往往用于设置容器启动后的第一个命令，这对一个容器来说往往是固定的。
CMD指令，往往用于设置容器启动的第一个命令的默认参数，这对一个容器来说可以是变化的。
docker run <command>往往用于给出替换CMD的临时参数。 
  
| | NO ENTRYPOINT | ENTRYPOINT exec_entry p1_entry | ENTRYPOINT [“exec_entry”, “p1_entry”]
| ---| --- | --- | ---
No CMD	 | error, not allowed | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry
CMD [“exec_cmd”, “p1_cmd”]	 | exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry exec_cmd p1_cmd
CMD [“p1_cmd”, “p2_cmd”] |  p1_cmd p2_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry p1_cmd p2_cmd
CMD exec_cmd p1_cmd	 | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entr  | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd

### COPY

- `COPY [--chown=<user>:<group>] <src>... <dest>`
- `COPY [--chown=<user>:<group>] ["<src>",... "<dest>"] (this form is required for paths containing whitespace)`

1. `<src>` 如果不指定为绝对路径, 默认为 docker build 所指定的 context 目录的相对路径.
2. `<src>`可以使用模式匹配 
	
	```docker
	COPY hom* /mydir/        # adds all files starting with "hom"
	COPY hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt"
	```
3. <dest> 为容器内的绝对路径,或者是 `WORKDIR `的相对路径.
	
	```docker
	COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
	COPY test /absoluteDir/  # adds "test" to /absoluteDir/
	```
4. 如果`<src> `的目录或者文件包含特殊字符如`[` `]`, 需要遵从Golang的规则,防止被解析成匹配模式.
	例如拷贝 `arr[0].txt`
	
	```docker
	COPY arr[[]0].txt /mydir/    # copy a file named "arr[0].txt" to /mydir/
	```
5. 接受一个flag `--from=<name | index>` 指定之前某个阶段的构建产物作为`<src>`
	* 通过(`FROM .. AS <name>`)定义的`name`
	* 通过`From`定义的 `index`


### ADD
 - `ADD [--chown=<user>:<group>] <src>... <dest>`
 - `ADD [--chown=<user>:<group>] ["<src>",... "<dest>"] (this form is required for paths containing whitespace)`

与COPY区别

 - `<src>` 可以是一个URL,下载的如果是tar,会自动解压
 - ADD 指令会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢
 - 推荐COPY, 语义很明确,就是复制
 - 所有的文件复制均使用 COPY 指令，仅在需要自动解压缩的场合使用 ADD。

### VOLUME
 - `VOLUME ["/data"]`

容器运行时,应该尽量保持容器存储层不发生写操作,对于数据库等需要保存动态数据的应用,其数据库文件应该保存与卷(volume)中.</br>
为了防止运行时用户忘记将动态文件所保存目录挂载为卷,在Dockerfile中,我们可以事先指定某些目录挂载为匿名卷,这样运行时如果用户不指定挂载,其应用也可以正常运行,不会向存储层写入大量的数据.

```
VOLUME ["/data"]
```
这里的/data目录就会在运行时自动挂载为匿名卷,任何向/data中写入的信息都不会记录进容器存储层,从而保证了容器存储层的无状态化. </br>

运行时可以覆盖这个挂载设置.下面的命令,使用mydata 这个命名卷挂载到了/data这个位置,代替了Dockerfile中定义的匿名卷的挂载配置.

```docker
docker run -d -v mydata:/data xxx
```

### EXPOSE

- `EXPOSE <port> [<port>/<protocol>...]`

声明运行时容器提供服务端口,只是声明,运行时,不会因为这个声明就开启这个端口的服务.好处

- 帮助镜像使用者理解这个镜像服务的守护端口,方便配置映射
- 运行时使用了随机端口映射, `docker run -P`, 会自动随机映射 EXPOSE的端口.

### WORKDIR
语法

``` docker
WORKDIR /path/to/workdir
```

- 为紧随其后的 RUN, CMD, ENTRYPOINT, COPY, ADD等提供工作空间.
- 可以多次指定,用来切换工作空间.

### ENV

语法
```
ENV <key> <value>
ENV <key>=<value> ...
```

设置容器默认的环境变量, 可以通过`docker run --env <key>=<value>`来修改对应的环境变量

### ARG

语法

```
ARG <name>[=<default value>]
```

构建参数和 ENV 的效果是一样的,都是设置环境变量.不同的是,ARG 设置的是构建环境的环境变量,将来容器运行时是不会存在的.
不要用 ARG保存密码之类的信息, 因为 `docker history` 还是可以看到所有值的.

Dockerfile 中的ARG 指令是定义参数名称及其默认值. 该默认值可以在构建命令`docker build` 中 用 `--build-arg <参数名>=<值>` 来覆盖.

```
1 FROM busybox
2 USER ${user:-some_user}
3 ARG user
4 USER $user
...
```

docker build

```
$ docker build --build-arg user=what_user .
```


