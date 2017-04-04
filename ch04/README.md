# Docker Image 만들기

## bash 기본 명려언

![pyraris에 있는 bash기본 명령어를 따라해봅시당](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04)

기분좋게 한번 따라해봅시당

## DockerFile 작성하기

![pyraris를 보고 dockerFile을 작성해봅시당](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04/02)

## build 명령으로 이미지를 생성해 봅시다.

docker image를 만들때에는 DockerFile 을 이용해야 합니다. 

```
 $ sudo docker build --tag {image_name}:{tag} {dockerfile_path}
```
 - {dockerfile_path} 이 존재하는 경로를 입력해야함.
 -  이미지 이름만 설정하면 태그는 latest로 설정됨.

#### 실행 화면

![sudo docker build](images/sudo_docker_build.png)

## 생성된 이미지를 run 하여 container를 생성해봅시다.


``` bash
 $ sudo docker run --name {container_name} -d -p {host_port}:{container_port} -v {host_dir}:{container_dir} 
```

 - -d => 컨테이너를 백그라운드로 실행한다.
 - -p {host_port}:{container_port} => 컨테이너의 포트와 호스트의 포트를 연결하여 외부에 노출.
 - -v {host_dir}:{container_dir} => 컨테이너의 디렉토리와 호스트의 디렉토리를 연결하여 호스트에서 컨테이너의 디렉토리를 읽습니다.


#### 실행 화면

![sudo docker run](images/sudo_docker_run.png)

![sudo_docker_access](images/nginx.png)

