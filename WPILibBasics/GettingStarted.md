# Intro to FRC Programming

## Programming in FRC is very similar to programming in other contexts, but it's important to be familiar with the tools at our disposal and our architecture and practices

Our code runs on the roboRIO, a small linux computer that lives on the robot.
Through the magic of wiring, we connect it to everything else on the robot that needs instructions, which includes hardware parts like motors, sensors, and cameras.
The robot connects to either a laptop running the driver station or the field at competitions over Wi-Fi to send and recieve data such as button presses, camera feeds, and enable signals.

However, it becomes very clumsy and difficult to interface with these parts directly.
WPILib is an open source library built for FRC which abstracts away the low level code to allow us to focus on the high-level control logic.
The project has a large number of features and is the main building block that our code is built on top of.
It also has extensive documentation both for it's own API and for the concepts and math behind many of its features.
Venders (companies that sell us parts) also often have libraries to interface with their parts.
These include the Phoenix library made by CTRE, the manufacturer of the majority of our motor controllers.

We also use other libraries for other tasks that WPILib doesn't support. One of these is [Photonvision](Vision.md), a library that processes our camera feeds to turn them into useful target information.
Another is Pathplanner, a library to create and follow paths for the Autonomous portion of a match.
Many of these libraries will have their own articles here as well as their own documentation.

## Resources

### Must read/watch

- [Installation guide](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html) (NOTE: you cannot deploy robot code on a macbook, if you want to test your code on the robot (not in sim) you MUST use one of the robotics laptops).
- Read through [WPILib's zero to robot programming guide](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-4/creating-test-drivetrain-program-cpp-java.html)
  - We will be writing a similar program in the next few weeks.
  - We mostly use CTRE components, so select that tab for the example code.

### Optional (but recommended to get more familiar with it before you start coding)

- [WPILib docs](https://docs.wpilib.org/en/stable/index.html)
- [WPILib full zero to robot guide](https://docs.wpilib.org/en/stable/docs/zero-to-robot/introduction.html)
- [WPILib repo](https://github.com/wpilibsuite/allwpilib)
- [Glossary of jargon](https://docs.wpilib.org/en/stable/docs/software/frc-glossary.html)

### Exercises

- Download and install WPILib
- Create a new branch under "2024 Training" with your name

### Notes

- A lot of what WPILib does is "under the hood" to let us focus on high-level code rather than low-level hardware interface.
  This type of code is called the Hardware Abstraction Layer, or HAL

### Credit
Stolen from 8033 lol
