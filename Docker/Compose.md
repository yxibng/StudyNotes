# Compose 模板文件
默认的模板文件文件名称是docker-compose.yml, 格式为YAML.

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```

注意:

- 每个服务都必须通过`image` 指令指定镜像
- 或者通过`build`指令(需要dockerfile)等来自动构建生成镜像
- 如果使用 `build`指令, 在 `Dockerfile`中设置的选项(如: CMD, EXPOSE, VOLUME, ENV)等将会自动被获取,无需在`docker-compose.yml`中再次设置.

## build
	
```yaml
version: '3'
services:
  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
```
- 可以是一个指定了`build context`的字符串

  ```yaml
	version: '3'
	services:
	  webapp:
	    build: ./dir
  ``` 
   
- 可以是一个包含了`context` `Dockerfile` `args`等的对象
	* `context`  指定构建上下文
	* `dockerfile` 指定Dockerfile 文件路径
	* `args` 指定构建镜像时的变量,这些变量只在构建镜像的时候可以被获取
	* `cache_from`指定构建镜像的缓存 This option is new in v3.2
	* `labels` 给构建的镜像添加元数据,推荐reverse-DNS的写法,避免与其它软件使用的元数据冲突. This option is new in v3.3
	
如果同时指定了`image` 和 `build`字段. Compose 过程会将build指定的构建结果,命名为`image`指令所指定的名字:'image:tag'

```yaml
build: ./dir
image: webapp:tag
```
## command
 
 `command` 覆盖容器启动后默认执行的命令
 
 - 覆盖默认的命令</br>
 	
 	```
 	command: bundle exec thin -p 3000
 	```
 - 可以是一个命令列表</br>
	
	```
	command: ["bundle", "exec", "thin", "-p", "3000"]
	```

## container_name
指定容器启动后的名字,替代默认的名字.默认使用`项目名称_服务名称_序号`的格式.

```
container_name: my-web-container
```
> 注意: 指定容器名称之后,改服务无法进行扩展(scale), 因为Docker不允许多个容器具有相同的名称.

## deploy
> Version 3 only

为部署服务指定配置,只在用`docker stack deploy`部署到`swarm`时起作用, 在`docker-compose up`和`docker-compose run`被忽略.

```yaml
version: '3'
services:
  redis:
    image: redis:alpine
    deploy:
      replicas: 6
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
```
 
 - labels
 - mode
	* global   添加一个服务,服务会运行在每个节点.
	* replicated  可以在指定运行服务的数量,给出数字n.
 - replicas 当`mode=replicated`,指定可以服务的数量.
 - resources 指定使用资源的限制,如cpu,memory等.

## depends_on

```yaml
version: '3'
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```

- 启动web,会先启动其依赖,db和redis
- web 服务不会等待 db 和 redis 完全启动之后才启动.
 
## dns

指定dns服务器, 可以是单项或列表

```yaml
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

## dns_search

指定dns搜索域,可以是单项或列表

```yaml
dns_search: example.com
dns_search:
  - dc1.example.com
  - dc2.example.com
```

## 重写默认的entrypoint

```
entrypoint: /code/entrypoint.sh
```

还可以是一个列表

```
entrypoint:
    - php
    - -d
    - zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20100525/xdebug.so
    - -d
    - memory_limit=-1
    - vendor/bin/phpunit
```

## env_file
从文件中添加环境变量,可以是单项或列表

```yaml
env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```
## environment

```yaml
environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SHOW=true
  - SESSION_SECRET
```

- 指定环境变量,可以使用数组或者字典两种格式.
- 如果变量名称或者值中用到了 true | false | yes | no等布尔含义的词汇,最好放到引号里,避免YAML自动解析某些内容为对应的布尔语义.
 
## expose
暴露端口,但不映射到宿主机,只被连接的服务访问.即可以指定内部端口为参数.

```
expose:
 - "3000"
 - "8000"
```

## healthcheck
检查容器是否健康运行.

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 1m30s
  timeout: 10s
  retries: 3
  start_period: 40s
```
`test` 必须是一个string或者一个list.如果是一个list,第一个项目必须是`NONE`,`CMD`或者`CMD-SHELL`中的一个.如果是一个string,等同于指定`CMD-SHELL`

```yaml
# Hit the local web app
test: ["CMD", "curl", "-f", "http://localhost"]

# As above, but wrapped in /bin/sh. Both forms below are equivalent.
test: ["CMD-SHELL", "curl -f http://localhost || exit 1"]
test: curl -f https://localhost || exit 1
```
关闭image默认设置的healthcheck ,可以设置`disable:true`,等同于`test:["NONE"]`.

```
healthcheck:
  disable: true
```

## image

指定启动容器的image. 可以是一个 `repository/tag`  或者是一个部分 `image ID`,如果镜像不存在,Compose将会尝试拉取这个镜像.

```yaml
image: redis
image: ubuntu:14.04
image: tutum/influxdb
image: example-registry.com:4000/postgresql
image: a4bc65fd
```
## labels 
为容器添加元数据信息.例如可以为容器添加辅助说明信息.推荐DNS反写的方式.

```
labels:
  com.example.description: "Accounting webapp"
  com.example.department: "Finance"
  com.example.label-with-empty-value: ""

labels:
  - "com.example.description=Accounting webapp"
  - "com.example.department=Finance"
  - "com.example.label-with-empty-value"
```
## logging
为服务配置日志选项.

```yaml
logging:
  driver: syslog
  options:
    syslog-address: "tcp://192.168.0.42:123"
```

### driver
目前支持三种日志驱动类型.

```
driver: "json-file"
driver: "syslog"
driver: "none"
```
## network_mode
设置网络模式.使用和docker run 的 --network 参数一样的值.

```
network_mode: "bridge"
network_mode: "host"
network_mode: "none"
network_mode: "service:[service name]"
network_mode: "container:[container name/id]"
```

## networks
配置容器连接的网络.

```
services:
  some-service:
    networks:
     - some-network
     - other-network
```

## pid
跟主机系统共享进程命名空间.打开该选项的容器之间,以及容器和宿主机系统之间可以通过进程ID来互相访问和操作.

```
pid: "host"
```

### ports
指定端口映射. `HOST:CONTAINER`,不指定 `CONTAINER `,容器端口会被随机映射(优先选择被expose的端口).

```
ports:
 - "3000"
 - "3000-3005"
 - "8000:8000"
 - "9090-9091:8080-8081"
 - "49100:22"
 - "127.0.0.1:8001:8001"
 - "127.0.0.1:5000-5010:5000-5010"
 - "6060:6060/udp"
```

## volumes
指定数据卷所挂载的路径.

- 挂载一个数据卷到一个特定的服务
- 挂载一个数据卷,被多个服务共享,将volumes放到顶层的定义里.


```yaml
version: "3.2"
services:
  web:
    image: nginx:alpine
    volumes:
      - type: volume
        source: mydata
        target: /data
        volume:
          nocopy: true
      - type: bind
        source: ./static
        target: /opt/app/static

  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  mydata:
  dbdata:
```



1. short syntax
 * host machine (HOST:CONTAINER) 
 * access mode (HOST:CONTAINER:ro).	
 
 ```
 volumes:
  # Just specify a path and let the Engine create a volume
  - /var/lib/mysql

  # Specify an absolute path mapping
  - /opt/data:/var/lib/mysql

  # Path on the host, relative to the Compose file
  - ./cache:/tmp/cache

  # User-relative path
  - ~/configs:/etc/configs/:ro

  # Named volume
  - datavolume:/var/lib/mysql
 ```
2. long syntax
	
	>The long form syntax allows the configuration of additional fields that can’t be expressed in the short form.
  * type :  mount type `volume`, `bind`, `tmpfs`
  * source: 挂载的源,可以是主机上的一个路径,或者是顶层 `volumes` 定义的名字.
  * target : 挂载目标
  * read_only: 设置为只读
  * bind: 
  	 * propagation
  * volume
  	 * nocopy : flag to disable copying of data from a container when a volume is created
  * tmpfs
  	 * size : the size for the tmpfs mount in bytes

	```yaml
	version: "3.2"
	services:
	  web:
	    image: nginx:alpine
	    ports:
	      - "80:80"
	    volumes:
	      - type: volume
	        source: mydata
	        target: /data
	        volume:
	          nocopy: true
	      - type: bind
	        source: ./static
	        target: /opt/app/static
	
	networks:
	  webnet:
	
	volumes:
	  mydata:
	```


