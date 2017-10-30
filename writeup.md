# Path Planning

> The goal in this project is to build a path planner that creates smooth, safe trajectories for the car to follow. The highway track has other vehicles, all going different speeds, but approximately obeying the 50 MPH speed limit.
>
> The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.

https://media.giphy.com/media/3o6nUYvmyTR3Opn6RW/giphy.gif

## Model

The simulator provides the following sensor data which we can build our models off of:

- Sparse waypoints of the track
- Locations of other cars via sensor fusion
- The location of the car in global and frenet coordinates
- Heading and speed of the car

### Coordinate system

The localization data for the car is in the standard global coordinate format but also augmented with frenet coordinates. The frenet coordinates greatly simplify the calculations by allowing us to remove the complexity of dealing with the curves of the road. It allows us to focus on the position of the car on the track relative to the center and distance traveled.

The controller expects the next waypoints to be provided in global coordinates so we still have to do the transformation but only for controlling the vehicle.

### Lanes

The simulator contains a 6 lane highway with 3 lanes going the same direction as the car and 3 lanes going the opposite. By abstracting the concept of a lane into a 3 element enumeration, a state machine becomes a simple way of describing the lane changing logic in the code.

### Prediction

Each car that is described by the sensor fusion must have their trajectories predicted in order to have safe lane changes. In my implementation I check in front of the car to verify that the car will not collide with another car in the same lane. I also do the same operation when deciding if the car should change lanes in an attempt to overpass a slower car in front.

### Jerk Avoidance

The provided waypoints that describe the track are sparse and will trigger the jerk alert if followed directly. To avoid too much jerk for the occupants, a spline interpolation is used to create a set of smooth waypoints for the car to follow.
