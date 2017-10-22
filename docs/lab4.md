### Lab 4

## Goals

## Subteams

### Radio Team
- Adrian Higgins Dohmann, Amanda Pathmanathan, JinJie Chen
### FPGA Team
- Sanush Nukshan, Khyati Sipani, Kenneth Huaman
## Procedure
### Radio

### FPGA


For this subsection of the lab, the main component was the DE0 Nano FPGA. It was connected to the Computer Monitor at the lab station via a VGA cable. Later on, both sections were combined when the Arduino from the Radio Subteam was connected to practice communication between the boards.
Starting off from the work of the FPGA subteam of lab 3 last week, the Verilog code was first optimized to work off of two nested for loops. The code was then modified to display the 5x4 grid by extending the iterations through the loop. The grid, like last week, was composed of white tiles amongst a black background.
\\ show modified code to create grid
The code had to be modified additionally to take in the input from an arduino. Our team decided to use parallel communication for the sake of this lab, with the information being sent in two packets. The input from the arduino would give a current x and y position. 
At those coordinates the tile would temporaily be turned green.
//mention code that shows iput,
\\ show video of the grid parsing through the x and y coordinates



