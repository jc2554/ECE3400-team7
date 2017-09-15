# Milestone 1

## Goal
The goal of this milestone was to add line tracking and grid traversing functionality to our robot.

## Lab Procedure
### Line Detection
 We first tested the functionality of the IR line sensors by reading their output while being held a few millimeters over black and white surfaces. Using the measured values, we determined reasonable threshold values for the Arduino to decide if a sensor is over a line.

We then attached 3 QRE113 analog IR line sensors to the front of our robot, spaced so that, when aligned straight over a line, only the center sensor would read the line, as shown here:

![](./image/milestone1/1_2.jpg)

With these sensors, our robot can detect whether it is over and whether it is skewed on a line.

### Line Following
Our algorithm for following a line is fairly simple and works as follows:
```C
if (middle sensor on black and left sensor on white and right sensor on white){ //robot is going straight
  forward
} else if (middle sensor on black and left sensor on black and right sensor on white){ //robot is angled slightly to the right
  adjust slight left //slow left servo slightly
} else if (middle sensor on black and left sensor on white and right sensor on black){ //robot is angled slightly to the left
  adjust slight right //slow right servo slightly
} else if (middle sensor on white and left sensor on black and right sensor on white){ //robot is angled strongly to the right
  adjust strong left //slow left servo strongly
} else if (middle sensor on white and left sensor on white and right sensor on black){ //robot is angled strongly to the left
  adjust strong right //slow right servo strongly
} else { //either sensors are over intersection, robot is entirely off line, or (impossibly) left and right are on but middle is off
  forward //keep going, either way it's better than stopping
}
```
By using three line following sensors, we are able to allow our robot not only to detect if it is at an angle, but also to detect if it is at a strong or slight angle. This allows the line following to respond significantly more robustly than similar layouts containing only two sensors.

Putting this algorithm to the test, we found that the robot will successfully track a line even while being pushed slightly off course.

[![](./image/milestone1/1_1.jpg)](https://youtu.be/nTEPqP1qgJY)

### Intersection Detection, Turning, and Figure 8
We connected two additional QRE113 IR line sensors to the robot in order to detect when the robot has entered an intersection. The sensors are placed approximately an inch apart in line with the axis of the wheel, such that both sensors will read the line only when the wheels are over an intersection. Their placement can be seen here:

![](./image/milestone1/1_3.jpg)

Our initial turning algorithm works by simply setting both wheels to spin in opposite directions then looping until the front middle sensor detects that it has arrived over a line again. At this point, the robot should have completed a 90 degree rotation in whichever direction it was set to spin and, even if it should be slightly off center on the line, capable of tracking the line again from this point.

We then strung together a series of turning and following line until reaching an intersection commands necessary for the robot to perform a figure 8, which you can see here:

[![](./image/milestone1/1_4.jpg)](https://youtu.be/NfCRnDHCJfM)
