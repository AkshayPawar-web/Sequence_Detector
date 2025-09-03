# Sequence Detector "111" (Verilog HDL)

## 📌 Introduction  
This project implements a **non-overlapping sequence detector** for detecting the bit sequence **`111`** using a **Moore Finite State Machine (FSM)** in Verilog HDL. The detector asserts output `z=1` when three consecutive `1`s appear in the input stream.

---

## 🎯 Objectives  
- Design a **FSM** for detecting `111`.  
- Implement in **Verilog HDL**.  
- Simulate and verify functionality using a testbench.  
- Analyze results with **waveform output** and **RTL schematic**.  

---

## 📖 Theory  
- **FSM Type:** Moore Machine  
- **States Used:**  
  - `S0` → Initial state / No `1` detected  
  - `S1` → One `1` detected  
  - `S2` → Two consecutive `1`s detected  
  - `S3` → Three consecutive `1`s detected → `z=1`  

👉 Since it is **non-overlapping**, once the sequence `111` is detected, FSM resets to `S0`.

---

## 📝 Verilog Code  

### `sequence_detector.v`
```verilog
module sequence_detector(
    input clk, reset, x,
    output reg z
);
    reg [1:0] state, next_state;

    parameter S0=2'b00, S1=2'b01, S2=2'b10, S3=2'b11;

    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else
            state <= next_state;
    end

    always @(*) begin
        case (state)
            S0: next_state = x ? S1 : S0;
            S1: next_state = x ? S2 : S0;
            S2: next_state = x ? S3 : S0;
            S3: next_state = x ? S0 : S0; // Non-overlapping
            default: next_state = S0;
        endcase
    end

    always @(*) begin
        case (state)
            S3: z = 1;
            default: z = 0;
        endcase
    end
endmodule
