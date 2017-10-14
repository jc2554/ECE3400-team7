# Lab 3

## Goals:
The goal of the Graphic subteam was to build a FPGA base station that will display a simplified version of the final maze grid by writing a VGA controller. 

The goal of the Acoustic subteam 


## Sub- teams:  
### Graphic
1. JinJie Chen; Adrian Higgins Dohmann; Amanda Pathmanathan
### Acoustic
2. Khyati Sipani; Sanush Nukshan Kehelella; Kenneth Huaman

## Lab Procedure

### Graphic
Generating the input signal
First, we wanted to create a 2 by 2 grid output on the display. We declared a 2 by 2 array of bits represetn the state of the grid, and  the highlighted_x and  square:


We decided to connect two toggle switches to the FPGA and show the change in a 2-by-2 grid on the screen. Later, this code can be expanded to display the full maze. The following figure shows an overview of our system:

Team Assignment

First, we declared our 2-by-2 array of bits, and switch 1 to control the x-coordinate and switch 2 to control the y-coordinate of the highlighted square:

[![grid video](./image/lab3/grid.JPG)](https://youtu.be/Z5URB4X2w9Q)  

Mapping external inputs to four different outputs on the screen  
[![button grid video](./image/lab3/FPGA_base.JPG)](https://youtu.be/TJx6PqLpqdA)  




### Acoustic

