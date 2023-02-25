
# Docker install 
snap install docker 



# Docker Container install 
https://velog.io/@titu/Docker-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-3.-Docker-%EC%84%A4%EC%B9%98

```shell
# nginx 이미지 다운로드
$ docker pull nginx

# Docker 이미지 'nginx'를 사용하여 'webserver'라는 이름의 Docker 컨테이너를 기동시킴
$ docker container run --name webserver -d -p 80:80 nginx

위 명령어를 실행했다면, 사용하고 있는 PC가 Nginx의 서버로 작동하고 있는 상태일 것이다.  
[http://localhost:80](http://localhost/) 에 접근해 Nginx의 welcome 화면이 뜨는지 확인해보자.

Nginx 서버의 상태를 도커로 확인하고 싶다면 아래와 같은 명령어를 실행한다.


# Nginx 서버 상태 확인
$ docker container ps

# 컨테이너 가동 확인
$ docker container stats webserver

```shell
# 컨테이너 정지
$ docker stop webserver
```

[[docker-compose]]


## Docker-engine install 

Core Network
```ad-note
The BGP* models are what we looked at in our meeting and show a simple 5 AS topology.

The PMA-274 model is a recent base model for that program that shows a few more techinques like 
the stronswan HAIPE (IPsec) emulation I mentioned.  It is a large model so don't be surprised if 
it has issues with slowness or just starting up in certain environments.
```


## docker copy command 
>docker cp 23c2cbe4ab73:/root/coreparser/hj_001_coretest.xml ./

## docker network command
>docker network ls
>docker network inspect bridge

# docker container name 

```shell
docker run _DOCKER_IMAGE_ --name _CONATINER_NAME
```

# docker mount or volume bind 
ref: https://docs.docker.com/storage/bind-mounts/


Q: Difference between -v(--volume) and --mount behavior? 
A: -v autogenerate file or folder that does not exist. --mount gerates an error

## volume mount command 
examp) --volume option 
> docker run -d -it --name <HJ_DOCKER> -v "$(pwd)"/target:/app <docker_image_name:tag> 

example) --mount option
> docker run -d -it --name <HJ_DOCKER> --mount type=bind,source="$(pwd)"/target,target=/app <docker_image_name:tag>


> docker run -d -p 8080:8080 -p 5900:5900 -v /usr/lib/modules:/usr/lib/modules -v /lib/modules:/lib/modules -v /usr/src/kernel:/usr/src/kernel --privileged --cap-add ALL --name CP820 coreemu-ubuntu:8.2.0 

## check result 
> docker inspect <HJ_DOCKER>


