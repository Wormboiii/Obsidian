
Texas Instrument사에서 공식으로 제공하는 TL072.031 spice 모델을 ltspice에서 사용 중 전류 소모량이 비정상적으로 큰 것을 확인.

![[../../Pasted image 20250804190241.png]]

데이터시트 상에서 무부하시 Icc가 최대 2.5mA인 것을 확인할 수 있는데,

![[../../스크린샷 2025-08-04 185551.png]]
LTspice에서 해당 모델을 사용해 무부하 조건에서 +-9V 전압을 인가했을 시 전압원에 흐르는 전류가 대략 +-8.5mA로 비정상적으로 큰 것을 확인할 수 있다.

[TL072 모델 공급전류 오류 링크](https://e2e.ti.com/support/tools/simulation-hardware-system-design-tools-group/sim-hw-system-design/f/simulation-hardware-system-design-tools-forum/622836/tina-spice-tl072-supply-current-result-of-tl072-spice-model)

위의 링크에서 참고할 수 있듯이, 양 단 전압 사이의 내부 저항 RP값이 21.43k옴이여야 하는데 2.143k옴으로 오류가 있어서 발생하는 문제이다.