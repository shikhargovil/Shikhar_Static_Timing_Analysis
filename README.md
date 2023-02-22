# Shikhar_Static_Timing_Analysis
#### This repository contains the whole summary of hands on done by SHIKHAR GOVIL during the workshop SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The full STA flow was implemented using OpenSTA with SKY130nm PDK.
![image](https://user-images.githubusercontent.com/78219141/220262083-d68bac42-f205-4ee9-96f6-6c21782687d0.png)
#### Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. STA breaks a design down into timing paths, calculates the signal propagation delay along each path, and checks for violations of timing constraints inside the design and at the input/output interface. The workshop covers all the basic concepts in STA and Timing constraints.
#### It starts with basics of Static Timing Analysis, timing paths, startpoint, endpoint and combinational logic definitions. It explains setup and hold checks, how STA tools calculate setup and hold violations. Then it slowly builds up to cover all aspects of STA like multiple types of timing paths, design rule checks, checks on async pins and clock gates. After that we go into slightly advanced topics like Time borrowing on latches, timing arcs, cell delays and models, impact of clock network on STA. Since STA and timing constraints go hand in hand the workshop covers basics of all the timing constraints that an engineer should know for STA like clock definitions, clock groups, clock characteristics, port delays and timing exceptions.
## DAY 1:
##### STA is the method of verifying timing performance of a design. It does not use test benches and does not check the functionality. It uses mathematical techniques instead of input vectors. Inputs to STA are gate level netlist, constraints file and logical libraries (delays, transition time, etc.,). STA breaks the design at ports and sequential elements (Path can be FF to port(I/O), FF to FF or port to FF). Timing elements includes, start point (usually input ports or reg clock pins), end point (usually output ports or reg data pins). Setup and hold time: The minimum amount of time, the data should be available(stable) at the input of sequential circuit before clock edge in the data path is called setup time. This enforces maximum delay. The minimum amount of time, the data should be stable at the input of sequential device after clock edge that captures data. This enforces minimum delay.

##### Setup slack calculation, Setup slack = data required time – data arrival time Arrival time should always be less than the required time. Slack value should be positive. If we get negative value, then we need to fix.

### OpenSTA LAB I

##### OpenSTA is a static analysis tool, used to verify the timing of the design. Incremental updating of delays, arrivals, and required times are done using queries. This analysis provides timing checks -from, -through, -to, multiple paths to endpoint, delay calculation and also checks timing setup Following image shows the steps involved to open Lab1,
![image](https://user-images.githubusercontent.com/78219141/220632157-310d081b-676c-4a0d-b68c-69d0e9977e1c.png)
##### Simple.v is our design file. The file ‘sky130_fd_sc_hd_tt_025C_1v80.lib’ include library cell information and inside this file, we have cell named ‘sky130_fd_sc_hd__nand2_1’ which is used in our design (simple.v).
![image](https://user-images.githubusercontent.com/78219141/220632256-c3f2ee7f-3ff1-4a5a-aec8-b35115784437.png)
![image](https://user-images.githubusercontent.com/78219141/220632291-6671cc5e-7be4-4454-9559-e2f82f463d4b.png)
#### Run File:
![image](https://user-images.githubusercontent.com/78219141/220632381-6b1e6b14-988b-483c-91a5-6984a52b5377.png)
#### Constraints creation: Sdc file consist of design and timing constrains(rising and falling edge of input/output delays and transitions and load values) as below, 
![image](https://user-images.githubusercontent.com/78219141/220632433-b9da4f5e-c827-4fbb-9e8b-5f5dd15064d2.png)
#### Execution of run file (run.tcl), Command - image
![image](https://user-images.githubusercontent.com/78219141/220632820-2ab80bf0-f8de-45ac-bf1f-e48a7687ba2e.png)

## DAY 2:

#####  Apart from setup and hold check, there are also other timing checks such as clock gating checks (happens on enable clk pin w.r.t clk), asyn pin check (ensure when the reset can de-assert w.r.t clk), data to data checks (between two skews there should be certain time). STA provides some design rule checks such as slew/transition analysis, Load analysis, clock skew analysis, pulse width check.

##### Slew/transition analysis: Rise slew is the time taken by the signal when it rises from 30% Vdd to 70% Vdd. Fall slew is the time taken to fall from 70% Vdd to 30% Vdd (from logic 0 to 1). We can specify specific constraints in order to ensure that these slew values have max and min values. In this analysis, STA will check whether the slew values are maintained throughout the design. Load Analysis: We can specify min and max capacitance on ports and nets, fanout loads on ports and I/O pins. Clock Skew analysis: It's also important to consider the skew between the launch and capture clock waveforms; the skew is positive if the capture flop clock leads the launch flop clock and the skew is negative if the launch clock leads the capture clock. Latch Timing: In flops, the data is launched and captured at the edges, whereas latched based designs allow more flexibility in timing. Data will be accepted at any time before the latch closes. Time borrowing is possible in latch. If one logic has more delay and other logic has less delay, then the time is borrowed from the logic which has less delay (next clock cycle).

![image](https://user-images.githubusercontent.com/78219141/220633643-ab07953a-08be-4f7a-80d6-21383d724bc2.png)

##### STA Text Report Evaluation: STA tool will convert gate level representation to node and arc level representation. The diagram shows the discription of timing analysis report.

![image](https://user-images.githubusercontent.com/78219141/220633705-624725ce-585c-4775-ad66-8c92c3e43128.png)


### OpenSTA LAB II

##### The .lib file is an ASCII representation of the timing and power parameters associated with any cell in a particular semiconductor technology. Lib files contain I/O delay paths, Timing check values, Interconnect delays.

![image](https://user-images.githubusercontent.com/78219141/220633864-135d9002-7702-4aee-a014-a4535cd860cf.png)

![image](https://user-images.githubusercontent.com/78219141/220633891-e937c0b8-75ee-4f70-856d-dbebb9ffb4c2.png)

##### 1) No of cells in simple_max file: command used grep -c “cell “ simple_max.lib Ans: 211 (INV (X,Y,Z) = 30 + NAND2,NAND3,NAND4(X,Y,Z) = 120 + NOR2,NOR3,NOR4 (X,Y,Z) = 120, DFF_X80 = 1)

![image](https://user-images.githubusercontent.com/78219141/220633966-4fd9bd8a-229b-468e-bbac-18e3dfcaba29.png)

##### 2) Pins of cell NAND2_X1 in simple_max.lib: Ans: 3 (Pin o,a,b)

![image](https://user-images.githubusercontent.com/78219141/220634067-63696bee-2d6a-4c0f-b397-09319ae3dd43.png)
##### 3) Difference between NAND2_X1 and NAND3_X1 Ans: NAND2_X1 – Capacitance value is 6.40 NAND3_X1 – Capacitance value is 4.27
##### 4) Difference between simple_min.lib and simple_max.lib Ans: Fabrication process variations could either increase or decrease the delay of a cell. Library files will have min and max values of delay which need to be considered for timing analysis. STA tool will make use of these libraries for analysis.

##### Standard Parasitic Exchange format (SPEF):

##### Describes parasitic information (electronic info like connectivity, capacitance, resistance and Inductance) of the design. SPEF file has 4 main sections, header, name map, top level ports, parasitic description.

![image](https://user-images.githubusercontent.com/78219141/220634457-cd3bc5bf-b770-403f-8fa2-6fadc42647f0.png)

##### Understanding STA text report,

![image](https://user-images.githubusercontent.com/78219141/220634566-b46359b7-e5f1-41c1-9b04-89a7b14a230b.png)

## DAY 3:

##### Multiple clock: When there are multiple clocks with different frequencies, setup check is calculated by expanding the clock to common base period. STA used the most restrictive setup time. There are two rules for hold check. Rule1: Data launched at setup launch edge must not be captured by previous capture edge. Rule2: Data launched at next launch edge must not be captured by current setup capture edge.

##### Timing Arc and Timing Sense: Timing arcs are of two types, 1- cell arcs (input to output), 2- net arcs (one cell to another cell).

![image](https://user-images.githubusercontent.com/78219141/220634870-0f053b5c-a712-4fdf-8ef5-93e52fc13aea.png)

##### Sequential arcs:
![image](https://user-images.githubusercontent.com/78219141/220634964-42bcaa50-b9fd-415e-8524-dc1f889f8851.png)

##### Combinational Arcs: Input A is connected to output and input B is connected to output and no state element is involved.

##### Relation between combinational/sequential/cell and net arcs: Combinational arcs: Combinational arcs represent the propagation delay of signals through the logic gates and are typically modeled as logical functions or Boolean expressions. Combinational arc is where data can propagate combinational without needing any enabling signal typically clock.

##### Cell Arc: A cell arc represents the delay through a single logic cell or gate, such as an AND gate or an inverter. Cell arcs are typically characterized by the delay of the cell for different input combinations and are used in the delay calculation of the combinational arcs. In STA, both types of delays are used to analyze the timing behavior of a digital circuit. So, a cell arc can be both combinational or sequential and sometimes both for complex cells

##### Combinational delays are used to calculate the critical path delay, while cell delays are used to calculate the delay of individual gates or cells in the circuit. The critical path is the longest path of combinational logic that determines the maximum operating frequency of the circuit.

##### Combinational arcs in Static Timing Analysis include the net delays between sequential elements and the combinational logic that connects them. Cell arcs, on the other hand, are a subset of combinational arcs and refer to the delay through a single logic cell or gate.

##### In other words, a combinational arc represents the delay through the entire path that connects two sequential elements, which includes the delays of all the logic gates and wires in the path. The delay of a combinational arc is the sum of the cell delays of all the gates along the path, as well as the delay of the wires that connect the gates.

##### Cell arcs are used to model the delay of individual gates or cells in the circuit, and are a key component of the delay calculation for the combinational arcs. The cell delay is typically measured by characterization, which involves measuring the delay of the gate or cell for different input patterns and loading conditions. So, while cell arcs are a subset of combinational arcs, they are also distinct in that they represent the delay through a single gate or cell, whereas a combinational arc represents the delay through an entire path of logic gates and wires.

##### Timing sense: Positive unate arc (output follows input), Negative unate arc (output followes the input in opposite direction), Non-unate arc (when we can’t predict if the output is going to follow the input or not).

##### Cell Delays: Cell delays calculations are the function of input transition (propagation delay), output load/capacitance.

##### Positive clock slew: Capture clock delay is more. This skew helps in making the time to meet setup time. Negative clock skew: Capture clock delay is less. Less time window to meet setup time. Clock latency: clock source latency, network latency.

##### Clock jitter: Practically, there will be more uncertainty in clock edges.

![image](https://user-images.githubusercontent.com/78219141/220635395-6aa16f9c-7633-472f-9f9a-474ccddcbdd8.png)
##### Setup and hold in detained: Setup check:
![image](https://user-images.githubusercontent.com/78219141/220635460-928e4211-3c1c-42d7-b9a7-61aaca43802e.png)

##### Setup check : Tc2q + Tcomb + Tsetup <= TPeriod + Tskew - Su Su = setup clock uncertainity

##### Hold Check: Tc2q + Tcomb + Tsetup >= TPeriod + Tskew + Hu Hu = hold clock uncertainty

##### Different Delay value on path – setup check: Path delay can have min, max or nominal value. STA to make setup pessimistic, uses max delay value on data path in launch side and min delay value in capture side.

##### Different Delay value on path – hold check: STA uses min delay value on data path in launch side and max delay value in capture side.

### OpenSTA LAB III

##### The below circuit id given for slack calculation,
![image](https://user-images.githubusercontent.com/78219141/220635911-80b940f6-2e9f-41d2-8974-e83ae59025c2.png)
##### Understanding slack calcuation,
![image](https://user-images.githubusercontent.com/78219141/220636000-4458e70d-35dd-4395-ba05-24f82886547a.png)

##### report_checks–from F1/CK -endpoint_count100

##### STA will report 1 path per end point.
![image](https://user-images.githubusercontent.com/78219141/220636092-f668036d-dc11-45e9-b542-89305a3722dd.png)

##### After executing the above run file (command - sta run.tcl–exit | tee run.log), the following image shows the slack claculation obtained for 8 path.

![image](https://user-images.githubusercontent.com/78219141/220636176-047032b3-f022-4a0a-b2d2-d2206f4bea45.png)

## DAY 4:

##### Crosstalk and noise: As wires are closely connected, coupling capacitance was introduced. When the signal change (1 to 0) in one node (aggressor) affects the nearby node (victim) that is, aggressor forces the victim to change in the same direction as it changes. If the requirement of the victim node is same (1 to 0), then this causes victim signal to change faster because of which reduces delay. When the victim signal is changing in opposite direction, aggressor will try to push the victim in the same direction because of which the change is slow and this causes more delay.

##### If there exist signal change in aggressor but no change or output in the victim side, there can be slight glitch/pulse in victim side which can cause functional failure.

##### Impact of crosstalk and noise: delay, functional failure.

##### STA must make sure that these are not present or present below threshold value. Other global variations like inter-die variations (variation in delay in different chip in the same wafer or different wafer) and intra-die variation (variation in delay within single chip). There are logical libraries with all this info which STA uses.

##### Clock gating check: It is done if a signal can control the path of the clock in a cell. If clock feed a flop/latch clk pin, feed output port, feed generated clk, clk gating check is done. • Active high clock gating check (occurs in AND and NAND cell) • Active low clock gating check (occurs in OR and NOR cell) • If there exist other cells apart from mentioned in above points (complex cell), the tool will issue a warning and does not check.

![image](https://user-images.githubusercontent.com/78219141/220636804-d5cfca5c-e737-46e8-ac07-66f4fb60b348.png)

##### Check on Async pin: Assertion is asynchronous event and no relation with clock. (eg: clear pin). De-assertion causes flop to become dependent on clock (timing checks needed, to avoid unknown state).


### OpenSTA LAB IV

Clock gating checks Design file,
![image](https://user-images.githubusercontent.com/78219141/220637069-c1550640-c5bd-4212-89a0-7e6ceb9abd72.png)

##### Run file,
![image](https://user-images.githubusercontent.com/78219141/220637192-d933fe52-053a-4b06-9a8a-496743a934fd.png)

##### Sdc file,
![image](https://user-images.githubusercontent.com/78219141/220637269-671e26e4-7a82-4b70-94cc-4e25712acd93.png)

##### OUTPUT:
![image](https://user-images.githubusercontent.com/78219141/220637342-b329bb0c-8d6d-42d7-8eae-f7524129800e.png)

##### Async pin check: Design file,
![image](https://user-images.githubusercontent.com/78219141/220637438-4f2363e2-0d81-41e7-b35e-d1dd9844921c.png)

##### Run file,
![image](https://user-images.githubusercontent.com/78219141/220637528-4911778a-1d7b-45b7-ae8d-ddcf0d7670cc.png)
 
##### OUTPUT:
![image](https://user-images.githubusercontent.com/78219141/220637649-f0a38b91-4df5-402d-9e2b-132ee7c6fc78.png)


## DAY 5:

##### Clock group commands are used to tell STA tool between what clock you have to meet timing and between what clock you don’t have to meet timing. Sometimes it is not necessary to meet timing between all clocks. Synchronous clock, asynchronous clock, logically exclusive clock, physically exclusive clock. Command: Set_clock_group -asynchronous -group{clk1 clk2 clk3} -group{clk4 clk5 clk6}
![image](https://user-images.githubusercontent.com/78219141/220637881-9b989dbe-093a-4205-97d7-a85e8d0f8cba.png)

##### Timing Exception: Path specification: If we want to change the default behavior of this timing on path, we need specify on which path we are changing the behavior. (from, to, through) Timing exception commands, set_false_path  no timing check should be done. set_multicycle_path If we want to modify the timing relationship (no. of cycles). set_max_delay  setup check (modify time check to specific delay value) set_min_delay  hold check set_disable_timing  used for disabling arcs in a cell. set_case_analysis  used to specify constant value in netlist that may or may not present, we can also overwrite with this command. (eg: in mux, two inputs are clks, selection line can be set using this command).


### OpenSTA LAB IV

##### Common Path Pessimism Removal(CPPR),
![image](https://user-images.githubusercontent.com/78219141/220638027-49f383aa-de5d-4925-bf72-445368022ba8.png)

##### Slack calculation without CPPR, Design file,
![image](https://user-images.githubusercontent.com/78219141/220638157-5f71183c-86f6-42e9-962b-de905e4bf272.png)

##### SDC file,
![image](https://user-images.githubusercontent.com/78219141/220638305-c3a38bf4-158d-482f-a9e6-3f9d9155d5f9.png)

##### OUTPUT: execute run.tcl file.
![image](https://user-images.githubusercontent.com/78219141/220638426-88b678c5-b113-404f-8e64-79a2f7f8deb9.png)

![image](https://user-images.githubusercontent.com/78219141/220638462-be114063-e24f-4486-8bd0-031fb807dc14.png)

##### Slack calculation with CPPR,

##### ‘c2’ is node which requires CPPR, change the run file. Command- set sta_crpr_enabled 1
![image](https://user-images.githubusercontent.com/78219141/220638646-c37858f2-6146-43f0-965b-1588f8bc606a.png)

##### OUTPUT:
![image](https://user-images.githubusercontent.com/78219141/220638764-b7443fb4-733e-4f55-89dd-65608ec6b9da.png)

## References
##### 1) OpenSTA Manual
##### 2) OpenSTA installation guide
##### 3)https://github.com/Anmol-wq/VSD-IAT-Sign-off-Timing-Analysis---Basics-to-Advanced

## Acknowledgement
##### Finally, I would like to express my sincere gratitude to Kunal Ghosh{Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.} and Vikas Sachdeva {Senior Director, Product Strategy and Business Development at Real Intent} for tremendous assistance in planning and presenting this workshop on SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The workshop was excellent and well designed, this workshop taught me a lot of new things about the design timing analysis using OpenSTA, open source tool.   
