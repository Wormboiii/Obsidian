
[참고 링크](https://digilent.com/reference/learn/programmable-logic/tutorials/basys-3-programming-guide/start?srsltid=AfmBOooo2JpOI8bYiZTj_PwDD6u4DRiW3fvVV48N7I8HSuAWK3KWyhtM)

### 1. 바이너리 프로그래밍 파일 생성

비바도 기본 설정은 비트스트림 생성 시 .bit 파일만 생성하기 때문에 플래시 메모리가 읽을 수 있는 .bin 파일을 같이 생성하도록 설정해 주어야 함.

![[Images/Pasted image 20250829112417.png]]
1. Tools -> Settings 로 설정 창을 열어준다.



![[Images/Pasted image 20250829112503.png]]
2. Project Settings -> Bitstream에서 -bin_file* 옵션을 체크한 후 저장한다.



![[Images/Pasted image 20250829112833.png]]
3. Hardware Manager에서 xc7a35t 칩에 우클릭 후 Add Configuration Memory Device 버튼을 선택.


