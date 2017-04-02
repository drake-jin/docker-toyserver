FROM ubuntu:16.04
MAINTAINER drakejin toyserver dydwls121200@gmail.com git@drake-jin


RUN apt-get update
# Run으로 셸 스크립트 혹은 명령을 실행할 수 있습니다.
RUN apt-get install -y nginx
# 이미지 생성 중에는 사용자 입력을 받을 수 없으므로 apt-get install 에서 -y옵션을 사용시킵니다.
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx  

VOLUME ["/data","/etc/nginx/site-enabled","/var/log/nginx"] 
# 호스트와 공유할 디렉토리 목록

WORKDIR /etc/nginx 
#다음 CMD에서 설정한 실행 파일이 실행될 디렉토리

CMD ["nginx"]
# 컨테이너가 시작되었을 때 실행할 실행 파일 또는 스크립트

EXPOSE 80
EXPOSE 443
# 호스트와 연결할 포트 번호 

