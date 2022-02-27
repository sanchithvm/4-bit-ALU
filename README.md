# 4-bit-ALU
This work presents a 4-bit ALU with reusable datapaths. It was designed based on 28nm technology node using Synopsys Custom Compiler and simulated using Primewave.
# Abstract
Arithemetic and Logic Unit (ALU) is the heart of a computer which performs all the arithmetics operations such as addition and subtraction and also logical operations such as AND, OR, XOR. The ALU presented here is a fully combinational with reusable datapath, meaning the ciruits can be reused to perform diferent operations.
# Tools Used
- Synopsys Custom Compiler: The Synopsys Custom Compiler™ design environment is a modern solution for full-custom analog, custom digital, and mixed-signal IC design. As the heart of the Synopsys Custom Design Platform, Custom Compiler provides design entry, simulation management and analysis, and custom layout editing features. This tool was used to design the circuit on a transistor level.
- Synopsys Primewave:  PrimeWave™ Design Environment is a comprehensive and flexible environment for simulation setup and analysis of analog, RF, mixed-signal design, custom-digital and memory designs within the Synopsys Custom Design Platform. This tool helped in various types of simulations of the above designed circuit.
- Synopsys 28nm PDK:  The Synopsys 28nm Process Design Kit(PDK) was used in creation and simulation of the above designed circuit.
# Design
The ALU has 2 4-bit inputs X and Y, one output OUT and 8 control signals zX, zY, nX, nY, L, F, Cs, nOUT. These control signals specify the operation the ALU has to perform. Usually an ALU has N:1 multiplexer which selects the operation based on the select lines. The ALU presented here uses a series of multiplexers to have reusable circuits.
