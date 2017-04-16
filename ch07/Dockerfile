FROM ubuntu:14.04
#From 명령이 수행되지 않으면 이미지가 생성되지 않습니다.
#FROM은 반드시 작성해주어야만 합니다.
# FROM 에 작성된 이미지가 로컬에 있다면 로컬에 있는걸 쓰고 없으면 docker hub 에서 받아옵니다.

MAINTAINER Cho, Yongjin <dydwls121200@gmail.com>
# 이미지를 생성한 사람의 정보를 설정합니다. 형식은 자유이고 보통은 이름과 이메일을 입력합니다.
# MAINTAINER는 생략할 수 있습니다.

RUN apt-get install -y nginx
RUN echo "Hello Docker" > /tmp/hello
RUN curl -sSL https://golang.org/dl/go1.3.1.src.tar.gz | tar -v -C /usr/local -xz
RUN git clone https://github.com/docker/docker.git
# 셸 스크립트 구문을 사용할 수 있습니다.
# FROM으로 설정한 이미지에 포함된 /bin/sh 실행파일을 사용하게 되며 /bin/sh실행 파일이 없으면 사용할 수 없습니다. 

# FROM 으로 설정한 이미지의 /bin/sh 실행파일을 사용하지 않는 방식. 셸 스크립트 문법이 인식되지 않으므로 셸 스크립트 문법과 관련된 문자를 그대로 실행파일에 넘겨줄 수 있습니다.

# RUN으로 실행한 결과는 캐시되며 다음 빌드 때 재사용된다. 캐쉬된 결과를 사용하지 않으려면 docker build명령에서 --no-cache 옵션을 사용하면 된다.

CMD touch /home/hello/hello.txt
# 셸 이용해서 인자주기 
CMD ["redis-server", "help"] 
# 셸 없이 실행하기

ENTRYPOINT ["echo"]
# 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행한다.
# 1. docker run 명령으로 컨테이너를 생성하거나 
# 2. docker start 명령으로 정지된 컨테이너를 시작할 때 실행된다.

# CMD와 ENTRYPOINT 의 차이.
# docker run 명령할 때 동작방식이 다르게 되는데
# run 명령에서 실행할 파일을 설정하면 CMD는 무시하게 됨.
# ENTRYPOINT 는 run명령에 붙은 부가적인 매개 변수가 ENTRYPOINT의 배열에 추가된다.
# $ sudo docker run --entrypoint="cat" example /etc/hostname
# --entrypoint를 설정하면 Dockerfile에 설정한 ENTRYPOINT의 내용이 무시됩니다.

 

CMD ["hello"]
# dockerfile에 Entrypoint가 있으면 CMD는 ENTRYPOINT에 매개 변수만 전달하는 역할로 변함.


EXPOSE 80 
EXPOSE 443
# docker run --expose {host_port}:{container_port} 명령과 같습니다.
# 컨테이너의 공개할 포트를 지정합니다.
# 포트를 외부에 노출하려면 docker run 명령의 -p, -P 옵션을 이용해야한다.

ENV GOPATH /go
ENV PATH /go/bin:$PATH
# 컨테이너의 환경변수를 정합니다.
# RUN CMD ENTRYPOINT 에 적용됩니다.
# 환경변수를 이용할 때에는 $ 를 사용하면 됩니다.
# sudo docker run [--env, -e] PATH=/go/bin:$PATH ......

RUN wget http://zlib.net/zlib-1.2.8.tar.gz -O - | tar -xz

ADD http://zlib.net/zlib-1.2.8.tar.gz / 
# ADD 파일을 이미지에 추가한다.
# ADD {copy_file} {paste_path_in_container}

COPY *.txt /root/
# COPY는 파일을 이미지에 추가한다. ADD와 달리COPY는 압축파일을 추가할 때 압축을 해제하지 않고,
# 파일 file URL도 사용할 수 없다. 

# VOLUME 디렉터리의 내용을 컨테이너에 저장하지 않고 호스트에 저장하도록 설정.
VOLUME /data
VOLUME ["/data", "/var/log/hello"]
# 단 VOLUME명령으로는 Host의 특정 디렉토리와 연결할 수 없다.
# 데이터 볼륨을 특정 호스트와 연결하고 싶다면 
# $ sudo docker run -v /root/data:/data {image_name} 
# -v옵션을 달아주어야만 한다.

USER someone
# USER는 새로운 리눅스 사용자를 생성합니다. 
# USER 뒤에 오는 모든 RUN, CMD, ENTRYPOINT에 적용되며, 중간에 다른 사용자를 설정하여 사용자를 바꿀 수 있습니다.

USER nobody
RUN touch /tmp/hello.txt

USER root
RUN touch /hello.txt
ENTRYPOINT /hello-entrypoint.sh
# 처음에는 nobody 계정으로 /tmp/hello.txt 파일을 생성합니다. 그 다음부터는 root 계정으로 /hello.txt 파일을 생성하고(/에는 root 계정만 파일을 생성할 수 있으므로), /hello-entrypoint.sh 파일을 실행합니다.

WORKDIR /var/www
# work dir은 상대경로로도 지정할 수 있습니다. workdir은 실행디렉터리를 바꿉니다.
# docker image 는 기본적으로 workdir설정을 하지 않으면 /var디렉토리에 기본값으로 생기게 되는데
# 이를 위한 workdir설정입니다. 

ONBUILD RUN touch /hello.txt
# ONBUILD는 생성한 이미지를 기반으로 다른 이미지가 생성될 때 
# 명령을 실행(trigger)합니다. 즉. 이 이미지가 상속되어 새로운 이미지가 만들어 질 때... 
# 최초에 ONBUILD를 설정하면 아무 명령도 실행되지 않지만 다음의 FROM 이 실행될 때 
# ONBUILD 이하의 명령이 실행됩니다.

# docker inspect 명령으로 이미지의 ONBUILD 설정을 확인할 수 있습니다.
# $ sudo docker inspect -f "{{ .ContainerConfig.OnBuild }}" example
# [RUN touch /hello.txt]

















