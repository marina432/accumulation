

### docker(容器外部)常用命令：
1. docker stop #containId/Name
2. docker start #containId/Name
3. docker restart #containId/Name
4. docker image inspect redis:latest| grep -i version       //查看镜像版本号
5. docker ps -a      //不加-a，查看的是当前已启动的容器列表，要查看所有已创建的容器列表需加-a




### 进入docker容器命令
1. docker attach        //exit退出会导致容器终止运行
2. docker exec -it #containId/Name /bin/bash        //exit退出不会导致容器终止运行


### docker(容器内)命令：
1. apt-get update
2. apt-get install vi //安装vi命令  
   api-get install vim  //安装vim命令
   > docker容器内按照ps指令：apt-get update && apt-get install -y procps
3. 



### 安装命令：
1. docker run --name elasticsearch -d -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 elasticsearch:7.7.0     //docker安装elasticsearch命令
2. docker run --name elasticsearch-head -p 9100:9100 mobz/elasticsearch-head:5      //docker安装elasticsearch-head命令
3. docker run --name kibana -d --link elasticsearch:elasticsearch -p 5601:5601 kibana:5.6.12    //docker安装kibana命令
   //ps：参考：https://blog.csdn.net/qq_40942490/article/details/111594267
   
