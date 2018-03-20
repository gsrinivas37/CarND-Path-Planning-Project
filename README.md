# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

### Model for generating paths

The path is generated using the map of the track. A lane is chosen and then 3 points are extracted from this lane at a distance of 40, 80 and 120 meters from the current car position.
These points are used to generate a spline, that interpolates the points between these future points. The distance between the interpolated points is such that when the car is running though them at 0.02 seconds/waypoint the resulting speed is the target speed.
The spline helps minimize acceleration and jerk, to pass the criteria of the project

On top of this basic driving capability, the car is also looking for other vehicles to avoid collisions. Once a car is found further in the lane, this becomes the lead car. The ego-vehicle speed is matched to the lead vehicle speed and a minimum safety distance is maintained.

Usually, the lead vehicle will drive significantly slower than the speed limit so it will make sense to change lanes to pass it.
For this, the vehicle is constantly monitoring lanes and assigning a lane speed, based on the speed of the vehicles close to our position in each line. Also, a variable lane_available is constantly updated, so that the vehicle knows whether or not it is safe to perform a lane change.
If it is safe to perform a lane change, and the adjacent lane is moving faster than the current lane, the vehicle switches lanes.
