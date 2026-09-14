---
title: "2장 실습 환경 구축"
date: 2020-08-03
weight: 2
---

### 2.1 UNIX/Linux 시스템 구축

- 2.1.1 가상머신 설치
    - x86에 설치되지 않는 UNIX/Linux 버전은 사용이 불가능하다.(IMB의 AIX, HP의 HP-UX, SUN의 SPARC CPU 버전 Solaris 등)
    - VMware 또는 VirtualBox 설치
- 2.1.2 게스트 운영체제 설치
    - iso 형식의 운영체제 이미지를 다운로드 받아 설치한다.

    |구분|운영체제/배포본|웹 사이트 주소|
    |:---|:---|:---|
    |UNIX|solaris|[http://www.oracle.com/kr/index.html](http://www.oracle.com/kr/index.html)|
    ||FreeBSD|[http://www.freebsd.org/](http://www.freebsd.org/)|
    ||NetBSD|[http://www.netbsd.org/](http://www.netbsd.org/)|
    ||OpenBSD|[http://www.openbsd.org/](http://www.openbsd.org/)|
    |Linux|ubuntu|[http://www.ubuntu.com/index_roadshow](http://www.ubuntu.com/index_roadshow)|
    ||fedora|[http://getfedora.org/](http://getfedora.org/)|
    ||CentOS|[http://www.centos.org/](http://www.centos.org/)|
    ||KALI linux|[https://www.kali.org/](https://www.kali.org/)|
    ||SUlinux|[https://www.sulinux.net/2014/](https://www.sulinux.net/2014/)|

    - Linux/ubuntu, 메모리 1GB, 가상 하드디스크 8GB(VDI, 동적할당)으로 가상머신을 할당한다.
    - IDE에 우분투 이미지를 넣고 설치를 진행한다.

### 2.2 터미널 접속 환경 구성

- PuTTY를 사용하여 가상머신에 ssh로 접속을 한다.
- PuTTY의 문자를 UTF-8으로 설정한다.

## 2.3 쉘 스크립트 제작 및 수정 환경 구성

- PUTTY를 설치하고 SSH 접속을 통해 시스템에서 제공하는 에디터를 이용해서 쉘 스크립트를 작성할 수 있지만 Windows 환경의 에디터를 활용할 수 있는 방법이 있다.
- 별도의 에디터를 이용하면 코드를 좀 더 가독성 있게 작성 및 분석할 수 있는 이점이 있다.
- [Acroedit](http://www.acrosoft.pe.kr/board/)에서 본인의 PC 환경에 맞는 버전을 다운로드하여 설치한다.
- [파일] 메뉴에서 [FTP]를 클릭하여 [FTP 열기]를 선택하면 창이 나타나는데 좌측 상단의 [계정관리(Alt+A)] 아이콘을 클릭하면 접속할 FTP 정보를 입력하는 창이 나타난다.
- 호스트 PC에서 쉘 스크립트를 작성하고 저장하면 작성된 쉘 스크립트 파일이 가상머신으로 자동으로 전송된다.
- 이때 가상머신의 시스템에서 FTP 서비스가 구동 중이어야 한다.
- FTP 설정이 완료되면 [서버 접속(Alt+E)] 아이콘을 클릭한다. 가상머신의 서버에 접속하여 파일 및 디렉터리 정보를 가져온다.
- 작업할 파일을 선택하면 해당 파일을 불러오고, 여기서 변경 사항을 수정하여 저장하면 서버에 바로 저장된다.
