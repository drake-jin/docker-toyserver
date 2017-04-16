FROM ubuntu:14.04
# FROM으로 ubuntu:14.04를 기반으로 이미지를 생성하도록 설정합니다.

RUN apt-get update
RUN apt-get install -y nodejs npm
# RUN으로 nodejs, npm 패키지를 설치합니다.

ADD app.js /var/www/app.js
ADD package.json /var/www/package.json
# ADD로 app.js와 package.json을 이미지의 /var/www 디렉터리에 복사합니다.

WORKDIR /var/www
# WORDIR로 실행 디렉터리를 /var/www로 변경합니다. 그리고 RUN으로 npm install 명령을 실행하여 package.json에 설정된 Node.js 모듈을 설치합니다.
RUN npm install

CMD nodejs app.js
# CMD로 컨테이너가 시작되면 nodejs를 이용하여 app.js를 실행하도록 설정합니다(우분투에서 Node.js를 패키지로 설치하면 실행 파일은 node가 아닌 nodejs입니다).


