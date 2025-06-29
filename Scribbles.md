
FPGA의 가장 큰 특징: **다른 언어와는 다르게 _병렬_ 으로 코드가 실행된다!!**

#### 1. 기본적인 모듈 구성 / AND Gate 실습
```verilog title:"And gate"
module And_gate(
    input A,
    input B,
    output F
    );   
    assign F = A & B; 
endmodule
```
기본적으로 모든 모듈을 선언 시 'module' 로 열고 'endmodule'로 닫아야 한다.
모듈의 첫 시작 괄호 내에는 모듈이 사용할 인/아웃풋을 최초로 선언해 준다.
모든 인/아웃풋을 선언하고 괄호를 닫은 후에는 이 모듈에서 인/아웃풋의 관계를 나타내는 코드를 작성한다.

위 코드의 경우 AND 게이트의 동작을 하기 위해 출력 F를 입력 A와 B를 AND연산을 해 주었다.

![[Pasted image 20250629170035.png]]
Source에서 해당 모듈을 Top으로 설정한 이후 Open Elaborated Design을 클릭해 Schematic을 확인해 보면 정상적으로 AND Gate가 생성된 것을 확인할 수 있다.

![[Pasted image 20250629170415.png]]
Source에서 시뮬레이션도 해당 모듈을 Top으로 설정한 후 Run Simulation을 클릭해 시뮬레이션을 실행해 보면, 입력 A와 B 모두 1일때만 출력 F도 1이 나오는 AND Gate가 정상적으로 생성된 것을 확인할 수 있다.


#### 2. 구조적 모델링 실습
```verilog

```