### 1. AND GATE

![[../Images/Pasted image 20250718213057.png]]


| **A** | **B** | **Q** |
| :---: | :---: | :---: |
|   0   |   0   |   0   |
|   0   |   1   |   0   |
|   1   |   0   |   0   |
|   1   |   1   |   1   |
**AND 게이트의 진리표**

모든 입력이 1일때만 1이 출력되는 게이트.

```verilog title:"AND_GATE_behavioral"
module and_gate_behavioral (

	input A, B,
	output reg Q
	);

	always @(A or B) begin // A, B 둘 중 하나라도 값이 변경되면 블록을 실행

		if (A == 1'b1 && B == 1'b1) // A와 B가 모두 1일 때
			Q = 1'b1; // Q에 1을 할당
		else
			Q = 1'b0; // 그렇지 않으면 Q에 0을 할당
	end

endmodule
```

```verilog title:"AND_GATE_structural"
module and_gate_structural (
	input A, B,
	output Q // Wire 타입으로 Q 선언해야 함!!!
	);  
	and U1(Q, A, B); // AND 게이트 인스턴스 생성

endmodule
```

```verilog title:"AND_GATE_dataflow"
module and_gate_dataflow (

input A, B,

output Q

);

  

assign Q = A & B; // A와 B의 AND 연산 결과를 Q에 할당

endmodule
```

```verilog title:"AND_GATE_testBench"
module tb_and_gate;
	reg A, B;
	wire Q;

	and_gate_behavioral uut (
	.A(A),
	.B(B),
	.Q(Q)
	);

	// and_gate_dataflow uut_dataflow ( // 사용하고 싶은 거 주석 해제
	// .A(A),
	// .B(B),
	// .Q(Q)
	// );
  
	// and_gate_structural uut_structural (
	// .A(A),
	// .B(B),
	// .Q(Q)
	// );

	initial begin
		$display("Time\tA B | Q"); // \t: 탭 밀리게 하는거
		$monitor("%0t\t%b %b | %b \t%b", $time, A, B, Q);
		// %0t: 시뮬레이션 시간, %b: 2진수(바이너리) 출력
		
		A = 0; B = 0; #10; // timescale에 따라 10ns 대기(10ns 후에 다음 코드 실행)
		A = 0; B = 1; #10;
		A = 1; B = 0; #10;
		A = 1; B = 1; #10;
		
		$finish;
	end

endmodule
```

![[../Images/Pasted image 20250718214741.png]]
**시뮬레이션 결과**


### 2. OR GATE

![[../Images/Pasted image 20250718214947.png]]


| **A** | **B** | **Q** |
| :---: | :---: | :---: |
|   0   |   0   |   0   |
|   0   |   1   |   1   |
|   1   |   0   |   1   |
|   1   |   1   |   1   |
**OR 게이트의 진리표**

여러 입력 중 어떤 것이라도 1이면 출력이 1이 되는 게이트.

```verilog title:"OR_GATE_behavioral"
module or_gate_behavioral (
	input A, B,
	output reg Q
	);
	
	always @(A, B) begin
		if(A == 1'b1 || B == 1'b1)
			Q = 1'b1;
		else
			Q = 1'b0;
	end

endmodule
```

```verilog title:"OR_GATE_structural"
module or_gate_structural (
	input A, B,
	output Q
	);
	
	or (Q, A, B); // 인스턴스 이름 생략 가능.

endmodule
```

```verilog title:"OR_GATE_dataflow"
module or_gate_dataflow (
	input A, B,
	output Q
	);
  
	assign Q = A | B;
	
endmodule
```

```verilog title:"OR_GATE_testBench"
module tb_or_gate;
	reg A, B;
	wire Q;

	or_gate_behavioral uut (
		.A(A),
		.B(B),
		.Q(Q)
		);

	// or_gate_dataflow uut_dataflow (
		// .A(C),
		// .B(D),
		// .Q(Q2)
		// );
	
	// or_gate_structural uut_structural (
		// .A(E),
		// .B(F),
		// .Q(Q3)
		// );

	initial begin

		$display("Time\tA B | Q");
		$monitor("%0t\t%b %b", $time, A, B, Q);
	
		A = 0; B = 0; #10; // timescale에 따라 10ns 대기(10ns 후에 다음 코드 실행)
		A = 0; B = 1; #10;
		A = 1; B = 0; #10;
		A = 1; B = 1; #10;

		$finish;

	end

endmodule
```

![[../Images/Pasted image 20250718215825.png]]
**시뮬레이션 결과**


### 3. XOR GATE
