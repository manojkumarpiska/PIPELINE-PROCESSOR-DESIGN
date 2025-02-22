module pipelined_processor_tb;
    reg clk;
    reg rst;

    // Instantiate the processor
    pipelined_processor uut (
        .clk(clk),
        .rst(rst)
    );

    // Clock generation
    always #5 clk = ~clk;

    initial begin
        // Initialize signals
        clk = 0;
        rst = 1;
        
        // Reset the processor
        #10 rst = 0;

        // Simulate instructions (assuming instruction memory is internally managed)
        #50;

        // End simulation
        #100 $finish;
    end

    initial begin
        $monitor("Time = %0t, Reset = %b", $time, rst);
    end
endmodule
