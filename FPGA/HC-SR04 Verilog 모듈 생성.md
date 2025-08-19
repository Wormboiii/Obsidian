## 1. TOP MODULE

```verilog title:"HC_SR04_controller"
module HCSR04_cntr (
    input clk, reset_p,
    input echo,
    output reg trig,
    output reg [8:0] distance
);

    localparam S_IDLE       = 5'b00_001;   
    // 아무것도 안하는 상태
    
    localparam S_TRIG       = 5'b00_010;   
    // 거리 측정을 위해 트리거핀에 High 신호 출력 상태
    
    localparam S_START_CNT  = 5'b00_100;        
    // Echo핀에 1이 들어오면 음파 시간을 재는 상태
    
    localparam S_END_CNT    = 5'b01_000;        
    // 음파가 반사되어 돌아오며 Echo가 0으로 떨어질 때 카운팅 끝내는 상태
    
    localparam S_MEASURE    = 5'b10_000;        
    // 카운팅 된 결과값으로 거리를 재는 상태

    wire clk_us_pedge;
    clk_div_100 us_clk(
        .clk(clk),
        .reset_p(reset_p),
        .pedge_div_100(clk_us_pedge)
    );

    reg [4:0] cur_state, next_state;            
    // 상태 천이를 위한 레지스터
    
    reg [13:0] cnt_us;                          
    // 최대 감지거리 400cm일때 12,000us 걸림, 거기에 맞춘 카운트값
    
    reg cnt_us_e;                               
    // 카운팅 on/off 를 위한 레지스터
    
    
    always @(posedge clk, posedge reset_p) begin
        if(reset_p) begin
            cnt_us <= 0;                        
            // 리셋 누르면 카운트 0으로 초기화
            
        end else if(clk_us_pedge && cnt_us_e) begin
            cnt_us <= cnt_us + 1;               
            // 카운트 enable이 되면 1us마다 1씩 증가
            
        end
        else if(!cnt_us_e && (cur_state == S_IDLE || cur_state == S_TRIG)) begin
            cnt_us <= 0;                        
            // 카운트 비활성화, 상태가 아이들, 트리거일때만 카운트 초기화
        end
    end


    wire echo_pedge, echo_nedge;                
    // Echo핀 High 유지시간을 재기 위한 엣지디텍터 사용
    
    edge_detector_p echo_edge (
        .clk(clk),
        .reset_p(reset_p),
        .cp(echo),
        .p_edge(echo_pedge),
        .n_edge(echo_nedge)
    );

    
    always @(posedge clk, posedge reset_p) begin    
    // 상태 천이, 리셋 누를때만 초기화
    
        if(reset_p) begin
            cur_state <= S_IDLE;
        end else begin
            cur_state <= next_state;
        end
    end
    
    reg [13:0] temp_time;                           
    // 거리, 시간값을 저장할 임시 레지스터
    
    reg [8:0] temp_distance;
    always @(posedge clk, posedge reset_p) begin
        if(reset_p) begin                           
        // 리셋 누르면 다 초기화
        
            trig <= 0;
            cnt_us_e <= 0;
            temp_time <= 0;
            temp_distance <= 0;
            next_state <= S_IDLE;

        end else begin
            case(cur_state)

                S_IDLE : begin                      
                // 아이들 상태일 때
                
                    if(cnt_us < 15'd1_000) begin
                        cnt_us_e <= 1;              
                        // 1ms동안 일단 대기, 트리거 low로 초기화
                        
                        trig <= 0;
                    end else begin
                        cnt_us_e <= 0;              
                        // 다 기다렸으면 카운트 끄고 다음 상태로 천이
                        
                        next_state <= S_TRIG;
                    end
                end

                S_TRIG : begin                  
                // 트리거 상태일 때
                
                    if(cnt_us < 15'd10) begin   
                    // 10us 트리거 신호
                    
                        cnt_us_e <= 1;          
                        trig <= 1;
                    end else begin
                        cnt_us_e <= 0;          
                        // 10us동안 high 보낸 후 low 초기화
                        //카운트 끄고 다음 상태로 천이
                        
                        trig <= 0;
                        next_state <= S_START_CNT;
                    end
                end

                S_START_CNT : begin             
                // 에코 카운트 시작 상태일 때
                
                    if(echo_pedge) begin        
                    // 8개의 초음파 펄스가 출력된 후 
                    //에코핀이 자동으로 high로 올라오면
                    
                        cnt_us_e <= 1;          
                        // 카운팅 시작하고 다음 상태로 천이
                        
                        next_state <= S_END_CNT;
                    end
                end

                S_END_CNT : begin
                    if(echo_nedge) begin        
                    // 에코 카운트 끝 상태일 때
                    
                        cnt_us_e <=0;           
                        // 카운트만 끄고 값 초기화 ㄴㄴ
                        //(위에 카운트 조건에 그렇게 맞춰둠)
                        
                        temp_time <= cnt_us;    
                        // Echo핀이 high로 유지된 시간 기록
                        
                        next_state <= S_MEASURE;
                        // 다음 상태로 천이
                    end
                end

                S_MEASURE : begin                       
                // 거리 측정 상태일 때
                
                    temp_distance <= temp_time / 58;    
                    // 걸린 시간을 58로 나누어서 cm단위 거리 계산
                    
                    distance <= temp_distance;          
                    // 계산한 거리를 출력과 연결
                    
                    next_state <= S_IDLE;               
                    // 첫 상태로(다음 상태) 천이
                    
                end
            
            endcase

        end
    end

endmodule
```

## 2. Testbench

```verilog title:"tb_hcsr04_controller"
module tb_hcsr04_controller();

    reg clk, reset_p;
    reg echo;
    wire trig;
    wire [8:0] distance;

    HCSR04_cntr DUT(
        clk,
        reset_p,
        echo,
        trig,
        distance
    );

    initial begin           // 시뮬레이션 초기값
        clk = 0;
        reset_p = 1;
        echo = 0;
    end

    always #5 clk = ~clk;   // 100MHz 클럭 생성

    initial begin
        #10;
        reset_p = 0;        // 리셋 꺼줌
        #1_000_000;         // idle 1ms동안 대기
        #10_000;            // 트리거 high 유지를 위해 10us 대기 
        echo = 1;           // 다 보냈으면 echo핀을 임의로 high로 올려줌
        #6_000_000;         // 대략 100cm 정도 걸리는 시간(6,000us)동안 대기
        echo = 0;           // echo핀 임의로 low로 떨궈줌
        #100;               // 다음 상태(idle)로 천이될수 있게 잠시 대기
        
        #1_000_000;         // 값 잘 바뀌나 보게 위에꺼 복붙함
        #10_000;            
        echo = 1;          
        #12_000_000;        
        // 대략 200cm정도 걸리는 시간(12,000us)으로 얘만 바꿔줌
                   
        echo = 0;           
        #100;

        $stop;              // 시뮬 끝

    end

endmodule
```

## 3. Simulation

![[../Images/Pasted image 20250819172751.png]]
MEASURE 상태에서 다음 상태로 천이하는 구문과 계산한 거리를 출력으로 연결하는 구문이 동시에 진행되어 그 다음번인 IDLE 상태가 되어야 distance에 거리값이 출력되는 문제가 있음.
아주 짧은 시간동안 연산 결과를 출력한 후 다음 상태로 가게 하는 식으로 수정해야 할듯.
값은 예상과 같이 잘 출력됨.

![[../Images/Pasted image 20250819173201.png]]
두번째 사이클에서도 예상과 같이 값도 잘 나오고 출력도 잘 바뀌지만 마찬가지로 다음 IDLE로 가야 거리값이 출력되는 문제가 있음.