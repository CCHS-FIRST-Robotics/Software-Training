# Code Structure

## It's important to understand the code structure so you can easily decide what code needs to go where.

I like to think of it as the "flow" of the code. In a simple project, you might have one file that handles everything, maybe with a couple of other supporting imports. In a wpilib project, however, you'll have many files doing work each on their own. Subsystems will have multiple files all interacting with each other, and then the larger subsystem (along with all the others) will be used by the core code to make the robot do what it needs to do. Don't worry if that doesn't quite make sense yet. Let's dive in.

### Starting at the bottom

The first file that's run will be Main.java. You will never have to look at Main.java (poor file). When you press "deploy robot code", that file gives the signal for the next one to start: Robot.java. This handles what the robot does at every step. Last year, this file was *super* important. It would initialize the code on startup through the init() methods, and then every 20ms it would run code through the periodic() methods. For example, you want to always be driving, so in teleopPeriodic() - which runs every 20ms of the manually-controlled mode - you would write code that drives the robot based on the joystick inputs.

This year, however, we're using a different **command-based** system. There'll be more on this later, but essentially it moves those init() and periodic() methods to the subsystems. Robot.java is used to set up that system, as well as some of the logging (again, more on that later). This year, the "core" file is RobotContainer.Java. This file is only in command-based systems, and it's where you define all your subsystems like the drive-base or an arm. It's also where you configure bindings for the controller (what each button does).

### Moving up

What are "subsystems" exactly? Simply, they're a specific mechanism on the robot. Last year we had 5 subsystems: the drive-base, the arm, the grabber (claw), the limelight camera, and the ZED camera. Each one handled their own specific functions. This year it will be similar, but the subsystems will be slightly more isolated. Take a look at Arm.java from last year, I developed it in a way that's very similar to how subsystems will look this year.

Commands will be another fundamental section this year. We didn't use commands at all last year, so this will be completely new. Subsystems will be mostly isolated, so what do you do if there's something that involves multiple? That's where commands come in. They will interact with multiple subsystems, and typically be setup in RobotContainer.java. There'll be more on commands later.

### Helpers

Finally, there will be other files that are "helpers". Their sole purpose is to remove clutter in the code by putting **static** methods and variables in their own separate files. This is often for things that will be used in multiple subsystems or files. One of these will be the Constants.java file. It contains most of the general constants that are used throughout the code. This could be the loop time (20ms), constraints for the robot (maximum velocity/acceleration), or information about motors (port #, encoder clicks). 

There will also be "util" files. These will be specific to each year's mechanisms. Typically they help with certain math things or are different types of data structures (e.g., Java provides ArrayList, as a "util" class). Last year this included a class to represent the **state** (position and other important things that could change) of the arm, a class for the **kinematics** (finding the position/speed) of the arm, and a **motion profile** (creates a path for the arm to travel that limits its velocity/acceleration). You can look at R2Vector.java and ArmState.java for examples of **data structures** and Kinematics.java, LinearProfile.java, and QuadraticProfile.java for examples of math/physics utils.

