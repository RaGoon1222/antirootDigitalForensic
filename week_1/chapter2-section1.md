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
![table 2.1](week_1/img/table2-1.png)

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
