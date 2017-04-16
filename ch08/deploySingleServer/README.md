# 서버 한대에서 배포하는 간단한 방법

1. 개발자 PC에서 애플리케이션을 개발함.
2. git push 명령으로 소스를 서버에 올린다.
3. 서버에서는 저장소에 git push 명령이 발생하면 git hook을 실행시킨다.
4. git hook에서 Docker 이미지를 생성하고, 이미지 컨테이너로 실행시킨다.

# deploySingleServer

 0. git 설치 및 project init 
 1. ![nodejs 앱 만들기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter08/01/02)
 2. ![dockerfile 만들기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter08/01/03)
 3. ![개발자 PC에서 SSH키 생성하기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter08/01/04)
 4. ![서버에 Git 설치 및 저장소 생성하기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter08/01/05)
  - /home/likemilk/{work_dir}/.git/hooks 에는 git 소스에서 발생되는 모든일을 trigger할 수 있는 실행파일이 존재한다. 
 







