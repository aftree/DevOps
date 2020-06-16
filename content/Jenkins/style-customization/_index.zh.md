---
date: 2016-04-09T16:50:16+02:00
title: Java微服务Docker构建
weight: 25
---
> 部署微服务组件 - Docker
```bash
Jenkins 插件
* GitLab Plugin
* GitLab Hook Plugin

> 场景：此版本为无镜像仓库，删除上一版镜像，重新生成新的镜像，遇到问题时，无法及时回滚。
echo "###############################当前构建的服务是${PROJECT}################################"
echo "*** Post Steps *** Build/Publish Docker Images & Send files or Execute Commands Over SSH"
image_name=${PROJECT}

let pre_build_number=${BUILD_NUMBER}-1
cur_run_container=$(docker ps -a|grep ${image_name}:v|awk '{print $1}')
im=$(docker images | grep ${image_name} | awk '{print $3}'| grep -v grep | tail -n 1)
if [ "$cur_run_container" != "" ];then
  docker  stop $cur_run_container
  docker rm  $cur_run_container
#  docker  rmi -f ${image_name}:v$pre_build_number
docker  rmi -f $im

# 通过修改tag，来修改docker name:tag,方便及时回滚
# docker tag  
fi
#docker run -dit  $MEMORY --name ${image_name}v${BUILD_NUMBER} -v /data/user:/data/user  -v /data/chinartn-log/${image_name}:/run/target -p $SER_PORT:$SER_PORT  ${image_name}:v${BUILD_NUMBER}
# 创建一个新的容器
docker run -dit   --name ${image_name}v${BUILD_NUMBER} -v /data/user:/data/user  -v /data/chinartn-log/${image_name}:/run/target -p $SER_PORT:$SER_PORT  ${image_name}:v${BUILD_NUMBER}
```

