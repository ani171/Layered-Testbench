# Verification of a Synchronous 4-Bit Adder Using a Layered Testbench

## Overview
This repository presents the design verification of a synchronous 4-bit adder using a SystemVerilog layered testbench architecture. The work demonstrates fundamental verification concepts such as stimulus generation, monitoring, and result checking in a structured and reusable manner.
## Types of Verification 
In digital IC design, verification ensures that the RTL behaves as intended. Common verification approaches include:
- Directed Testing: Predefined input vectors are manually written to test specific scenarios. Simple but limited in coverage.
- Constrained Random Verification (CRV): Inputs are generated randomly within constraints to explore a large input space and catch corner cases.
- Assertion-Based Verification (ABV): Assertions are used to check protocol rules and functional properties during simulation.
- Coverage-Driven Verification: Functional and code coverage metrics guide stimulus generation to achieve completeness.
- Layered / Modular Testbench Verification: Testbench components such as generator, driver, monitor, and scoreboard are separated into layers for clarity, reuse, and scalability.

#### Verification Type Used in This Project

This project employs a layered testbench–based functional verification approach using SystemVerilog. It combines:
    - Transaction-level stimulus
    - Modular testbench components
    - Self-checking using a scoreboard

## Synchronous 4-Bit Adder Description

- The DUT is a synchronous 4-bit adder with the following characteristics:
- Inputs: `a[3:0], b[3:0], clk, reset`
- Functionality: Computes a + b using combinational logic. The sum is registered and updated on the clock edge
- Output: `c[4:0]` (includes carry-out)
- Reset clears the output register
<img width="1042" height="435" alt="image" src="https://github.com/user-attachments/assets/68923812-14ac-4db1-b7f7-7d9ac0982b86" />

## Layered Testbench Architecture

- The testbench is organized into independent layers, each with a clear responsibility. This improves readability, maintainability, and reusability.
Testbench Components Used
1. Transaction (`transaction.sv`)
    - Defines a transaction object containing:
        - Input operands (a, b)
        - Expected result
        - Acts as the data packet exchanged between testbench components

2. Generator (`generator.sv`)
    - Generates randomized or directed transactions
    - Sends transactions to the driver
    - Controls stimulus flow

3. Interface (`interface.sv`)
    - Connects the testbench to the DUT
    - Encapsulates signals like a, b, clk, reset, and c
    - Provides a clean abstraction for signal access

4. Driver (`driver.sv`)
    - Drives input transactions onto the DUT through the interface
    - Synchronizes stimulus with the clock
    - Applies reset and valid inputs

5. Monitor (`monitor.sv`)
    - Observes DUT inputs and outputs
    - Converts signal-level activity back into transactions
    - Sends observed results to the scoreboard

6. Scoreboard (`scoreboard.sv`)
    - Compares DUT output with expected results
    - Reports pass/fail status
    - Enables self-checking verification

7. Environment (`environment.sv`)
    - Instantiates and connects the generator, driver, monitor, and scoreboard
    - Manages overall testbench execution

8. Program Block (`program.sv`)
    - Ensures correct scheduling between the testbench and DUT
    - Avoids race conditions in simulation

9. Top Module (`top.sv`)
    - Instantiates:
        - DUT
        - Interface
        - Testbench environment
    - Starts the simulation
