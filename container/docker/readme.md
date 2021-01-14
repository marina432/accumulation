

### docker(容器外部)常用命令：
1. docker stop #containId/Name
2. docker start #containId/Name
3. docker restart #containId/Name
4. docker image inspect redis:latest| grep -i version       //查看镜像版本号
5. docker ps -a      //不加-a，查看的是当前已启动的容器列表，要查看所有已创建的容器列表需加-a
6. 




### 进入docker容器命令
1. docker attach        //exit退出会导致容器终止运行
2. docker exec -it #containId/Name /bin/bash        //exit退出不会导致容器终止运行


### docker(容器内)命令：
1. apt-get update
2. apt-get install vi //安装vi命令  
   api-get install vim  //安装vim命令
3. 