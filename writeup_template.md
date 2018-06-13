## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
`Answer:`
These script motion_panning.py that was provided initially was a basic planner extending 
backyard_flyer_solution.py.
The main difference is that backyard_flyer_solution.py uses hardcoded fixed waypoints,
where as motion_panning.py uses waypoints that are generated realtime from the map after 
using create_grid, a_start functions from planning_utils.py.
The basic implementation starts from the center of the map. The goal location is 10 mtrs 
to the north and 10 mtrs to the east. So the goal is to reach from the bottom left of a 10x10 square 
to the top right of the square. Since no diagonal movements are allowed because of our a_star
basic implementation, the drone reaches the top right in a zig zag manner on the grid.
 

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
`Answer:`
`code lines : 122-130 in motion_planning.py`
I read the first line from the csv file using python csv reader and separated
latitude and longitude. Then I used set_home_position function to initialize home
position.

#### 2. Set your current local position
`Answer:`
`code lines : 131-134 in motion_planning.py`
I extracted current global position using self._longitude, self._latitude and self._altitude.
Then used global_to_local function to get current local position in NED frame.
#### 3. Set grid start position from local position
`Answer:`
`code lines : 146-147 in motion_planning.py`
Grid start position is set to current local position of the drone.
#### 4. Set grid goal position from geodetic coords
`Answer:`
`code lines : 156-159 in motion_planning.py`
Any latitude and longitude is taken as a goal position and it is converted to 
local NED coordinate frame using global_to_local function. Then grid goal position is set.


#### 5. Modify A* to include diagonal motion (or replace A* altogether)
`Answer:`
`code lines : 90-93 and 123-130 in planning_utils.py`
In the actions class I added(90-93 lines) diagonal motions (NE, NW,SE,SW) with a cost of sqrt(2).
Then I added(123-130 lines) corresponding checks in valid_actions to remove any
invalid diagonal actions.


#### 6. Cull waypoints 
`Answer:`
`code lines : 6-35 in planning_utils.py`
I added functions to prune line segments using the collinearity condition.
The code is taken from previous exercises shown in the tutorials.

The basic algorithm is that if three points are collinear their determinant is zero
or some low value. We use this logic to eliminate the middle point in every set of three
points that are collinear.

### Execute the flight
#### 1. Does it work?
It works!
[Here is the youtube video link](https://youtu.be/5bH68pU24P4)

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  Yes, all the specifications have been met.


