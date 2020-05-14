## Write Up: 3D Motion Planning

<p align="center">
   
  <img width="805" height="593" src="https://user-images.githubusercontent.com/34810513/80971134-ed13cf80-8e39-11ea-8321-c2e53e64d603.jpg">
  
</p>
This Writeup.md considers the rubric points individually and describes how I addressed each rubric point in my implementation.  
You can find the youtube video of the demo here:

---
### Writeup / README

#### 1. To provide a Writeup that includes all the rubric points and how I addressed each one.  

The current Writeup.md (markdown) file serves the purpose of addressing how I have dealt with the various tasks.

### Explaining the Starter Code

#### 1. Explaining the functionality of what's provided in `motion_planning.py` and `planning_utils.py`

This is my undertanding of the starter code:

1) The Drone initially spawns at the center of the map.
2) This is the global home by deafult. 
3) The center of the map (-north_offset, -east_offset)
is set as the starting point.
4) The goal is 10 meters to the north and 10 meters to the east of the starting point.
5) A path is calculated using A* algorithm and the waypoint is calculated.
6) The drone follows the waypoints and moves in a zigzag manner to the goal.

<p align="center">
   
  <img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80979044-79c38b00-8e44-11ea-8756-0fd7ed3b9f58.jpg">
  
</p>

### Implementing Your Path Planning Algorithm

#### 1. Setting up the global home position

The task is to read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home.

I have implemented the solution in the lines from 123 to 127 in motion planning.py I used file reading functionality of python to extract the string and seperate the words based on commas and spaces. Once the string versions of the latitude and longitude were obtained, I converted them to floating point values. I used the set_home_position function to set the latitude and longitude obtained as the home position.
<p align="center">
   
  <img width="485" height="138" src="https://user-images.githubusercontent.com/34810513/80983326-12a8d500-8e4a-11ea-9035-5212f071c665.jpg">
  
</p>

#### 2. Setting the current local position
Here I have determined the local position relative to global home using the global_to_local function of the udacidrone API.

<p align="center">
   
  <img width="817" height="122" src="https://user-images.githubusercontent.com/34810513/80983688-77642f80-8e4a-11ea-911b-005b35aa4fe1.jpg">
  
</p>

#### 3. Set grid start position from local position
The local position obtained is with reference to the center of the global home position. We have to subtract this local value with the north and east offset values to obtain our position in the grid. This can be found in line 142.

<p align="center">
   
  <img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80989834-1b51d900-8e53-11ea-998c-7bcb69c098a5.jpg">
  
</p>

#### 4. Set grid goal position from geodetic coords
In order to set the global position, I have first specified it in terms of its geodetic coordinates and converted them to their local coordinates and applied the offset on them just like I did with the start coordinates. The lines 147-152 serve this purpose.

<p align="center">
   
  <img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80989996-52c08580-8e53-11ea-9025-6b3262277c8e.jpg">
  
</p>

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
I modified the A* algorithm to incorporate diagonal movments too. I did this by adding additional member values to the "Action" class in planning_utils.py. In addition to this I made some changes in the "valid_actions" function to make the drone move in the diagonal directions. These can be found in lines 58-61 and lines 91-98 in planning_utils.py.
<p align="center">
   
  <img width="400" height="300" src="https://user-images.githubusercontent.com/34810513/80990336-cd89a080-8e53-11ea-9fad-31af615f67b4.jpg">
  
</p>

#### 6. Cull waypoints 
Finally I used the concept collinearity to cull the path and extract the waypoints. I have created three new functions called as point(), collinearity_check() and prune_path() to do the same. point() converts the input values in the form of a tuple to a numpy array for easy calculation. collinearity_check() calculates the determinant of the points and if the area obtained so is lesser than a threshold epsilon, it return the boolean value True. Else it return False. prune_path removes the 2nd value from a group of 3 points if the points are collinear. If not, it considers the 2nd,3rd and the 4th point and leaves the first point behind. This uses a concept of sliding window.


### Execute the flight

After implementing the above methods, it was time to execute the flight. The A* algorithm surprisingly consumed more time than in the jupyter notebook excercises to find the path although the code was the same. 

<p align="center">
   
  <img width="800" height="600" src="https://user-images.githubusercontent.com/34810513/80990456-fad64e80-8e53-11ea-844a-4d85631180db.gif">
  
</p>
  
# Extra Challenges:

Sadly I haven't implemented any of the planning techniques such as probabilistic roadmap or receding horizon method. The drone's movement doesn't account for the dubin's car problem although I plan to try these things later in the future.


