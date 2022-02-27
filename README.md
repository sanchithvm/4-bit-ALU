# 4-bit-ALU
This work presents a 4-bit ALU with reusable datapaths. It was designed based on 28nm technology node using Synopsys Custom Compiler and simulated using Primewave.
## Abstract
Arithemetic and Logic Unit (ALU) is the heart of a computer which performs all the arithmetics operations such as addition and subtraction and also logical operations such as AND, OR, XOR. The ALU presented here is a fully combinational with reusable datapath, meaning the ciruits can be reused to perform diferent operations.
## Tools Used
- Synopsys Custom Compiler: The Synopsys Custom Compiler™ design environment is a modern solution for full-custom analog, custom digital, and mixed-signal IC design. As the heart of the Synopsys Custom Design Platform, Custom Compiler provides design entry, simulation management and analysis, and custom layout editing features. This tool was used to design the circuit on a transistor level.
- Synopsys Primewave:  PrimeWave™ Design Environment is a comprehensive and flexible environment for simulation setup and analysis of analog, RF, mixed-signal design, custom-digital and memory designs within the Synopsys Custom Design Platform. This tool helped in various types of simulations of the above designed circuit.
- Synopsys 28nm PDK:  The Synopsys 28nm Process Design Kit(PDK) was used in creation and simulation of the above designed circuit.
## Design
The ALU has 2 4-bit inputs X and Y, one output OUT and 8 control signals zX, zY, nX, nY, L, F, Cs, nOUT. These control signals specify the operation the ALU has to perform. Usually an ALU has N:1 multiplexer which selects the operation based on the select lines. The ALU presented here uses a series of multiplexers to have reusable circuits. The multiplexers are designed using transmission gate to reduce power consumption.<br>
The control signals specify a type of operation:
- zX and zY: If HIGH then it passes 0 to next stage
- nX and nY: If HIGH then it passes inverted input to next stage
- L: Specifies if AND / XOR gate is selected
- F: Specifies if logical operation or adder has to be selected
- Cs: Specifies if carry_in needs to be used or not
- nOUT: If HIGH then it passes inverted input to output<br><br>
For example, let us take the example of AND operation, this is done by making: zX and zY low (to pass actual inputs), nX and nY low (to not invert them), L and F low (to select AND operation), Cs low (not applicable for current example) and nOUT low (to not invert them). <br>
To do NAND, take the same values of the control signals but make nOUT HIGH to invert the ANDed value.<br>
To do OR operation, we will use De-Morgans Law : ~(~a.~b) = a|b <br>
Hence the control signals will be zX=zY=0, nX=nY=1, L=F=0,Cs=0, nOUT=1.<br>
The below table has a list of common operations and the values of control signals corresponding to it:
<table>
  <tr>
    <th> zX </th>
    <th> zY </th>
    <th> nX </th>
    <th> nY </th>
    <th> L </th>
    <th> F </th>
    <th> Cs </th>
    <th> nOUT </th>
    <th> OPERATION </th>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> X&Y</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> ~(X&Y)</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> X|Y</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> ~(X|Y)</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> X^Y</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> ~(X^Y)</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> X+Y</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> X+Y+c</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> X-Y</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> Y-X (borrow)</td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> -X (borrow)</td>
  </tr>
  <tr>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> Y+1</td>
  </tr>
  <tr>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> X-1</td>
  </tr>
</table>
and many more
<h2> Schematic </h2>
<h2> Netlist </h2>
<h2> Waveform </h2>
<h2> Author </h2>
<h2> Acknowledgements </h2>
<ul>
  <li> VSD </li>
  <li> Synopsys India </li>
  <li> IIT Hyderabad </li>
  <li> Chinmay Panda, IIT-H </li>
  <li> Sameer Durgoji </li>
  </ul>
<h2> References</h2>



  

