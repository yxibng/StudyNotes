## docker run

### description

> Run a command in a new container

启动一个新的容器,并执行一些指令.

### Usage 

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

### Options

Name,Shorthand | Default | Description
---------------|---------|------------
--detach , -d	  |         |	Run container in background and print container ID
--interactive , -i | 	|Keep STDIN open even if not attached
--label , -l	| 	|	Set meta data on a container
--name	| |	Assign a name to the container
--restart |	no |	Restart policy to apply when a container exits
--rm | 	| Automatically remove the container when it exits
--tty , -t	| | 	Allocate a pseudo-TTY
--workdir , -w	| |	Working directory inside the container
--volume , -v ||		Bind mount a volume
--read-only	||	Mount the container’s root filesystem as read only
--env , -e	||	Set environment variables
--env-file	||	Read in a file of environment variables
--publish , -p	||	Publish a container’s port(s) to the host
--workdir , -w	||	Working directory inside the container
--volumes-from	||	Mount volumes from the specified container(s)


#### Examples

1. Assign name and allocate pseudo-TTY (--name, -it)
	
	```
	$ docker run --name test -it debian
	```
2. Set working directory (-w)
	
	```
	$ docker  run -w /path/to/dir/ -i -t  ubuntu pwd 
	```
3. Mount volume (-v, --read-only)
	
	- ```$ docker  run  -v `pwd`:`pwd` -w `pwd` -i -t  ubuntu pwd```
	- ```$ docker run -v /doesnt/exist:/foo -w /foo -i -t ubuntu bash```
	- ```$ docker run --read-only -v /icanwrite busybox touch /icanwrite/here```
	- ```$ docker run -t -i -v /var/run/docker.sock:/var/run/docker.sock -v /path/to/static-docker-binary:/usr/bin/docker busybox sh```
   
4. Publish or expose port (-p, --expose)
	
	```bash
	$ docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
	```
	This binds port 8080 of the container to TCP port 80 on 127.0.0.1 of the host machine. 
	
	```bash
	$ docker run --expose 80 ubuntu bash
	```
	This exposes port 80 of the container without publishing the port to the host system’s interfaces.
5. Set environment variables (-e, --env, --env-file)
	
	```bash
	$ docker run -e MYVAR1 --env MYVAR2=foo --env-file ./env.list ubuntu bash
	```
	
	```bash
	$ docker run --env VAR1=value1 --env VAR2=value2 ubuntu env | grep VAR
	VAR1=value1
	VAR2=value2
	```
	
	```bash
	$ cat env.list
	# This is a comment
	VAR1=value1
	VAR2=value2
	USER
	
	$ docker run --env-file env.list ubuntu env | grep VAR
	VAR1=value1
	VAR2=value2
	USER=denis
	```
6. Set metadata on container (-l, --label, --label-file)
	
	Labels are a mechanism for applying metadata to Docker objects, including:
	- Images
	- Containers
	- Local daemons
	- Volumes
	- Networks
	- Swarm nodes
	- Swarm services
	
	> You can use labels to organize your images, record licensing information, annotate relationships between containers, volumes, and networks, or in any way that makes sense for your business or application.
	
	
	> A label is a key=value pair that applies metadata to a container. To label a container with two labels:
	 
	```
	$ docker run -l my-label --label com.example.foo=bar ubuntu bash
	```
	The my-label key doesn’t specify a value so the label defaults to an empty string(""). To add multiple labels, repeat the label flag (-l or --label).

	The key=value must be unique to avoid overwriting the label value. If you specify labels with identical keys but different values, each subsequent value overwrites the previous. Docker uses the last key=value you supply.
	
	Use the --label-file flag to load multiple labels from a file. Delimit each label in the file with an EOL mark. The example below loads labels from a labels file in the current directory:
	
	```
	$ docker run --label-file ./labels ubuntu bash
	```
	
	> ./labels
	
	```
	com.example.label1="a label"

	# this is a comment
	com.example.label2=another\ label
	com.example.label3
	```
7. Mount volumes from container (--volumes-from)

	```
	$ docker run --volumes-from 777f7dc92da7 --volumes-from ba8c0c54f0f2:ro -i -t ubuntu pwd
	```

8. Restart policies (--restart)
	
	>Use Docker’s --restart to specify a container’s restart policy. A restart policy controls whether the Docker daemon restarts a container after exit. Docker supports the following restart policies:
	
	Policy | Result
	-------|-------
	no | Do not automatically restart the container when it exits. This is the default.
	on-failure[:max-retries] | Restart only if the container exits with a non-zero exit status. Optionally, limit the number of restart retries the Docker daemon attempts.
	unless-stopped | Restart the container unless it is explicitly stopped or Docker itself is stopped or restarted.
	always | Always restart the container regardless of the exit status. When you specify always, the Docker daemon will try to restart the container indefinitely. The container will also always start on daemon startup, regardless of the current state of the container.
	
	This will run the redis container with a restart policy of always so that if the container exits, Docker will restart it.
	