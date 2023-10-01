# What are "Controls"?

## Controls (and Control Theory) look at how you actually get the robot to do what you want.

It's a pretty broad and complicated field, so let's get a little more specific. Say we're an engineer trying to create a function to keep an oven at a specific temperature. This is one of the most simple control examples. The simple solution has the following logic:
  if the oven is below the desired temperature:
    turn the heaters on
  otherwise:
    turn the heaters off.

It's simple but effective. Now let's look at something more common to FRC: controlling a motor. Say we apply this principle to controlling the position of a motor (for example that could mean moving it exactly half a rotation from where it started). If it hasn't yet reached the position we want, turn the motor on, if it has, stop. The issue with this in the case of a motor, is that we need precision. It doesn't quite matter if an oven is at 428 degrees instead of 425, but for a robot, that could mean our arm drops the cone and it completely misses the post. With motors, they don't stop instantaneously. When we reach the position and turn the motor off, it has momentum and will keep moving at the same speed until forces like friction slow it to a stop. This means we'll have gone way past the position we wanted by the time the motor actually stops. 

There are a couple ways you might think off the top of your head to solve this: 
  1. What if we just tell it to **move in the opposite direction** once we reach the position?
      This would help to mitigate the issue, but how will the motor stop? You have the same issue coming back the other way.
  2. What if we **turn the motor off before** we reach the position?
       This is also a good idea, but when do you turn it off? It can take a while for it to slow to a stop due to friction, so is that the most efficient method? 

This is where **Control Theory** comes in. Using the tools of control theory, we can determine how to control the motor to do what we want.

While even the problem of controlling a motor has complexities, there are more advanced control systems, that often have multiple moving parts. To give an example, let's say instead of just controlling a single position, we want to move the **entire robot** to a position. 

I didn't mention it before, but in order to know a motor's position, we use something called an encoder (basically a sensor that tracks the rotation of a shaft). We don't have one sensor that just plainly tells us the position of the robot, but it's absolutely critical because we can't know if we've reached a desired position if we don't know what our position is. On the robot, we have a bunch of different sensors, all giving us different and noisy (slightly incorrect) values. This poses another problem that control theory attempts to solve: **state estimation**, or accurately determining the **state** (position, velocity, or other attributes we care about) of our robot. 

We also need to determine what the best path is for the robot to take. Do we want to move in a straight line? A curve? Do we want to go faster at certain points on our path and slower on others? Do we care about how fast the robot moves, and how fast it accelerates? **Path planning** is a complex problem that we also use control theory to solve.

And once again, we need to know how to actually move the robot to the position and follow the path we've set. This is the same problem as controlling a motor to a position, and while we use the same tools, is going to be slightly different in execution.


All of these are extremely important and fundamental to getting the robot to do what we want to do. Robots are complex systems that have a lot of moving parts, so it's important to have a robust control system
that moves accurately and efficiently.


### Exercises

- Answer the question: **"Develop a method to control a motor's position and describe why you think it will work, and why it may not."** on [this form](https://docs.google.com/forms/d/e/1FAIpQLSdc9Ht6sldIKpqTfQdf9LvpWEeQi3xPuqtvBxN8g83msOSnIw/viewform) 
