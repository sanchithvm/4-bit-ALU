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
<img src="https://user-images.githubusercontent.com/75156759/155886829-5a7065a0-2bf9-46f7-b8f3-f6e0c293c819.png">

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
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> -Y </td>
  </tr>
  <tr>
    <td> 0</td>
    <td> 1</td>
    <td> 1</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> X+1</td>
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
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> X-1</td>
  </tr>
  <tr>
    <td> 1</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> 1</td>
    <td> 0</td>
    <td> 0</td>
    <td> Y-1</td>
  </tr>
</table>
and many more
<h2> Schematic </h2>
Inverter
<img src = "https://user-images.githubusercontent.com/75156759/155886689-7efc2acd-830e-428f-abf7-3282154dc0a3.png">
XOR gate
<img src="https://user-images.githubusercontent.com/75156759/155886537-cbd83d45-699b-4afe-9503-44ca8c15c723.png">
2:1 Mux Schematic
<img src="https://user-images.githubusercontent.com/75156759/155886510-fd2391d9-8003-437a-82e4-f25c83314468.png">
Full Adder
<img src="https://user-images.githubusercontent.com/75156759/155886564-62a42eaa-e9fe-40e0-bd91-0e3a551c6c7b.png">
ALU 1-bit
<img src="https://user-images.githubusercontent.com/75156759/156156517-1b81a95f-b0c9-4649-a689-f407cab5aa43.png">
ALU 4-bit
<img src="https://user-images.githubusercontent.com/75156759/156156601-269921b8-e7db-48d9-be22-fcfc5aa16ab6.png">
<h2> Netlist </h2>
<h2> Waveform </h2>
For Simulation, I have given X=4'b1001 and Y=4'0101 and the output is seen in {o3,o2,o1,o0}. I have split the simulation into 3 parts - logical, add/aub and inc/dec and gave different inputs to simulate the different operations. For easy reference, I have added labels indicating the operation in the waveforms.
<h3>Logical Operations</h3>
 Also F and Cs are LOW to simulate logical operations. The waveform for logical operations is below. 
Below are the schematic and waveform.
<img src="https://user-images.githubusercontent.com/75156759/156156756-e70bf3c0-69f4-4a9b-9d93-feee43d20849.png">
<br>
<img src="https://user-images.githubusercontent.com/75156759/156156980-b0e8429e-7b21-4cba-b990-f9045ad9691e.png">
<br>
<h3>Add/Sub Operations</h3>
I have made F=1, Cin=1 and L=0. Below are schematic and waveform.
<img src="https://user-images.githubusercontent.com/75156759/156157558-8050a567-20b5-4553-9e0a-a33301f07417.png">
<br>
<img src="https://user-images.githubusercontent.com/75156759/156157646-5e095ab4-11fe-481e-a44b-704991178087.png">
<h3>INC/DEC Operations</h3>
Here L=0, F=1, Cin=0 and Cs=0. Below are schematic and waveform.
<img src="https://user-images.githubusercontent.com/75156759/156158294-6f1b0b19-6a56-4e3b-af6d-6e06c3dcffc1.png">
<br>
<img src="https://user-images.githubusercontent.com/75156759/156158382-f1bf0df8-b953-4468-a9de-7829d3ee001a.png">
<br>
We can ignore the spikes in actual CPU because the data will be held in flipflops and these spikes do not conform to setup and hold times, hence will not cause any issues.
<h2> Author </h2>
<a href="https://www.linkedin.com/in/sanchith-v-m-b70a061bb/">Sanchith V M</a>
<h2> Acknowledgements </h2>
<ul>
  <li><a href="https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/"> Kunal Ghosh, VSD</a> </li>
  <li> <a href="https://www.synopsys.com/">Synopsys India </a></li>
  <li> <a href="https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/">IIT Hyderabad</a> </li>
  <li> <a href="https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/"> Chinmay Panda, IIT-H</a> </li>
  <li> <a href="https://www.linkedin.com/in/sumanto-kar-0424391a9/"> Sumanto Kar </a></li>
  <li> <a href="https://www.linkedin.com/in/sameer-s-durgoji-340b26180/">Sameer Durgoji, NIT-K </a></li>
  </ul>
<h2> References</h2>
1. Sanchith V M , Prashanth K S , Madhvesh M Ballal , Pawan Bharadwaj, 2021, Low Power Microprocessor Design, INTERNATIONAL JOURNAL OF ENGINEERING RESEARCH & TECHNOLOGY (IJERT) Volume 10, Issue 07 (July 2021).
<br><br>
2. Subodh Wairya, Rajendra Kumar Nagaria, Sudarshan Tiwari, "Performance Analysis of High Speed Hybrid CMOS Full Adder Circuits for Low Voltage VLSI Design", VLSI Design, vol. 2012, Article ID 173079, 18 pages, 2012. https://doi.org/10.1155/2012/173079
<br><br>
3. V. Venkata Roa, et al., Design and Simulation of 1-bit Arithmetic Logic Unit using Pass Transistor Logic Families, International Journal of Pure and Applied Mathematics, Volume 120, No. 6 2018
