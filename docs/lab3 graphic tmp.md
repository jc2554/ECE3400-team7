### Graphic
Generating the input signal
First, we wanted to create a 2 by 2 grid on the display. We declared a 2 by 2 array of bits representing the grid, and a wire that connect to the two input on-board button.

```Verilog
//2-by-2 array of bits
reg grid_array [1:0][1:0]; //[rows][columns]
reg grid_ind_x; //Index x into the array
reg grid_ind_y; //Index y into the array
// current highlighted square
wire highlighted_x;
wire highlighted_y;
```

Next, we use a simple state machine that will iterating through the 4 grid cell and set the button input to the corresponding grid cells.
![](./image/lab3/Button_Grid.JPG)
Here is the code for the state machine:

```Verilog
always @ (posedge CLOCK_25) begin
	// State 0: Loop through grid
	if (state == 0) begin
	  if (grid_ind_x < 1) begin         //If X-coordinate is 0
		 grid_ind_x <= grid_ind_x + 1;   //X-coordinate = 1
	  end
	  else begin                        //If X-coordinate is 1
		 grid_ind_x <= 0;                //X-coordinate = 0
		 if (grid_ind_y < 1) begin       //If Y-coordinate is 0
			grid_ind_y <= grid_ind_y + 1; //Y-coordinate = 1
		 end
		 else begin                      //else Y-coordinate = 0
			grid_ind_y <= 0;
		 end
	  end
	  state <= 1;                       //Switch to state 1
	end // state 0

	// State 1: If at highlighed coordinates, set grid space to 1, else set to 0
	else if (state == 1) begin
	  if (grid_ind_x == highlighted_x && grid_ind_y == highlighted_y) begin
		 grid_array[grid_ind_y][grid_ind_x] <= 1;
	  end
	  else begin
		 grid_array[grid_ind_y][grid_ind_x] <= 0;
	  end
	  state <= 0;
	end // state 1

	else begin                          //Default
	  state <= state;
	end
end
```

In order to confirm our logic work as expected, we connected four of the LEDs on the FPGA board, each one corresponds to a grid cell.

```Verilog
assign LED[0] = grid_array[0][0];
assign LED[1] = grid_array[0][1];
assign LED[2] = grid_array[1][0];
assign LED[3] = grid_array[1][1];
```
The following code is the set of combinational logic that determine the color of each pixel on the screen. The registers PIXEL_COORD_X and PIXEL_COORD_Y are the output from the VGA driver and the main module will output the color of that pixel.

```Verilog
always @ (*) begin 
  if(PIXEL_COORD_X > 0 && PIXEL_COORD_X <  grid_width && PIXEL_COORD_Y > 0 && PIXEL_COORD_Y < grid_width) begin
		if (grid_array[0][0]) PIXEL_COLOR = green;
		else PIXEL_COLOR = white;
  end
  else if(PIXEL_COORD_X > grid_width && PIXEL_COORD_X <  2*grid_width && PIXEL_COORD_Y > 0 && PIXEL_COORD_Y < grid_width) begin
  	if (grid_array[0][1]) PIXEL_COLOR = green;
		else PIXEL_COLOR = white;
  end
  else if(PIXEL_COORD_X > 0 && PIXEL_COORD_X <  grid_width && PIXEL_COORD_Y > grid_width && PIXEL_COORD_Y < 2*grid_width) begin
		if (grid_array[1][0]) PIXEL_COLOR = green;
		else PIXEL_COLOR = white;
  end
  else if(PIXEL_COORD_X > grid_width && PIXEL_COORD_X <  2*grid_width && PIXEL_COORD_Y > grid_width && PIXEL_COORD_Y < 2*grid_width) begin
  	if (grid_array[1][1]) PIXEL_COLOR = green;
		else PIXEL_COLOR = white;
  end
  else begin
		 PIXEL_COLOR = black;
  end
end
```

Here is a video of the grid display:
[![grid video](./image/lab3/grid.JPG)](https://youtu.be/Z5URB4X2w9Q)  


Next, we want to build a basic base station that have external input to the arduino board and the arduino with communicate with the FPGA using the digital ports on both of these devices. An important note is that the arduino output 5V and the FPGA operate at 3.3V, so we build a voltage divider to adjust the voltage from the arduino to the FPGA.  
We use two external push button as trigger of the message to be transfer from the arduino to the FPGA. Here is the code snippet for the arduino:  
```C
void loop() {
  int value1 = digitalRead(3);
  int value2 = digitalRead(4);
  digitalWrite(5, value1);    // output voltage of pin 3 to pin 5
  digitalWrite(6, value2);    // output voltage of pin 4 to pin 6
  delay(50);     
}
```

The code for the FPGA remains the same from the previous part, but onlt the input of the highlighted_x and highlighted_y is mapped to the 2 GPIO pins we connect to the arduino. 

```Verilog
assign highlighted_y= GPIO_0_D[29];
assign highlighted_x= GPIO_0_D[31];
```

Here is a video of the simple base state:
[![button grid video](./image/lab3/FPGA_base.JPG)](https://youtu.be/TJx6PqLpqdA)  

**The VGA cable connecting to the monitor only has one wire for red, one wire for green, and one wire for blue. These are analog cables (they take values from 0 to 1 V). We have provided a Digital-to-Analog-Converter (DAC) that converts the 8 given color bits (with a 3.3V digital output from the FPGA) to the desired three color 1V analog signals. Given that the VGA display has an internal resistance of 50 Ohms, you must list the resistor values in your lab report and explain how they were chosen.
Description of how the DAC on the provided VGA connectors works and how the resistor values were chosen.

#### VGA DAC
The VGA connector we use in the lab used a resistor DAC (Digital-to-Analog-Converter) to onverts the 8 given color bits (3 bit for Red, 3 bit for Green, and 2 bit for Blue) at 3.3V form the FPGA to the desired three color 1V analog signals.




