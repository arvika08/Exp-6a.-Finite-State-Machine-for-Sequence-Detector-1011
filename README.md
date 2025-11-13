# Exp-6a, 6b-Finite-State-Machine-for-Sequence-Detector-1011
# Aim 
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required 

Vivado 2023.1 

# Procedure

Launch Vivado 2023.1 Open Vivado and create a new project.
Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO
Create the Testbench Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
Create the Verilog Files Create both the design module and the testbench in the Vivado project.
Run Simulation Run the behavioral simulation to verify the output.
Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.
# Code
# Mealy 1011
## Verilog code
```verilog
module mealysequence(
    input clk,
    input reset,
    input in,
    output reg out
);


    parameter S0 = 3'b000, 
              S1 = 3'b001, 
              S2 = 3'b010, 
              S3 = 3'b011, 
              S4 = 3'b100; 

    reg [2:0] current_state, next_state;

    
    always @(posedge clk or posedge reset) begin
        if (reset)
            current_state <= S0;
        else
            current_state <= next_state;
    end

   
    always @(*) begin
        
        out = 0;
        next_state = S0;

        case (current_state)
            S0: begin
                if (in)
                    next_state = S1;
                else
                    next_state = S0;
            end

            S1: begin
                if (in)
                    next_state = S1;
                else
                    next_state = S2;
            end

            S2: begin
                if (in)
                    next_state = S3; 
                else
                    next_state = S0;
            end

            S3: begin
                if (in) begin
                    next_state = S1;
                    out = 1;         
                end
                else
                    next_state = S2; 
            end

            default: next_state = S0;
        endcase
    end

endmodule
```

## Test bench
```verilog
`timescale 1ns / 1ps

module tb_mealy;

    reg clk, reset, in;
    wire out;

    mealysequence uut (
        .clk(clk),
        .reset(reset),
        .in(in),
        .out(out)
    );

    initial clk = 0;
    always #5 clk = ~clk;

    initial begin
        reset = 1; 
        in = 0;
        #12 reset = 0;

        #10 in = 1;   
        #10 in = 0;   
        #10 in = 1;  
        #10 in = 1;   
        #10 in = 0;
        #10 in = 1;
        #10 in = 0;
        #10 in = 1;
        #10 in = 1;   
        #50 $finish;
    end

    initial begin
        $monitor("Time=%0t | in=%b | out=%b | state=%b", 
                 $time, in, out, uut.current_state);
    end

endmodule
```

# Output Waveform
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/60ee1c89-819d-449e-8dce-446abe52890b" />

# Moore 1011

## Verilog Code
```verilog
`timescale 1ns / 1ps
module moore_seq(
    input clk,
    input reset,
    input in,
    output reg out
);

    parameter  S0 = 3'b000, 
               S1 = 3'b001,  
               S2 = 3'b010, 
               S3 = 3'b011,  
               S4 = 3'b100;  

    reg [2:0] current_state, next_state;


    always @(posedge clk or posedge reset) begin
        if (reset)
            current_state <= S0;
        else
            current_state <= next_state;
    end


    always @(*) begin
        case (current_state)
            S0: begin
                if (in)
                    next_state = S1;
                else
                    next_state = S0;
            end

            S1: begin
                if (in)
                    next_state = S1;   
                else
                    next_state = S2;   
            end

            S2: begin
                if (in)
                    next_state = S3;   
                else
                    next_state = S0;   
            end

            S3: begin
                if (in)
                    next_state = S4;   
                else
                    next_state = S2;   
            end

            S4: begin
                if (in)
                    next_state = S1;   
                else
                    next_state = S2;   
            end

            default: next_state = S0;
        endcase
    end

  
    always @(*) begin
        case (current_state)
            S4: out = 1'b1; 
            default: out = 1'b0;
        endcase
    end
endmodule
```

## Test bench
```verilog
`timescale 1ns / 1ps
module tb_moore_seq;
    reg clk, reset, in;
    wire out;

    moore_seq uut(clk, reset, in, out);


    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        reset = 1;
        in = 0;
        #12 reset = 0;

        in = 1; #10; 
        in = 0; #10;  
        in = 1; #10;  
        in = 1; #10;  
        in = 1; #10;
        in = 0; #10;
        in = 0; #10;
        in = 1; #10;
        in = 1; #10;  
        #20 $finish;
    end

    initial begin
        $monitor("Time=%0t | in=%b | out=%b | state=%b", 
                 $time, in, out, uut.current_state);
    end
endmodule
```

# Output Waveform

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/134af9fb-cbb7-4930-93fa-4ac19de06c66" />


# Conclusion
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.
