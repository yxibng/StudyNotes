# teamciy

## install through docker

```
docker run -it --name teamcity-server-instance  \
-v <path to data directory>:/data/teamcity_server/datadir \
-v <path to logs directory>:/opt/teamcity/logs  \
-p <port on host>:8111 \
jetbrains/teamcity-server
```
## teamcity agent
1. install from [Zip file distribution](http://localhost:8111/update/buildAgent.zip)
2. Unzip the downloaded file into the desired directory,for example `~/teamcity/buildAgent`
3. edit config file
	
	```
	cd ~/teamcity/buildAgent/conf
	mv buildAgent.dist.properties buildAgent.properties
	```
	in `buildAgent.properties`, edit the `server URL` and `name` of the agent
	
	for more, refer to [Build Agent Configuration](https://confluence.jetbrains.com/display/TCD10/Build+Agent+Configuration)
3. start/stop agent

	```
	cd ~/teamcity/buildAgent/bin/
	./agent.sh start/stop/run/stop force
	```
	
