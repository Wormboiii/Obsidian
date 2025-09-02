
[참고 링크](https://digilent.com/reference/learn/programmable-logic/tutorials/basys-3-programming-guide/start?srsltid=AfmBOooo2JpOI8bYiZTj_PwDD6u4DRiW3fvVV48N7I8HSuAWK3KWyhtM)

### 1. 바이너리 프로그래밍 파일 생성

비바도 기본 설정은 비트스트림 생성 시 .bit 파일만 생성하기 때문에 플래시 메모리가 읽을 수 있는 .bin 파일을 같이 생성하도록 설정해 주어야 함.

![[Images/Pasted image 20250829112417.png]]
1. Tools -> Settings 로 설정 창을 열어준다.



![[Images/Pasted image 20250829112503.png]]
2. Project Settings -> Bitstream에서 -bin_file* 옵션을 체크한 후 저장한다.


### 2. 플래시 메모리 설정
![[Images/Pasted image 20250829112833.png]]
1. Hardware Manager에서 xc7a35t 칩에 우클릭 후 Add Configuration Memory Device 버튼을 선택.


![[Images/Pasted image 20250829113034.png]]
2. mx25l3273f 를 검색 후 나오는 spi 칩을 선택하고 확인 버튼을 누른다.



![[Images/Pasted image 20250829113132.png]]
3.  다음 대화 상자에서 확인 버튼을 누른다.




### 3. 프로그래밍

![[Images/KakaoTalk_20250829_113556346.jpg]]
1. Basys 3 보드에서 프로그래밍 점퍼를 QSPI 모드로 물린다.


	
![[Images/Pasted image 20250829113200.png]]
2. Configuration file 에서 내 .bin 파일을 선택하고 확인 버튼을 누른다..
		(경로는 프로젝트 -> 프로젝트명.runs -> impl 에 있다.)




![[Images/Pasted image 20250829113750.png]]
플래시가 성공적으로 되면 상단 문구가 뜬다.
보드의 전원을 껐다 켜서 내 코드가 전원을 꺼도 잘 유지되는지 확인한다.