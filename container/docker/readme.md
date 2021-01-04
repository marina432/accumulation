

### docker(容器外部)常用命令：
1. docker stop #containId/Name
2. docker start #containId/Name
3. docker restart #containId/Name




### 进入docker容器命令
1. docker attach        //exit退出会导致容器终止运行
2. docker exec -it #containId/Name /bin/bash        //exit退出不会导致容器终止运行


### docker(容器内)命令：
1. apt-get update
2. apt-get install vi //安装vi命令  
   api-get install vim  //安装vim命令
3. 