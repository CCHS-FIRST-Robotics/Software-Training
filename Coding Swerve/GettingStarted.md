These are the steps for getting started writing swerve code. You *can* reference my code (2024DriveBase), but I'd try to mostly write it yourself (using WPILib and vendor libraries, ofc).


## Part 1 - Setup (~20-40mins?)

**1.** Clone the github repository "2024 Training" (you might want to download github desktop if you're having trouble with git), and create a new branch under your name. Push this new branch to github, and check to make sure you've actually created a new branch and are on it. In github desktop that will look like this: 
image.png

**2.** Delete everything in the folder it creates, EXCEPT .github, .git, and .gitignore

**3.** Create a new CommandBased project: wpilib in vscode (download guide) -> create a new project -> project type -> template -> java -> Command Robot. This will make a command-based project, with some example code to kinda show how things work. WPILib has a whole section in their docs (everything through "The Command Scheduler" -- don't go past that) that explains everything with command-based, so definitely read that (you might want to read some of it or skim before you read the rest of the email since I'm gonna be referencing command-based things.

**4.** Create the structure of the project -- this I'm going to kinda just give to you. This won't be your final structure, but it's how I'd recommend you do it for now. In the subsystems folder, have a subfolder called "drive". This will have two files: **Drive.java** and **Module.java**. 

**Drive.java** will be your actual subsystem (extends SubsystemBase); this will control more "macro" things about the robot: using the gyro, controlling each module, and housing logic for how the drive base will move as a whole. 

**Module.java** will be like a mini-subsystem for each module. Since they're all pretty much identical, it's a good idea to just have one Module class, and just create 4 Module objects in Drive.java. I would recommend creating a periodic() method in Module.java so it acts like a subsystem (note that since it's NOT a subsystem, the periodic() method will NOT be called automatically, you should call it in the Drive.java periodic() method).


## Part 2 - Background Info (~70-90mins)

**5.** Read up about swerve and some math/physics basics. I don't know how much physics and calculus you've done, but you might want to check out 
- [video on vectors]
- [video on basic/conceptual calculus] (+ the ep 2 video after it if you feel like it)
- [video on linear kinematics/using vectors in practice]
- [video on rotational kinematics]
I watched through all those videos to make sure they're relevant, and 90% of it I use somewhere in the current swerve code. If you haven't done trig, check out Wikipedia/khanacademy. That's also pretty important, and we work with sin/cos a lot, especially with vectors. 

**6.** Check out the geometry section on wpilib (short), the Swerve Drive Kinematics page on wpilib (short), and this "paper" (short but detailed), which goes into the derivation of the first order (one derivative) inverse kinematics (from chassis kinematics to module kinematics, as opposed to forward kinematics which is module -> chassis) for swerve drive.

**7.** Check out the advanced controls introduction on wpilib; this talks about the problems with controlling motors and how you solve them. Very important, so make sure you understand this section. If you want to learn a bit more, check out this book (written by wpiman himself), it's a very very good resource to go a little more in-depth.


## Part 3 - Coding the Modules

**8.** It's time to start writing Module.java! This will control each module and process its sensor inputs. For that, you'll need to know a bit about the hardware we're using. The wheel on each swerve module is controlled by two motors, one that controls the wheel's angle (the azimuth/turn motor), and one that controls the wheel's speed (the drive motor). 

We're using **NEO motors** by REV Robotics, each controlled by a REV **SparkMAX motor controller**. 
- Each motor has a **built-in encoder** (something that reports the position of the motor shaft); these encoders are relative, meaning that every time you turn on the robot, the measured position resets. Each motor also has built-in sensors to measure things like voltage, current, temperature, etc; these, along with the relative encoders, are read through the SparkMAX.
- In addition, we have one absolute encoder per module. These are Redux **canandcoders**, which use a magnet to read the absolute position of the output shaft (note that this is different from the motor shaft, since the motor shaft goes through a few gears, meaning the actual output of how fast it turns the wheel is much lower -- i.e., ~6.7 rotations of the motor shaft will result in one rotation of the wheel). This absolute position is only used for the azimuth/turn motor, which needs an absolute position so that we don't have to reset the wheels to 0 degrees every time we turn on the robot: it will just know where they are. These absolute encoders are connected via a cable (not built into the motor) to the SparkMAX directly, so again, this can be read through the SparkMAX.

Now, for using these in code, you'll need to refer to the manufacturer's library. The REV docs can be found here, and the Redux docs here. Note that both of these have general documentation/information on the product and how to use it (what I linked), but they also have a Java API documentation (or just Javadocs/Java documentation); this is super important, as it has the documentation for what each class in the library is, what it does, what its methods are, and what those methods do. 

So, for example, say you want to read the temperature of a motor in code: you'll look at the REV Java API docs and find the SparkMAX motor controller class, then look for the method to read the temperature. In code, you'll create a SparkMAX object, giving the proper constructor arguments as specified in the docs, and then when needed, call the getTemperature() method, or whatever it may be. If you're wondering about something more broad/conceptual, like what the different SparkMAX control modes mean, you'll want to look at the general REV documentation, which has a detailed explanation. Check out this page on wpilib for info on how to install these libraries.

**9.** Now that you've got your hardware setup and found the methods to send outputs and read data to/from the motor, it's time to start writing some of the logic to make it work. There are two main parameters you're trying to control (described in the WPILib SwerveModuleState class): the wheel velocity (how fast it spins) and the wheel angle (the direction it's facing). 
- For this, you'll need to use a PID loop. You can use the WPILib PID Controller class to implement this, but I would recommend using the onboard PIDF built into the SparkMAX (documentation should explain how to use it, and has code examples) -- the main difference is that the WPILib class will run on the RoboRIO at a frequency of 50hz (i.e., it updates the motors every 20ms), whereas the onboard SparkMAX control will run at a frequency of 1kHz (updating the motors every 1ms: much more accurate).
- You don't actually want to use a full PIDF controller, as it can actually diminish performance in certain cases. For controlling the angle of the module (with the azimuth/turn motor), you could use full PID, but practically a P controller is going to do just as well; the motors are extremely powerful in relation to the friction of the floor, and it will have no trouble zooming to the setpoint and immediately stopping with no overshoot.
- For velocity control of the module (with the drive motor), you should use a feedforward (and tune that first) -- it acts essentially like a flywheel [relevant wpilib page] -- and then add a little bit of P. Do not use D gain, as it will hurt your performance (I won't get into why for now, if you're interested, you can check out the book I mentioned on controls earlier, section 6.10.7). You could use I, but you likely won't need to with a properly tuned feedfoward. Note that the SparkMAX "Feedforward" term is just "kV", as described in the WPILib feedforward section. 

**10.** Once you have the code for the modules all written, it's time to write Drive.java and put them all together! In the periodic() loop, write some code to run all four modules at some setpoint (it can be arbitrary for now). This is how you'll tune your PID constants. Now, fire up the robot and let it run, see how the robot behaves, and adjust accordingly. Since all four modules will be moving at the exact same setpoint, I would just focus on tuning one. The same constants should work fine for all four modules. I recommend (especially for velocity tuning), printing out the measured values so that you know whether you need to increase or decrease your gains, as it can be hard to determine visually.

**11.** Once your modules are tuned, try to create a command to get driving with joysticks. In the commands folder, create a new command called "DriveWithJoysticks.java". This will be your default command for the drive subsystem. It should take suppliers from each controller joystick and use those to generate some setpoint. For now, don't worry about using Swerve Kinematics or anything; just have the command process the joystick inputs to produce some setpoint that each module will follow. The way I did it was: X direction controls rotation angle (all the way to the right -> 90 degrees, all the way to the left -> -90 degrees), and Y direction controls speed (all the way up -> full speed - really you should do like 20% to start --, all the way down -> full speed backward).

That should be all for now! After that, it gets a little more complicated, so let's see if you can do that. It might sound like a lot, but it's not too much code, I promise.
