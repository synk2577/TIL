기출 오답정리

<br>

# 1. X 윈도

### `xhost`
x 서버에 접근할 수 있는 클라이언트 지정하거나 해제하는 명령

**`# host [+|-] [IP_주소 or 도메인명]`**


### `xauth`
X 접근하거(authority) 파일 관련 도구   
**사용자 기반 인증**으로 `xhost`의 문제 해결

**`# xauth list $DISPLAY`**: `.Xauthority 파일`의 **MIT-MAGIC_COOKIE-1 키 값** 출력

### X 윈도 응용프로그램
- 파일 관리 프로그램
  - dolphin
  - nautilus
  - konqueror
- 문서 및 이미지 뷰어 프로그램
  - Okular    

<br>

# 2. 인터넷 활용

### TCP/IP 
- 구조
  - 응용 계층
  - 전송 계층: TCP, UDP
  - 인터넷 계층
  - 네트워크 인터페이스 계층   

### TCP 
연결 지향 프로토콜    
세그먼트가 응답(Ack)을 주고받으며 점검 → 오류 검사 및 제어     

#### 3-way handshaking 수행 순서
`SYN` → `ACK/SYN` → `ACK`


### 인터넷 서비스 종류
- **SSH**
  - 인증파일 경로: `/home/(사용자명)/.ssh/authorized_keys`
- **삼바 (Samba)**
  - SMB 프로토콜 확장 => **`CIFS`** (Common Internet File System)   
- **NFS (Network File System)**
  - 네트워크 상에서 다른 컴퓨터의 파일 시스템을 마운트하고 공유
  - `NIS`와 `RPC` 프로토콜 기반 작동 → 해당 서비스를 해주는 데몬을 먼저 실행해야 함 = `rpcbind 데몬`(구 `portmap 데몬`) 

### 관련 명령어
- `netstat`: 네트워크 연결 상태 출력, 서버에 접속한 클라이언트의 IP 주소 및 포트번호 확인
- `ss`: 소켓 상태 출력, `netstat`와 유사
- `ip`: 설정 정보 출력 및 변경
  - `ip addr show`: IP 주소 정보 출력 




<br>

# 3. 응용 분야 
