# Live Response
> 구동되고 있는 시스템에서 휘발성 정보를 수집하는 행위이다.  
최근들어 휘발성 정보의 중요성이 인식되며 해당 행위가 중요시되고 있다.

휘발성 저장장치에서는
* 현재 동작중인 프로세스
* 열려있는 파일 목록
* 네트워크 연결 정보
* 클립보드
* 사용자 정보
* 임시파일
* 비밀번호
* 복호화(암호화의 반댓말) 된 데이터
* 기타  

등의 정보를 찾을 수 있기 때문에 중요하다고 볼 수 있다.  

또한 수집한 휘발성 정보를 통해 시스템의 상황을 파악할 수 있다.  
윈도우 버전과 서비스 팩 ㅡ 단일 설치본 형식의 소프트웨어 프로그램에 대한 업데이트, 수정 사항, 개선 사항의 모임 ㅡ 으로 취약점을 목록화할 수 있는데다가 연계하여 조사 범위를 좁힐 수 있으므로 시간 절약에 용이하다!

그리고 24시간 서버 컴퓨터가 운영되어 만약 멈출 경우 막대한 금전적 피해를 얻는 경우에도 종료하지 않고 Live Response로 증거를 수집하고 분석해야 한다.

### Live Response 수행 여부
본 책에서는 하지 않아야 할 상황을 크게 두 가지로 나누어 정리했다. (해야 할 상황은 정리하기 힘들다나 뭐라나..)

1. 파일이 삭제되고 있는 경우  
개인 PC일 경우는 플러그를 뽑고, 서버 PC일 경우에는 정상적으로 종료한다(서버 PC는 강제종료시 시스템에 치명적인 손상이 생길 수 있다고 한다).
2. 수사에 사용하는 도구가 검증되지 않은 도구일 경우  
검증되지 않은 도구는 어떤 영향을 끼칠 지 모르기 때문이다.

### Live Response 원칙

__로카르드의 교환 법칙__ 이란?  
>"접촉하는 두 물체 사이에는 반드시 교환이 일어난다"는 명제이다.

즉, 조사할 때에는 언제나 시스템에 영향을 미칠 수 밖에 없다는 이야기이다.  
그러므로 영향을 최소화하기 위해 검증되고 포렌식에 특화된 도구를 사용해야 한다.

다음은 시스템 구성요소들이 어떤 영향을 받는지 나열한 것이다.
1. 메모리  
도구가 실행되면 메모리가 갱신되기 때문에 이전의 데이터가 날아가버린다.  
2. 네트워크  
도구가 네트워크 기능을 사용한다면 통신을 시도할 때 로그가 남게되며 기존 로그정보가 지워지게 된다.  
또한 [커널 오브젝트](https://docs.microsoft.com/en-us/windows/desktop/sysinfo/kernel-objects)에 의해 [커널 테이블](https://support.ca.com/cadocs/0/CA%20ControlMinder%2012%207-KOR/Bookshelf_Files/HTML/idocs/index.htm?toc.htm?1358957.html)이 갱신된다고 한다(...?)
3. 프리패치 파일(Windows 한정)  
__프리패치 파일__ 은 윈도우에서 새로운 프로그램이 실행될 때 생성되는 파일이다.  
그러므로 조사관의 도구가 실행될 때도 프리패치 파일이 생성된다.  
윈도우의 프리패치 파일의 개수는 정해져 있으므로 이전의 프리패치 파일이 없어질 수도 있다.
4. 레지스트리(Windows 한정)  
많은 Live Response 도구들은 [레지스트리](https://ko.wikipedia.org/wiki/%EC%9C%88%EB%8F%84%EC%9A%B0_%EB%A0%88%EC%A7%80%EC%8A%A4%ED%8A%B8%EB%A6%AC)에 접근하므로 마지막 접근 시간이 갱신되어 버린다.  
또한 레지스트리의 값을 설정하게 될 경우 마지막 쓰기 시간도 갱신된다.
5. DLL(Dynamic Link Library, Windows 한정)  
__DLL__ 은 여러 가지 비슷한 함수의 모음으로, 동적으로 컴파일할 경우 필요한 파일이다.  
도구가 DLL을 사용할 경우, 마지막 접근 시간이 마찬가지로 갱신된다.
6. 로그파일  
조사관의 도구가 에러를 일으키면 에러로그가 남게 되고, 이로인해 기존의 로그정보가 담긴 로그파일이 갱신되어 이전의 로그가 삭제될 수 있다.

Live Response에서 사용할 도구는 이러한 경우들을 고려하여 성능이 좋고 영향을 최소화하는 도구를 선별하여 사용하여야 한다.

### 휘발성 정보 수집

일단 이 책에서는 많은 조사관에게 길잡이 역할을 하는 다음의 두 개의 휘발성 정보 수집 절차 문서가 제시된다.
1. [Guidelines for Evidence Collection and Archiving(RFC 3227)](https://tools.ietf.org/html/rfc3227)
2. [Guide to integrating Forensic Techniques into Incident Response(NIST Special Publication
800- 86)](https://www.nist.gov/publications/guide-integrating-forensic-techniques-incident-response)

두 문서의 구성 순서이다.
![table 2.1](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/table2-1.PNG?raw=true)

휘발성 정보 수집을 위해서는 도구 사용이 필요한데, 도구 사용을 위해서는 관리자 권한 획득이 꼭 필요하다.  
관리자 권한을 획득하기 위해서는 여러 가지 방법이 있는데, 가장 쉬운 방법은 사용자 로그오프 후 관리자 로그인을 하는 것이다.  
이 방법은 포렌식 관점으로 봤을 때 잘못된 방법이므로 다른 방식으로 관리자 권한을 획득해야 한다.  
윈도우에서는 [Runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525(v%3Dws.11))라는 기본 명령어가 있는데 이를 통해 일시적인 관리자 권한을 얻을 수 있다.

### Windows 휘발성 정보 수집

1. 시스템 시간  
시스템 시간은 포렌식에서 중요한 의미를 가진다.  
[date](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/date)와 [time](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/time)명령어로 수집 가능하다.  
윈도우 하단의 트레이 패널로 시간을 조작하면 레지스트리에 기록이 남지만 명령어로 조작할 경우 레지스트리에 기록이 남지 않는다.
2. 네트워크 연결 정보  
[netstat](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat) 명령어로 수집할 수 있다.
3. 프로세스 목록  
프로세스 분석은 [tasklist](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)명령어와 [Pslist도구](https://docs.microsoft.com/en-us/sysinternals/downloads/pslist)로 프로세스를 볼 수 있다.  
![pslist](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/pslist.PNG?raw=true)  
다음 스크린샷은 -t 옵션을 사용하여 부모 프로세스와 자식 프로세스의 관계를 직관적으로 파악할 수 있도록 하였다.  
GUI 도구로는 Process Monitor, Process Explorer가 있다.  
GUI는 CLI보다 영향을 많이 주지만 프로세스의 전체 경로와 DLL, TCP 동작 등의 여러 가지 정보를 제공한다.  
4. [핸들](https://ko.wikipedia.org/wiki/%ED%95%B8%EB%93%A4_(%EC%BB%B4%ED%93%A8%ED%8C%85)) 정보  
도구로는 주로 Handle.exe를 사용한다.
5. DLL 목록  
만약 [DLL Injection](https://ko.wikipedia.org/wiki/DLL_%EC%9D%B8%EC%A0%9D%EC%85%98)으로 인해 사고가 난 것이라면 프로세스 목록으로만 분석이 어렵기 때문에 DLL 목록을 불러와 분석을 해야한다.  
DLL 목록은 [ListDlls.exe도구](https://docs.microsoft.com/en-us/sysinternals/downloads/listdlls)를 사용해 쉽게 얻을 수 있다.
5. 로그온(=로그인) 사용자 정보  
로그온 사용자 정보를 수집하는데 필요한 명령어는 [net session](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh750729(v%3Dws.11))이라는 명령어와 도구로서는 [Psloggedon](https://docs.microsoft.com/en-us/sysinternals/downloads/psloggedon)(앞서 설치한 PSTools폴더 안에 포함되어 있다), [Logonsession](https://docs.microsoft.com/en-us/sysinternals/downloads/logonsessions) 이라는 도구가 있다.  
net session 명령어는 원격에서 로그온 한 사용자의 정보를 알려주고, psloggedon과 Logonsession 도구는 로컬과 원격 로그온 사용자의 정보를 보여준다.  
자세한 정도는 Logonsession도구가 프로세스 목록(Logonsession도구의 /p옵션을 사용할 경우 프로세스 목록을 볼 수 있다)까지 보여주기 때문에 앞선다.
7. 파일 핸들  
[net file](https://sourcedaddy.com/networking/the-net-file-command.html) 명령어와 Psfile도구가 있다.  
다음은 net file명령어를 실행하여 현재 열어둔 파일이 없다는 것을 보여준다.
![netFile](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/netFile.PNG?raw=true)
8. [Open 포트](https://en.wikipedia.org/wiki/Open_port)와 *프로세스 맵핑*(???) 정보  
해당 정보를 수집하고 분석하면 어떠한 프로세스가 어떤 포트로 들어가 어떤 서버와 연결하여 어떤 작업을 하는지 파악할 수 있다.  
[netstat -b](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat#syntax) 명령어로 확인할 수 있다.  
하지만 프로세스의 전체 경로를 제공하지는 않는다.
9. 히스토리  
명령 프롬프트를 닫지 않을 경우 [doskey/history](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) 명령어를 통해 명령 프롬프트에서 어떠한 명령어를 사용하였는지 알 수 있다.
10. 윈도우 서비스 정보  
해당 정보를 수집, 분석하면 어떤 악성 프로그램이 서비스에 몰래 등록되어 사용되는지 판단할 수 있다.  
이러한 서비스 목록을 얻기 위해서는 [Psservice](https://docs.microsoft.com/en-us/sysinternals/downloads/psservice)라는 도구를 사용하면 된다.
해당 책에서는 다음과 같이 Psservice.exe : more 명령어를 사용했다.
![Psservice](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/Psservice.PNG?raw=true)
종종 서비스의 설명이 없는 경우가 있는데 그 서비스는 악성일 수도 있고 아닐 수도 있다. 그렇다고 서비스 설명이 있다고 꼭 악성 프로그램 서비스가 아니라고 할 수도 없다.
11. 시작 프로그램 목록 정보  
해당 정보는 OS에 의해 자동으로 실행되는 정보이다.  
[autorunsc](https://docs.microsoft.com/en-us/sysinternals/downloads/autoruns) 도구로 얻을 수 있다.
12. [라우팅 테이블](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%8C%85_%ED%85%8C%EC%9D%B4%EB%B8%94) 정보 / *네트워크 인터페이스*(???) 정보  
라우팅 테이블은 개인 PC에도 존재한다.  
이 정보는 네트워크 데이터 흐름을 조절할 수 있다.  
게이트웨이 등이 내부 개인 PC로 설정되어 있으면 중계 컴퓨터를 의심해 봐야 한다.  
해당 정보 수집 방법으로는 [netstat -r](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat#syntax)가 있다.  
네트워크 인터페이스 정보를 분석해야 하는 이유는 [MAC](https://ko.wikipedia.org/wiki/MAC_%EC%A3%BC%EC%86%8C) 변조 유무를 파악하기 위해서이다.  
네트워크 인터페이스 정보를 얻으려면 [ipconfig /all](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig#syntax)명령어를 사용한다.  
만약 [promiscuous mode](https://en.wikipedia.org/wiki/Promiscuous_mode) 유무를 파악하고 싶으면 PromiscDetect 도구를 사용한다. 
13. 클립보드 정보  
복붙 정보다.  
Windows XP이후의 윈도우에서는 별도의 도구를 사용해야 한다.
14. 네트워크 공유 폴더 정보  
해당 정보의 수집은 [net use](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155(v%3Dws.11)) 명령어를 사용하면 된다.  
[net share](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh750728(v%3dws.11)) 명령어와 다른 점은 net use는 원격 컴퓨터의 공유 폴더 목록을 확인할 수 있고, net share 명령어는 로컬 컴퓨터의 공유 폴더 목록을 확인할 수 있다는 점이다.
15. NetBios 정보  
NetBios는 데이터 교환으로 TCP/IP 프로토콜을 사용한다.  
그러므로 NetBios를 통해 데이터를 교환할 경우 상대방의 IP와 NetBios 이름을 테이블 형태로 저장하며, 이것을 'Cached NetBios Name Table'이라고 부른다.  
이 정보는 600초 동안 유지되며 [nbtstat -c](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/nbtstat) 명령어를 통해 수집한다.  
NetBios의 이름은 Windows 2000부터 DNS이름과 같도록 설정되어 있으므로 구별법을 알아야 한다.  
구별하는 방법으로는  
    * DNS 이름은 255자인 반면, NetBios는 16자이다.
    * NetBios는 이름 뒤에 <16진수>의 접미사가 붙는다.  
    ex) NetBios<0F>  

    다음은 NetBios 접미사 목록이다. 
    ![netbios1](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/NetBios1.PNG?raw=true)
    ![netbios2](https://github.com/RaGoon1222/antirootDigitalForensic/blob/master/week_1/img/NetBios2.PNG?raw=true)
    
16. 프로세스 메모리  
프로세스는 소스 코드와 DLL파일, 실행 전체 경로 등을 가상 메모리에 업로드하여 동작한다. 이때 프로세스가 사용하는 가상 메모리를 덤프하는 __프로세스 덤프(컴퓨터 프로그램이 특정 시점에 작업 중이던 메모리 상태를 기록한 것)__ 를 분석함으로써 좀 더 정확하고 중요한 정보를 알 수 있다.  
덤프 방법으로는 [userdump.exe](https://www.microsoft.com/en-us/download/details.aspx?id=4060) 도구를 사용한다.  

프로세스 메모리를 끝으로 윈도우 휘발성 정보들에 대해 알아보았다.  
이 많은 휘발성 정보들을 자동으로 수집할 수 있는 자신만의 프로그램이나 스크립트([배치파일](https://ko.wikipedia.org/wiki/%EB%B0%B0%EC%B9%98_%ED%8C%8C%EC%9D%BC), 스크립트 언어 등으로)를 만들어 포렌식 업무에 적용하면 더욱 효율적으로 수행할 수 있다.  
또한 수집한 대부분의 도구들이 EULA 확인 대화상자가 뜬다.  
이는 상당히 걸림돌이 되는데, 실행할 때 옵션으로 /accepteula를 지정하면 대화상자가 나타나지 않는다.

### Mac OS X 휘발성 정보 수집