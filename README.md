# Docker 镜像 Mongo 企业版

* https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-with-docker/
* https://github.com/docker-library/mongo

## 1. 拉取官方 Dockerfile
> 制作镜像只需要 官方的 Dockerfile 和 docker-entrypoint.sh 两个文件

```bash
export MONGODB_VERSION=4.0
curl -O --remote-name-all https://raw.githubusercontent.com/docker-library/mongo/master/$MONGODB_VERSION/{Dockerfile,docker-entrypoint.sh}
```

修改环境变量，使用代理，解决因为网络的卡顿

## 2. 制作本地镜像

环境变量 DOCKER_USERNAME 设置为 Docker Hub 的用户名

```bash
export DOCKER_USERNAME=heawercher
chmod 755 ./docker-entrypoint.sh
docker build --build-arg MONGO_PACKAGE=mongodb-enterprise --build-arg MONGO_REPO=repo.mongodb.com -t $DOCKER_USERNAME/mongo-enterprise:$MONGODB_VERSION .
```

## 3. 发布到 Dockerhub

```bash
# 查看本地所有镜像
docker images

# Push
docker login
docker push $DOCKER_USERNAME/mongo-enterprise:$MONGODB_VERSION
```

## 4. 使用， 

借鉴 docker-compose.yml

```bash
docker-compose up
```

### replica set 需要初始化
> * https://docs.mongodb.com/manual/tutorial/deploy-replica-set/#initiate-the-replica-set

```bash
docker exec -it mongo_db mongo
```

```js
rs.initiate( {
   _id : "singleton",
   members: [
       { _id: 0, host: "127.0.0.1:27017" },
   ]
})
```