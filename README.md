# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
// 4-bit Ripple Carry Adder using Task
```
`timescale 1ns/1ps

module ripple_carry_adder_tb;
    reg  [9:0] A, B;
    reg        Cin;
    reg  [9:0] Sum;
    reg        Cout;

    // Task to perform full adder logic for one bit
    task full_adder_task;
        input  a, b, cin;
        output sum, cout;
        begin
            sum  = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (a & cin);
        end
    endtask

    // Task to perform 10-bit ripple carry addition
    task ripple_carry_add_10bit;
        input  [9:0] a, b;
        input        cin;
        output [9:0] sum;
        output       cout;
        reg    [10:0] c;
        integer i;
        begin
            c[0] = cin;
            for (i = 0; i < 10; i = i + 1) begin
                full_adder_task(a[i], b[i], c[i], sum[i], c[i+1]);
            end
            cout = c[10];
        end
    endtask
```



# Test Bench
```


    initial begin
        $display("Time | A        B        Cin | Sum      Cout");
        $display("-----------------------------------------------");

        A = 10'd5; B = 10'd7; Cin = 0;
        ripple_carry_add_10bit(A, B, Cin, Sum, Cout);
        #10 $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A = 10'd100; B = 10'd200; Cin = 1;
        ripple_carry_add_10bit(A, B, Cin, Sum, Cout);
        #10 $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A = 10'd1023; B = 10'd1; Cin = 0;
        ripple_carry_add_10bit(A, B, Cin, Sum, Cout);
        #10 $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A = 10'd512; B = 10'd511; Cin = 1;
        ripple_carry_add_10bit(A, B, Cin, Sum, Cout);
        #10 $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        $finish;
    end
endmodule
```

# Output Waveform
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/46e7bfc5-c6b3-49f4-9066-1ff6c562346c" />


# 4 bit Ripple counter using Function
// 4-bit Ripple Counter using Function
```
`timescale 1ns/1ps

module ripple_carry_adder_functional (
    input  wire [9:0] a,
    input  wire [9:0] b,
    input  wire       cin,
    output reg  [9:0] sum,
    output reg        cout
);

    // Function to perform 1-bit full adder logic
    function [1:0] full_adder_func;
        input a, b, cin;
        begin
            full_adder_func[0] = a ^ b ^ cin;                      // sum
            full_adder_func[1] = (a & b) | (b & cin) | (a & cin);  // carry
        end
    endfunction

    integer i;
    reg [10:0] carry;

    always @(*) begin
        carry[0] = cin;
        for (i = 0; i < 10; i = i + 1) begin
            {carry[i+1], sum[i]} = full_adder_func(a[i], b[i], carry[i]);
        end
        cout = carry[10];
    end

endmodule
```

# Test Bench
```
module ripple_carry_adder_tb;
    reg  [9:0] A, B;
    reg        Cin;
    wire [9:0] Sum;
    wire       Cout;

    ripple_carry_adder_functional uut (
        .a(A), .b(B), .cin(Cin), .sum(Sum), .cout(Cout)
    );

    initial begin
        $display("Time | A        B        Cin | Sum      Cout");
        $display("-----------------------------------------------");

        A=10'd0; B=10'd0; Cin=0; #10;
        $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A=10'd5; B=10'd7; Cin=0; #10;
        $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A=10'd100; B=10'd200; Cin=1; #10;
        $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A=10'd1023; B=10'd1; Cin=0; #10;
        $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        A=10'd512; B=10'd511; Cin=1; #10;
        $display("%0t | %d %d %b | %d %b", $time, A, B, Cin, Sum, Cout);

        $finish;
    end
endmodule
```


# Output Waveform 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7157e75e-f3b1-4647-b4fc-91e5cd1d6702" />


# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.
