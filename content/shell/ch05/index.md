---
title: "5장 시스템 관리 쉘 스크립트"
date: 2020-07-24
weight: 5
---

### 5.1 디스크 사용량 분석/보고

- 5.1.1 쉘 스크립트 코드
    ```bash
    #!/bin/bash

    # 점검 용량 MB 단위 설정
    SETSIZE=10
    EXMB=`expr $SETSIZE\*\(1024\*1024\)`

    # UID가 500 이상인 계정을 구분한다.
    for name in $(cut -d: -f1,3 /etc/passwd | awk -F: '$2 > 499 {print $1}')
    do
        echo "사용자 $name의 $SETSIZE MB 초과 파일 목록 / 용량"
        find /user/tmp/home -user $name -type f -ls | awk "\$7 > $EXMB" | awk '{print "경로:" $11, "/ 용량:" $7}'
        echo ""
    done
    exit
    ```
- 5.1.2 실행 결과
    ```
    $ ./5-1.sh
    사용자 nobody의 10 MB 초과 파일 목록 / 용량

    사용자 ubuntu의 10 MB 초과 파일 목록 / 용량

    사용자 user01의 10 MB 초과 파일 목록 / 용량

    사용자 user02의 10 MB 초과 파일 목록 / 용량

    ```
- 5.1.3 코드 분석
    - 점검할 용량을 MB 단위로 설정하는 부분, 이후 점검 파일과 비교하기 위해 expr 명령어를 이용해 바이트 단위로 변환한다.
        ```sh
        SETSIZE=10
        EXMB=`expr $SETSIZE\*\(1024\*1024\)`
        ```
    - /etc/passwd 에서 UID가 500 이상인 계정을 대상으로 cut 명령어를 이용하여 첫 번째 필드와 세 번째 필드를 구분한다.
        ```sh
        for name in $(cut -d: -f1,3 /etc/passwd | awk -F: '$2 > 499 {print $1}')
        ```
    - name에 할당된 계정 소유자가 소유한 파일의 용량을 비교하여 설정된 용량보다 큰 파일의 경로와 세부 용량을 표시한다.
        ```sh
        echo "사용자 $name의 $SETSIZE MB 초과 파일 목록 / 용량"
        find /user/tmp/home -user $name -type f -ls | awk "\$7 > $EXMB" | awk '{print "경로:" $11, "/ 용량:" $7}'
        ```

### 5.2 사용자 계정 일시 정지

- 5.2.1 쉘 스크립트 코드
    ```sh
    #!/bin/sh

    stime=90 		
    ami=`whoami`

    if [ "$ami" != "root" ]; then
        echo "★본 프로그램은 체계관리자(ROOT)외 사용자는 실행이 제한됩니다. ★"
        exit
    fi

    # $1 not exist
    if [ -z $1 ]
        then
            echo "usage : 5-2.sh username"
            exit
    fi

    echo "◆ $1의 사용자 계정을 일시정지합니다. a ~ d 단계로 진행합니다."
    echo ""
    echo " a $1 계정의 패스워드를 변경해주세요"
    echo ""
    passwd $1

    tty=`who | grep $1 | tail -1 | awk '{print $2}'`

    if [ -z $tty ]
        then echo ""
        else
            cat << EOF > /dev/$tty
    *************************************************************
    ★경   고 ★
    지금 사용하고 있는 계정은 시스템관리자에게 의해 일시정지되니
    90초 이후에는 강제 Log Out 됩니다.
    지금하는 작업을 마무리하시고 Log Out 바랍니다.

    계정사용 연장 및 기타문의사항은 시스템관리부서에 문의하세요
                        ☎053-972-1234~8
    *************************************************************
    EOF
            sleep $stime
    fi

    killall -s HUP -u $1
    sleep 1
    killall -s KILL -u $1
    echo " b $1의 사용중인 모든 프로세스는 종료되었습니다."
    echo " c 현 시각부로 계정 $1 은 Log Out 되었습니다. "
    echo "                                    - $(date) - "

    chmod 000 /home/$1
    echo " d $1 계정은 일시 정지 처리되었습니다."
    exit
    ```
- 5.2.2 실행 결과
    ```
    $ ./5-2.sh ubuntu
    ◆ ubuntu의 사용자 계정을 일시정지합니다. a ~ d 단계로 진행합니다.

    a ubuntu 계정의 패스워드를 변경해주세요

    Enter new UNIX password:
    Retype new UNIX password:
    passwd: password updated successfully

    b ubuntu의 사용중인 모든 프로세스는 종료되었습니다.
    c 현 시각부로 계정 ubuntu 은 Log Out 되었습니다.
                                        - Fri Jul 24 05:13:53 UTC 2020 -
    d ubuntu 계정은 일시 정지 처리되었습니다.
    ```
- 5.2.3 코드 분석
    ```sh
    #!/bin/sh

    # 슬립 타임을 설정한다.
    stime=90
    # 현재 사용자를 알아온다.
    ami=`whoami`

    # 계정 일시 정지는 root 사용자만 가능하도록 한다.
    if [ "$ami" != "root" ]; then
        echo "★본 프로그램은 체계관리자(ROOT)외 사용자는 실행이 제한됩니다. ★"
        exit
    fi

    # 인자가 없으면 usage를 나타낸다.
    if [ -z $1 ]
        then
            echo "usage : 5-2.sh username"
            exit
    fi

    echo "◆ $1의 사용자 계정을 일시정지합니다. a ~ d 단계로 진행합니다."
    echo ""
    echo " a $1 계정의 패스워드를 변경해주세요"
    echo ""
    # 비밀번호 변경을 위한 작업
    # user가 존재하지 않는 경우는 배제한다.
    passwd $1

    # 프롬프트는 1개 열려 있다는 가정에서 열려있는 프롬프트에 접근한다.
    tty=`who | grep $1 | tail -1 | awk '{print $2}'`

    if [ -z $tty ]
        # 열려있는 프롬프트가 없다면
        then echo ""
        # 열려있는 프롬프트가 있다면 알림을 보내준다.
        else
            cat << EOF > /dev/$tty
    *************************************************************
    ★경   고 ★
    지금 사용하고 있는 계정은 시스템관리자에게 의해 일시정지되니
    90초 이후에는 강제 Log Out 됩니다.
    지금하는 작업을 마무리하시고 Log Out 바랍니다.

    계정사용 연장 및 기타문의사항은 시스템관리부서에 문의하세요
                        ☎053-972-1234~8
    *************************************************************
    EOF
            sleep $stime
    fi

    # 프롬프트 프로세스를 종료시킨다.
    killall -s HUP -u $1
    sleep 1
    killall -s KILL -u $1
    echo " b $1의 사용중인 모든 프로세스는 종료되었습니다."
    echo " c 현 시각부로 계정 $1 은 Log Out 되었습니다. "
    echo "                                    - $(date) - "

    # home path에 접근하지 못하도록 계정을 비활성화 한다.
    chmod 000 /home/$1
    echo " d $1 계정은 일시 정지 처리되었습니다."
    exit
    ```
### 5.3 guest 및 공용 계정 초기화

- 5.3.1 쉘 스크립트 코드
    ```sh
    #!/bin/sh

    sampledir="/tmp/sample/"
    ami=`whoami`

    if [ "$ami" != "root" ]; then
        echo "★본 프로그램은 체계관리자(ROOT)외 사용자는 실행이 제한됩니다. ★"
        exit
    fi

    # /etc/passwd에서 user로 시작하는 계정을 추출한다.
    for name in $(cat /etc/passwd | awk -F: '/^user/{print $1}')
    do  
        cd /home/$name
        rm -r ./* 
        cp -rp /$sampledir /home/$name/

        echo " ■사용자 $name 의 홈디렉토리 초기화 완료"
        echo ""
    done 
    exit 
    ```
- 5.3.2 실행 결과
    ```
    $ ./5-3.sh
    ■사용자 B311148 의 홈디렉토리 초기화 완료

    ■사용자 B311146 의 홈디렉토리 초기화 완료

    ```
- 5.3.3 코드 분석
    ```sh
    #!/bin/sh

    # 기본값으로 구성되어 있는 초기 상태의 user
    # 없다면 새로 생성해야 한다.
    sampledir="/tmp/sample/"
    # 현재 사용자를 알기 위해
    ami=`whoami`

    # root 유저만 초기화할 수 있도록 설정한다.
    if [ "$ami" != "root" ]; then
        echo "★본 프로그램은 체계관리자(ROOT)외 사용자는 실행이 제한됩니다. ★"
        exit
    fi

    # /etc/passwd에서 B로 시작하는 계정을 추출한다.
    for name in $(cat /etc/passwd | awk -F: '/^B/{print $1}')
    do  
        cd /home/$name
        rm -r ./* 
        cp -rp /$sampledir /home/$name/

        echo " ■사용자 $name 의 홈디렉토리 초기화 완료"
        echo ""
    done 
    exit 
    ```

### 5.4 서버의 네트워크 상태 감시

- 5.4.1 쉘 스크립트 코드
    - 5-4.sh
        ```sh
        #!/bin/sh

        serIP="/test/ch_5/serverIP.lst"

        # /test/ch_5/serverIP.lst에서 서버의 IP를 추출한다.
        for ip in $(cat $serIP | awk -F: '{print $2}')
        do  
            sername=`grep $ip $serIP | awk -F: '{print $1}'`

            if ! ping -c 2 $ip >> /dev/null 
                then   
                    echo ""
                    echo " ★ $sername 서버 또는 네트워크 접속 제한 : 점검요망 ★"
                    
                    grep $sername $serIP >> /test/ch_5/Server_chk_err_`date +%C%y%m%d`.log
            fi
        done 
        exit 
        ```
    - serverIP.lst
        ```
        WAS:192.168.3.128
        DB_1:192.168.3.129
        DB_2:192.168.3.120
        ftp:192.168.3.131
        mail:192.168.3.132
        ```
- 5.4.2 실행 결과
    ```
    $ ./5-4.sh

    ★ WAS 서버 또는 네트워크 접속 제한 : 점검요망 ★

    ★ DB_1 서버 또는 네트워크 접속 제한 : 점검요망 ★

    ★ DB_2 서버 또는 네트워크 접속 제한 : 점검요망 ★

    ★ ftp 서버 또는 네트워크 접속 제한 : 점검요망 ★

    ★ mail 서버 또는 네트워크 접속 제한 : 점검요망 ★
    ```
    - ping 점건 결과 이상이 있는 시스템을 화면에 표시하고, 점건이 필요한 서버를 Server_chk_err_날짜.log 파일에 저장한다.
    - `서버이름:서버IP` 형식으로 log 파일에 저장된다.
- 5.4.3 코드 분석
    ```sh
    #!/bin/sh

    # 형식에 맞게 파일이 작성되어있어야 한다.
    serIP="/test/ch_5/serverIP.lst"

    # /test/ch_5/serverIP.lst에서 서버의 IP를 추출한다.
    for ip in $(cat $serIP | awk -F: '{print $2}')
    do  
        sername=`grep $ip $serIP | awk -F: '{print $1}'`

        if ! ping -c 2 $ip >> /dev/null 
            then   
                echo ""
                echo " ★ $sername 서버 또는 네트워크 접속 제한 : 점검요망 ★"
                
                grep $sername $serIP >> /test/ch_5/Server_chk_err_`date +%C%y%m%d`.log
        fi
    done 
    exit 
    ```

### 5.5 서비스 프로세스 상태 점검

- 5.5.1 쉘 스크립트 코드 a
- 5.5.2 실행 결과 a
- 5.5.3 코드 분석 a
- 5.5.4 쉘 스크립트 코드 b
- 5.5.5 실행 결과 b
- 5.5.6 코드 분석 b

### 5.6 특정 디렉터리의 파일 내용 일괄 수정하기

- 5.6.1 쉘 스크립트 코드
- 5.6.2 실행 결과
- 5.6.3 코드 분석

### 5.7 지정된 날짜의 웹 접속 통계

- 5.7.1 쉘 스크립트 코드
- 5.7.2 실행 결과
- 5.7.3 코드 분석

### 5.8 점검 결과를 메일로 보고

- 5.8.1 쉘 스크립트 코드
- 5.8.2 실행 결과
- 5.8.3 코드 분석

### 5.9 ftp를 이용한 파일 전송 자동화

- 5.9.1 쉘 스크립트 코드
- 5.9.2 실행 결과
- 5.9.3 코드 분석

### 5.10 cron 스케줄 등록

- 5.10.1 쉘 스크립트 코드
- 5.10.2 실행 결과
- 5.10.3 코드 분석

### 5.11 데몬 및 서비스 프로세스의 시작과 정지

- 5.11.1 쉘 스크립트 코드
- 5.11.2 실행 결과
- 5.11.3 코드 분석

### 5.12 로그 파일 로테이션

- 5.12.1 쉘 스크립트 코드 a
- 5.12.2 실행 결과 a
- 5.12.3 코드 분석 a
- 5.12.4 쉘 스크립트 코드 b
- 5.12.5 실행 결과 b
- 5.12.6 코드 분석 b
