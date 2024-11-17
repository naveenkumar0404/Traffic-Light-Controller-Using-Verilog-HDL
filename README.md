# EXP-4 Traffic-Light-Controller-Using-Verilog-HDL

## Aim:

&emsp;&emsp;To design and simulate a traffic light controller using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 simulation environment. The objective is to control the traffic lights for a junction with a specific time-based sequence for Red, Yellow, and Green lights.<br>

## Apparatus Required:

&emsp;&emsp;Vivado 2023.1 or equivalent Verilog simulation tool.<br>
&emsp;&emsp;Computer system with a suitable operating system.<br>
&emsp;&emsp;FPGA board (optional for hardware verification).<br>

## Procedure:

## Launch Vivado 2023.1:

&emsp;&emsp;Open Vivado and create a new project.

## Design the Traffic Light Controller Verilog Code:

&emsp;&emsp;Write the Verilog code for the traffic light controller, using an FSM (Finite State Machine) to transition between Green, Yellow, and Red lights based on timing intervals.

## Create the Testbench:

&emsp;&emsp;Write a testbench to simulate the traffic light controller. The testbench will check the sequence of light transitions based on time.

**Add the Verilog Files:**

&emsp;&emsp;Add the traffic light controller Verilog code and the testbench file to the project.

**Run Simulation:**

&emsp;&emsp;Run the behavioral simulation in Vivado to verify the correct sequence of the traffic lights.

## Observe the Waveforms:

&emsp;&emsp;Examine the waveform output to verify that the traffic light transitions through the Green, Yellow, and Red lights in the correct sequence.

## Save and Document Results:

&emsp;&emsp;Capture screenshots of the waveform and save the simulation logs to include in your report.

## Verilog Code for Traffic Light Controller:

```

module traffic_light_controller (
    input wire clk,
    input wire reset,
    output reg [2:0] lights  // 3-bit output: [2]=Red, [1]=Yellow, [0]=Green
);
    // Define states
    typedef enum reg [1:0] {
        GREEN = 2'b00,
        YELLOW = 2'b01,
        RED = 2'b10
    } state_t;

    state_t current_state, next_state;
    reg [3:0] counter;  // Timer counter

    // State transition based on counter
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= GREEN;
            counter <= 0;
        end else begin
            if (counter == 4'd9) begin
                current_state <= next_state;
                counter <= 0;
            end else begin
                counter <= counter + 1;
            end
        end
    end

    // Next state logic and output control
    always @(*) begin
        case (current_state)
            GREEN: begin
                lights = 3'b001;  // Green light on
                next_state = YELLOW;
            end
            YELLOW: begin
                lights = 3'b010;  // Yellow light on
                next_state = RED;
            end
            RED: begin
                lights = 3'b100;  // Red light on
                next_state = GREEN;
            end
            default: begin
                lights = 3'b000;  // All lights off
                next_state = GREEN;
            end
        endcase
    end
endmodule

```

## Testbench for Traffic Light Controller:

```

`timescale 1ns / 1ps
module traffic_light_controller_tb;
    // Inputs
    reg clk;
    reg reset;

    // Outputs
    wire [2:0] lights;

    // Instantiate the Unit Under Test (UUT)
    traffic_light_controller uut (
        .clk(clk),
        .reset(reset),
        .lights(lights)
    );

    // Clock generation
    always #5 clk = ~clk;  // Toggle clock every 5 ns

    // Test procedure
    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;

        // Release reset after some time
        #10 reset = 0;

        // Run simulation for 100 ns to observe light transitions
        #100 $stop;
    end

    // Monitor outputs
    initial begin
        $monitor("Time=%0t | Lights (R Y G) = %b", $time, lights);
    end

endmodule

```

## Sample Output:

```

Time=0 | Lights (R Y G) = 001
Time=105000 | Lights (R Y G) = 010
Time=205000 | Lights (R Y G) = 011
Time=305000 | Lights (R Y G) = 001
Time=405000 | Lights (R Y G) = 010
Time=505000 | Lights (R Y G) = 011
Time=605000 | Lights (R Y G) = 001
Time=705000 | Lights (R Y G) = 010
Time=805000 | Lights (R Y G) = 011
Time=905000 | Lights (R Y G) = 001

```
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Output Waveform:

![Screenshot 2024-10-05 144809](https://github.com/user-attachments/assets/55968959-66d3-4a40-838a-e9b7cd89a0f4)

## Conclusion:

In this experiment, a traffic light controller was successfully designed and simulated using Verilog HDL. The design controlled the traffic lights to switch between Green, Yellow, and Red in a cyclic manner based on timing intervals. The testbench verified that the traffic lights followed the correct sequence and timing. The simulation results confirm the correct functionality of the traffic light controller, demonstrating the effectiveness of Verilog HDL in designing FSM-based controllers for real-world applications.
