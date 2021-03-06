이 문서의 저작권은 ![pyrasis.com](http://pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book) 여기에 있습니다.
혼자 공부하려고 글들의 내용을 참고하였습니다. 문제시 삭제하겠습니다. 

# 컨테이너의 필요성

 - 클라우드환경에서 서버는 클릭 몇번으로 만들어 내지만 서버의 환경(설치해야하는 각종 DB 등등..) 을 클릭으로 만들어 낼 수 없었다.
 - 환경은 OS가 무엇이냐에 따라 설치방법이 달라지고.. 새로 구매한 서버에서 운영환경을 만드는것은 고된 작업중 하나..
 - 셸 스크립트로 만들어내는것에도 한계가 있다. 셸 스크립트로 중앙 관리 기능이나 복잡한 관리 기능 구현은 힘듦.
 - 관리해야할 서비스 프로그램이 많아지고.. 설정도 복잡해지고... 
 - 그리고 배포 및 관리를 하는대 서버를 통째로 이미지로 만들어서 올리는것이 좀 더 편리해서 서버를 통째로 샀다가 지우는 경우가 생김
 - 그러므로 이미지 기반 어플리케이션 배포 패러다임이 등장 (Immutable-infrastructure)
 
#### ![Immutable Infrastructure 패러다임](http://blog.nacyot.com/articles/2014-04-06-immutable-infrastructure/)
 - 이미지 기반 어플리케이션 배포 패러다임
 - 한 번 설정하고 변경하지 않는 이미지 기반의 어플리케이션 배포 패러다임을 뜻함.
 - 다수의 서버를 동적으로 관리하는 클라우드를 기반으로 효과적이고 유연하게 배포할 수 있을까에 시작함
 - 기존에 서버를 지속적으로 관리 하는것에 벗어나 어떻게 해야 서버를 잘 쓰고 버리는지가 관건인 패러다임.
 - Heroku, Travis에서 이 페러다임을 적극 채용하고 있으며 docker와 serf 같은 도구들은 이 패러다임의 적용을 돕는다.
 - 호스트 OS와 서비스 운영환경(소스코드, 프로그램, 컴파일된 바이너리)를 분리하고, 한 번 설정한 운영 환경은 변경하지 않음

#### Immutable-infrastructure 장점
 - 관리 용이 : 서비스 운영환경을 이미지로 생성했기 때문에 이미지만 관리하면 됨, docker에서 이미지를 중앙관리 할 것임.
 - 확장성 : 이미지 하나로 서버를 계속 만들어 낼 수 있다. 클라우드 플랫폼의 자동확장 시능과 함께라면 손쉽게 확장할 수 있다.
 - 테스트 : 개발자의 PC나 테스트 서버에서 이미지를 실행하기만 하면 서비스 운영 환경과 동일한 환경이 구성되기 때문에 테스트가 쉽다.
 - 가볍다 : OS에 딸려있는 쓰잘때기 없는 기능들을 안써도 된다.

# 가상머신과 Docker의 컨테이너 

#### 가상머신
 - 가상머신은 완전한 컴퓨터라 항상 *게스트 OS*를 설치해야 한다. 그래서 이미지 안에 OS가 포함되기 때문에 이미지 용량이 커진다.
 - 게스트 OS는 넘나 무겁다. 전 가상화 방식의 느린 속도를 개선하기 위해 반 가상화를 내 놓았지만, 반 가상화라는것도 도찐개찐 무겁당.

#### Docker
 - Docker는 반 가상화보다 경량화된 방식. 게스트 OS를 설치하지 않음. Docker이미지에 서버 운영을 위한 프로그램과 라이브러리를 격리해서 설치할 수 있다.
 - 하드웨어를 가상화하는 계층(Hyper-V)이 없기 때문에 메모리 접근, 파일시스템, 네트워크 속도가 가상머신에 비해 월등히 빠름
 - 메인 호스트와 Docker의 컨테이너 사이의 계층과 성능차이는 크게 발생하지 않는다.

#### 가상머신 vs Docker 비교
![가상머신 vs Docker](http://patg.net/assets/container_vs_vm.jpg)

#### 반가상화(Xen, Hyper-V)
![반 가상화](http://cfile24.uf.tistory.com/image/2514094E52693DA31D9518)

#### chroot와 jail
 - chroot라는 명령을 이용한 방식. 파일 시스템에서 루트 디렉터리를 변경하는 명령
 - chroot는 jail이라는 환경을 생성하는대 chroot jail안에서는 바깥의 파일과 디렉토리에 접근할 수 없다.
 - chroot디렉토리 경로를 격리하기 때문에 서버정보의 유출과 피해를 최소화 하는데 주로 사용함
 - chroot는 jail에 들어갈 실행파일과 공유라이브러리를 직접 준비해야하고 설정방법이 조금 복잡함.
 - 완벽한 가상머신이 아님
 - 이후 리눅스는 LXC(LinuX Container)라는 시스템 레벨의 가상화를 제공했다.
![chroot에 대해 아라보쟈](https://debcairn.files.wordpress.com/2014/08/chrooted_fig1.gif)
 
#### 리눅스 컨테이너-1 (LinuX Container compose of cgroups& namespaces)
 - LXC 는 컴퓨터를 통째로 가상화 하여 OS를 실행하는게 아닌 리눅스 커널레벨에서 제공하는 일종의 격리(isolate)된 가상의 공간.
 - 이 가상의 공간이 바로 컨테이너이다.
 - 리눅스 커널의 Control Groups(cgroups)는 CPU, 메모리, 디스크, 네트워크 자원을 할당하여 완전한 형태의 가상공간을 제공하게 됨
 - 프로세스 트리, 사용자 계졍, 파일 시스템, IPC등을 격리 시킨 호스트와 별개의 공간. 이를 Namespace Isolation(namespaces)라 부름
 - LXC는 격리된 공간을 제공할 뿐, 서비스 운영에 필요한 기능이 부족했다.
![LXC에 대해 알아보자](https://image.slidesharecdn.com/lxcnextgenvirtualizationforcloud-introcloudexpo-140613044826-phpapp01/95/lxc-next-gen-virtualization-for-cloud-intro-cloudexpo-4-638.jpg?cb=1402634985)
 
#### 리눅스 컨테이너-2 (Add libcontainer)
 - 도커 초기에는 LXC를 기반으로 구현했지만 LXC 를 대신하는 libcontainer를 개발하여 사용하고 있다.
 - 내부적으로는 실행드라이버(exec driver)라 한다. 이게 도커
 - libcontainer는 native 표시
 - LXC는 lxc로 표시
![libcontainer](https://cdn-images-1.medium.com/max/1200/1*E8KZJNl21-f1Fb93zgwPWA.jpeg)
![냠냠 ](http://www.publickey1.jp/blog/14/docker09.jpg)

# 이미지와 컨테이너의 차이

#### 이미지 
 - 리눅스 배포판의 유저랜드만 설치된 파일
 - 보통 리눅스 배포판 이름으로 되어있다.
 - Redis나 Nginx등 설치된 베이스 이미지도 있다.
 - 즉, 필요한 라이브러리, 바이너리, 소스코드가 설치된 뒤 파일 하나로 묶인 것.
 - 이미지는 베이스 이미지, 애플리케이션(베이스 이미지가 사용된 것)으로 구분할 수 있다.
 - 실행할 때에는 베이스이미지에서 바뀐 부분(애플리케이션에서 변경된 부분)을 합쳐서 실행함.
 - 유저랜드 = 실행파일 + 라이브러리 
 - 유저랜드 = 리눅스 부팅에 필요한 최소 실행파일과 라이브러리 조합(고유의 패키징 시스템을 포함)
![updated base image ](https://image.slidesharecdn.com/webinarrealworlddocker2014-12-09v31-141209161210-conversion-gate01/95/realworld-docker-10-things-weve-learned-28-638.jpg?cb=1461786358)
![흑](http://pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/10.png)
![Change and Update](https://i.stack.imgur.com/IsRzv.png)
##### 이미지 의존성
![이미지 의존성](http://blog.kollus.com/wp-content/uploads/2015/10/docker_images.gif)

#### 컨테이너
 - 이미지를 실행한 상태.
 - 이미지로 여러개의 컨테이너를 만들 수 있다.
 - 운영체제로 보면 이미지는 실행파일, 컨테이너는 프로세스
 - 즉, Docker는 특정 실행파일 또는 스크립트를 위한 실행 환경

