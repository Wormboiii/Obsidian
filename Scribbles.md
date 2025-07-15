
[[모델링 기법]]


타이머 / 카운터 가장 큰 차이점
주기가 일정한지???
주기가 일정해야 클락을 세면서 걸린 시간을 잴 수 있음.

일정한 주기를 사용해서 몇 클락이 지난 시간을 잼 -> 타이머
주기는 모르겠고 클락이 몇번 올랐는지(내려갔는지) 횟수를 셈 -> 카운터


래치 / 플립플롭 가장 큰 차이점
래치: 레벨에 따라 동작, ex) 0일때 동작 x, 1일때 동작 o
플립플롭: 엣지에 따라 동작, ex) 상승엣지일때 동작 / 하강엣지일때 동작


![[Pasted image 20250630144136.png]]
폴링엣지 검출에서는 0000 0001 이런식으로
-> 이거 업카운트




![[Pasted image 20250630144804.png]]
라이징 엣지 검출때 T 플립플롭 사용하면
-> 이건 다운카운트
1111 1110.... 이런식으로 내려가는게 맞음



클락이 있다고 다 동기 회로가 아님
모두 동일한 클락을 공유해야 동기 회로



RTL_ADD <- 이거 실제로 칩 안에 들어있는 덧셈 회로임
elaborate 하면 나오는 schematic에 저거 나오면 추상적 기호라고 하는데 실제로 있는 회로???
더 찾아봐야할듯?




SPI통신
모토로라 통신은 MSB부터 보냄
TI통신은 LSB부터 보냄

CPOL(Clock Polarity)는 LOW / HIGH 옵션 두개 있는데
low로 설정하면 0부터 시작하는 클럭(아날로그로 따지면 사인파)
high로 설정하면 1부터 시작하는 클럭(아날로그로 따지면 코사인파)

CPHA(clock phase)는 1 edge / 2 edge 옵션 두개 있는데
1 edge로 설정하면 매 주기에 신호 발생
2 edge로 설정하면 짝수 주기때 신호 발생

cpol x cpha 설정을 조합해서 2x2 = 4가지 모드가 있음.


[[순차논리회로]]

모터컨트롤러 1,4 / 2,3 쌍임




slave F0C77F690898
master 4006A06CC85A



mas D436399FAA40
sla 04A3166A1AEB



[[HM10 문제해결]]


Vivado 상단 Help -> Add design tools or devices 버튼 눌러서 케이블 드라이버(USB 드라이버) 다운받으려고 하면 아래와 같이 뜸.
![[Pasted image 20250715100802.png]]

[UG973 문서](https://docs.amd.com/r/en-US/ug973-vivado-release-notes-install-license/Installing-Cable-Drivers)

![[Pasted image 20250715102521.png]]
드라이버를 받기 전 선행 라이브러리를 받아야 함
현재 우리는 24.01 버전 사용하므로 맨 마지막 줄에 있는 디렉토리를 찾아봐야 함.
vitis가 설치된 폴더 내 scripts 폴더로 가보면 installLibs.sh 파일이 있음.
``sudo install ./installLibs.sh`` 로 라이브러리 설치.
뭔가 굉장히 많은걸 받기 시작함.

![[Pasted image 20250715102748.png]]
그 다음에 케이블 드라이버 설치
비바도 폴더 내 data/xicom/cable_drivers/in64 디렉토리로 가보면 텍스트 편집해서 디렉토리를 바꿔준 후 설치했던 install_digilent.sh 파일 외에 install_drivers 라는 파일도 있음.
해당 파일도 ``sudo install ./install_drivers`` 로 다운로드 받아줌.
본인은 파일 한두개? 업데이트가 진행됐음.


