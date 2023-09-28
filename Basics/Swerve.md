# What is Swerve Drive?

## Swerve drive provides a consistent and stable way for the robot to move in any direction

One of the most important aspects of performing well in competitions is your mobility. It's extremely important to be able to move quickly and efficiently throughout the field. Swerve drive is currently the best way of doing so. Briefly going over the other main options: tank drive has a set of wheels on each side of the robot controlled forward/backward, so the robot can turn in place, move forward or backward, or a combination of those. Mecanum drive has 4 wheels each with angled rollers, producing force vectors at an angle, which can be manipulated to move in any direction. This creates lots of wheel slippage, though, due to its design.

Swerve has the mobility of a mecanum drive, but the traction and stability of tank drive. It works by controlling both the position and speed of each wheel (4 total). So if you want to drive to the left, you simply turn all four wheels to point to the left, and then turn them foward. It's similar to how a shopping cart moves with swiveling wheels. By having complete control over the states (position and velocity) of all 4 wheels, you can create almost any force (thing that moves the robot side-to-side/forward-backward) and any torque (thing that rotates the robot) you desire.

### Resources (all optional)

- [WPILib docs](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/swerve-drive-kinematics.html).
  Gives examples of how some of it functions in code (can be confusing if you don't know what you're looking at)
- [Swerve Drive Kinematics (advanced)](https://www.chiefdelphi.com/t/paper-4-wheel-independent-drive-independent-steering-swerve/107383).
  Gives a derivation of some of the kinematics for swerve drive in a bunch of different scenarios. If you're comfortable with math/physics it might help you understand a bit about how swerve works
- [Swerve Drive Kinematics (very advanced)](https://www.chiefdelphi.com/t/whitepaper-swerve-drive-skew-and-second-order-kinematics/416964).
  Gives an explanation/derivation of second order kinematics and some of the issues with the first order model. It's interesting, but gets pretty technical.
  
### Notes

- There are a few different types of swerve that all achieve the same idea in a few different ways, and with varying mechanical designs. The design that we use is the Mk4i modules from Swerve Drive Specialties (SDS)
