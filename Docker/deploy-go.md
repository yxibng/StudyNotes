1. Dockerfile
	1. From golang AS build
		- 将go工程拷贝到容器内部
		- 指定工作目录 /go
		- 编译go工程,生成可执行文件,命名为server
	2. From alpine
		- 一些准备工作,软件更新,配置环境变量等
		- 指定工作目录/app
		- 将上个阶段的构建产物server拷贝到工作目录
		- 指定ENTRYPOINT,默认启动server可执行文件
		- 暴露端口8080	
2. compose-file

