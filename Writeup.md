## Write Up: Building a Controller

<p align="center">
   
  <img width="805" height="593" src="https://user-images.githubusercontent.com/34810513/80971134-ed13cf80-8e39-11ea-8321-c2e53e64d603.jpg">
  
</p>
This Writeup.md considers the rubric points individually and describes how I addressed each rubric point in my implementation.  
You can find the youtube video of the demo here:

---
### Writeup / README

#### 1. To provide a Writeup that includes all the rubric points and how I addressed each one.  

The current Writeup.md (markdown) file serves the purpose of addressing how I have dealt with the various tasks.

### Implemented Controller

#### 1. Implemented body rate control in C++.

The body rate controller is implemented in the function BodyRateControl. It is a P controller in which a V3F variable called momentCmd is created to contain the moment values provided to the controller. This is calculated by finding the difference between target and the actual body rate values in each axis and multiplying them with the moments of Inertia in each axis to find the target torque. This calculated value is the error value. It is multiplied with the gain of the body rate controller kpPQR to find the target output. After some trial and error, I set the kpPQR values as 85,85 and 5 for P Q and R respectively.

#### 2. Implement roll pitch control in C++.

The roll pitch controller is implemented in the function RollPitchControl(). This is also a p controller where the value to be corrected is the elements R13 and R23 in the rotation matrix R. This is used to accurately control the roll angle and the pitch angle of the controller. The output of this controller is the target value(set point) for the bodyrate controller. Thus the coreection value is translated from the world frame to the body frame through a transformation. The gain value, kpBank for this controller is 12.

After implementing these controllers, the output of the second scenario is as follows.
<p align="center">
   
  <img width="817" height="122" src="https://user-images.githubusercontent.com/34810513/80983688-77642f80-8e4a-11ea-911b-005b35aa4fe1.jpg">
  
</p>

#### 3. Implement altitude controller in C++.
The altitude controller is implemented in the function AltitudeControl(). This is a complete PID controller. The velocity and the position errors are calculated to determine the P and D terms. The position error is integrated to find the I term. The target acceleration is added along with these terms to find the correction term called z_term. This value is then used to calculate the thrust to be applied while accounting for the action of gravity. The difference between the z_term and gravity is divided by R33 to account for the orientation of the vehicle. This is later constrained to stay within the maximum thrust of the drone. The gain values for the controller are 25(kpPosz), 6.5(kpVelz) and 20(KiPosZ).

#### 4. Implement lateral position control in C++.
This controller is a PD controller. It is implemented in the function LateralPositionControl(). This is used to control the drone's X and Y coordinates. The position and velocity errors are calculated. The velocity error is constrained to stay below the maximum XY velocity. The acceleration command is calculated by multiplying the error with the gain terms and it is constrained. The kpPosXY and the kpVelXY gain terms are 30 and 11 respectively.

#### 5. Implement yaw control in C++.
The yaw control calculates the target moment along the Z axis. It is implemented in YawControl() function. The yaw error value is constrained between -2pi and +2pi and it is multiplied with the gain value kpYaw. The kpYaw value is 2.
   
After implementing the altitude, lateral position and the yaw controller, this was the result in scenario 3.

<p align="center">
   
<img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80990336-cd89a080-8e53-11ea-9fad-31af615f67b4.jpg">
  
</p>

#### 6. Implement calculating the motor commands given commanded thrust and moments in C++.
The thrust and the moment values calculated by the different P, PD and PID controllers are converted to the motor thrust values in the GenerateMotorCommands() function. The collective thrust and the commanded moment values are used to calculate the c bar, p bar q bar and the r bar values. These 4 values are then used to determine individual thrust values by solving the linear equation using the matrix method.

<p align="center">
   
<img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80990336-cd89a080-8e53-11ea-9fad-31af615f67b4.jpg">
  
</p>

### Flight Evaluation

After implementing the above methods, it was time to execute the flight. The A* algorithm surprisingly consumed more time than in the jupyter notebook excercises to find the path although the code was the same. 

<p align="center">
   
  <img width="800" height="600" src="https://user-images.githubusercontent.com/34810513/80990456-fad64e80-8e53-11ea-844a-4d85631180db.gif">
  
</p>
  
# Extra Challenges:

Sadly I haven't implemented any of the planning techniques such as probabilistic roadmap or receding horizon method. The drone's movement doesn't account for the dubin's car problem although I plan to try these things later in the future.


