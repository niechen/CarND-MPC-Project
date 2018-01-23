# MPC project


## Model

In this project, we use an optimization approach to control the vehicle. Inputs are the coordinates of the road ahead of the vehicle's current position, and out put is actuator controls including steering angle and throttle.

We first transform the input coordinates from map coordinates to coordinates in vehicle's frame. Then we fit the points to a third-degree polynomial. The coefficients for the polynomial is pass to the MPC model to represent the road path ahead.

The MPC model then constructs the cost function and constraints of the optimization model. The actuators (steering angle, and throttle) are the independent variables. We use the kinetic model to predict vehicle's position giving the actuator values. The cost function that we try to minimize consists of the squared distance between predicted position and center of the road path and squared difference of the predicted direction and tangent of the road path. There is also an additional L2 regularization term in the cost function to discourage large sudden actuator values.

The model looks ahead 8 time steps, with each step being 0.1 second. These values are chosen by trial and error. Other values are tested, but these are the values that best balances performance, resolution, and a reasonable look ahead time for moving car.

The model is also taking a 100ms latency into consideration, by adjusting the state of the vehicle to it's predicted value in 100ms using the same kinetic model we used in the model.

