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


![[../Images/Pasted image 20250718220310.png]]

| **A** | **B** | **Q** |
| :---: | :---: | :---: |
|   0   |   0   |   0   |
|   0   |   1   |   1   |
|   1   |   0   |   1   |
|   1   |   1   |   0   |
**XOR 게이트의 진리표**

배타적(eXclusive) OR라는 이름에 걸맞게, 입력 중 오직 하나만 1일 때 1을 출력한다.

```verilog title:"XOR_GATE_behavioral"
module xor_gate_behavioral (
    input a, b,
    output reg q
	);
    always @(a, b) begin
        if(a != b)
            q = 1'b1;
        else
            q = 1'b0;

    end
endmodule
```

```verilog title:"XOR_GATE_structural"
module xor_gate_structural (
    input a, b,
    output q
	);
    xor U1(q, a, b);     // 인스턴스는 습관적으로 적어주는게 좋다
endmodule
```

```verilog title:"XOR_GATE_dataflow"
module xor_gate_dataflow (
    input a, b,
    output q
	);
    assign q = a ^ b;
endmodule

```

```verilog title:"
module tb_xor_gate;
	reg a, b;
	wire q;

	xor_gate_behavioral uut (
		.a(a),
		.b(b),
		.q(q)
		);

	// xor_gate_dataflow uut_dataflow (
	// .a(a),
	// .b(b),
	// .q(q)
	// );

	// xor_gate_structural uut_structural (
	// .a(a),
	// .b(b),
	// .q(q)
	// );

	initial begin
		$display("Time\tA B | Q");
		$monitor("%4t\t%b %b | %b", $time, a, b, q);
		
		a = 0; b = 0; #10;
		a = 0; b = 1; #10;
		a = 1; b = 0; #10;
		a = 1; b = 1; #10;

		$finish; // 시뮬레이션 종료
	end 

endmodule
```


![[../Images/Pasted image 20250718221315.png]]
**시뮬레이션 결과**


### 3. NOT DERIVED GATE

#### 3-1 NAND GATE

![[../Images/Pasted image 20250718221637.png]]

| **A** | **B** | **Q** |
| :---: | :---: | :---: |
|   0   |   0   |   1   |
|   0   |   1   |   1   |
|   1   |   0   |   1   |
|   1   |   1   |   0   |
**NAND 게이트의 진리표**

NAND GATE 출력단에 NOT GATE가 붙어 출력이 반전된 형태의 게이트.
회로도를 보면 알 수 있지만, 입력은 NAND 게이트와 같지만 출력만 정 반대이다.


```verilog title:"NAND_GATE_behavioral"
module nand_gate_behavioral (
    input A, B,
    output reg Q
);

    always @(A or B) begin              // always @(A, B)랑 똑같음, A / B 둘 중 하나라도 값이 변경되면 블록을 실행
        if(A == 1'b1 && B == 1'b1)
            Q = 1'b0;
        else
            Q = 1'b1;      
    end
    
endmodule
```

```verilog title:"NAND_GATE_dataflow"
module nand_gate_dataflow (
    input A, B,
    output Q
);

    assign Q = ~(A & B);  // A와 B의 AND 연산 결과를 NOT 연산하여 Q에 할당
    
endmodule
```

```verilog title:"NAND_GATE_structural"
module nand_gate_structural (
    input A, B,
    output Q
);

    nand U1(Q, A, B);  // Verilog에서 제공하는 nand 게이트를 사용하여 구조적 모델링
    
endmodule
```

```verilog title:"NAND_GATE_testBench"
module tb_nand_gate;
    reg a, b;
    wire q;

    nand_gate_behavioral uut (
        .A(a),
        .B(b),
        .Q(q)
    );

    // nand_gate_dataflow uut_dataflow (
    //     .A(a),
    //     .B(b),
    //     .Q(q)
    // );

    // nand_gate_structural uut_structural (
    //     .A(a),
    //     .B(b),
    //     .Q(q)
    // );

    initial begin
        $display("Time\tA B | Q");
        $monitor("%4t\t%b %b | %b", $time, a, b, q);

        a = 0; b = 0; #10;
        a = 0; b = 1; #10;
        a = 1; b = 0; #10;
        a = 1; b = 1; #10;
        $finish;  // 시뮬레이션 종료
    end

endmodule
```

![[../Images/Pasted image 20250718222732.png]]
**시뮬레이션 결과**


#### 3-2 NOR GATE


