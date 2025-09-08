```verilog title:"multifunction_watch_top.v"
module multifunction_watch_top (
    input clk, reset_p,
    input [3:0] btn,
    input alarm_off,
    output [7:0] seg_7,
    output [3:0] com,
    output [15:0] led,
    output alarm
    );

    localparam WATCH = 0;
    localparam TIMER = 1;
    localparam STOPWATCH = 2;

    wire btn_mode;
    wire alarm, alarm_off;
    wire [7:0] watch_seg_7, cook_seg_7, stop_seg_7;
    wire [3:0] watch_com, cook_com, stop_com;
    reg [2:0] watch_btn, cook_btn, stop_btn;
    reg [1:0] mode;

    assign led[1:0] = mode;
    assign led[15] = alarm;
    

    watch_top watch (
        .clk(clk),
        .reset_p(reset_p),
        .btn(watch_btn),
        .seg_7(watch_seg_7),
        .com(watch_com)
    );

    cook_timer_top timer(
        .clk(clk),
        .reset_p(reset_p),
        .btn({alarm_off, cook_btn}),
        .seg_7(cook_seg_7),
        .com(cook_com),
        .alarm(alarm)
    );

    stop_watch_top stop (
        .clk(clk),
        .reset_p(reset_p),
        .btn(stop_btn),
        .seg_7(stop_seg_7),
        .com(stop_com)
    );

    btn_cntr mode_btn (
        .clk(clk),
        .reset_p(reset_p),
        .btn(btn[0]),
        .btn_posedge(btn_mode)
    );


    always @(posedge clk, posedge reset_p) begin
        if(reset_p) begin
            mode <= WATCH;
        end
        else if(btn_mode) begin
            if(mode == WATCH) begin
                mode <= TIMER;
            end
            else if(mode == TIMER) begin
                mode <= STOPWATCH;
            end
            else if(mode == STOPWATCH) begin
                mode <= WATCH;
            end
        end
    end

    always @(*) begin
        case(mode)
            WATCH: begin
                watch_btn = btn[3:1];
                cook_btn = 0;
                stop_btn = 0;
            end
            TIMER: begin
                watch_btn = 0;
                cook_btn = btn[3:1];
                stop_btn = 0;
            end
            STOPWATCH: begin
                watch_btn = 0;
                cook_btn = 0;
                stop_btn = btn[3:1];
            end
        endcase
    end

    assign seg_7 = mode == WATCH ? watch_seg_7 :
                   mode == TIMER ? cook_seg_7 :
                   mode == STOPWATCH ? stop_seg_7 : watch_seg_7;
    assign com = mode == WATCH ? watch_com :
                   mode == TIMER ? cook_com :
                   mode == STOPWATCH ? stop_com : watch_com;

endmodule
```
