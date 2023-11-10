# Lesson 6: Profiled motion  11/09/2023

Today we will be exploring profiled motion, kinematics, robot poses, PID on the SparkMax and writing your own subsystem!

## First, make your code less confusing
I noticed when we were getting Git installed on everyone's laptops that most of you still have an annoying VSCode "helper" function turned on - Inlay Hints.  These explain method parameters when you are editing a line, but they act like extra code that doesn't exist.  Here's how you turn them off:

1. Click on the gear symbol on the lower left hand side of VSCode.
2. In `Settings`, search for `Inlay` and turn off `Inlay Hints`. This eliminates text added to you code while you are editing a line.

## Where we are headed
What is our goal learing things like kinematics and robot poses?  Anyone remember the swerve drive robot we built?  It just barely functions right now, because Jacob doesn't understand the program - but you will.  In order to do even simple driving with swerve, we need to have a better model of how to drive a robot.

## What are [Kinematics](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/intro-and-chassis-speeds.html)?
We will be working on "Forward" Kinematics - if we move a robot wheel, where does the robot end up on the playing field?  Kinematics can also involve long chains of mechanisms (such as a robot arm), but today we will be focused only on the motion of the robot base - also known as odometry.

> For the curious:
> * [WPILib: Intro to kinematics](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/intro-and-chassis-speeds.html)
> * [Robot Kinematics in a Nutshell](https://robocademy.com/2020/04/21/robot-kinematics-in-a-nutshell/)
> * [Robot arm kinematics](https://www.cs.cmu.edu/~16311/current/schedule/ppp/Lec17-FK.pdf)
> * [College-level Kinematics](https://www.cs.cmu.edu/afs/cs.cmu.edu/academic/class/16311/www/current/lecture/Kinematics_final.pdf)

Odometry is the use of data from motion sensors to estimate change in position over time.  We use the wheel encoders to measure how far they travel and the gyro to keep track of the robot orientation to the field.  Summing up the small changes in these measurements will keep track of the robot's position.

![Odometry math](https://www.researchgate.net/profile/Kooktae-Lee/publication/225502915/figure/fig9/AS:668281539137550@1536342143576/The-kinematic-model-of-a-two-wheel-differential-mobile-robot.png)

Once we can track the robot's position on the field, then we can keep track of the robot's pose.  Who knows what a pose is?

The last thing I need to talk about is using a PID on the SparkMax motor controller.  Up to now, we have been using PID controllers defined in software.  These work reasonably well, but our software only runs the `execute` portion of a command every 20ms (50Hz).  For a slow robot this is fine, but we can do better.  The control loop onboard a SparkMax runs at 1kHz (1ms) - that's 20X faster!  How can it do this?  It avoids the communication over the CAN bus between the SparkMax and the Roborio.  Therefore, it can respond to a disturbance faster.  The code we will use now has pushed the PID loop onto the SparkMax - the velocity of the wheels as they turn is now controlled.  This can make loop tuning a little more interesting, but I will show you how to do it.

## Experimenting with profiled motion
1. Open up `ThomasCanGo`.
2. Sync with GitHub.
3. Open the `Velocity_Profile` branch.
4. Test it out.  Try different accelerations and max speeds (don't get carried away!)

## Programming Challenge
The profiled motion program is not that new for us, so now it's time to work on a real challenge.  You have a program that can move the robot base, but not the intake motor.  Create a new subsystem called `Intake`.  Create a method to turn the motor.  Bind it to a button.  Make it work.

> Hints:
>
> [SparkMax Code Examples](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-code-examples)

## SparkMax PID tuning demo
If we have time, I will demonstrate how to tune the PID on the SparkMax using REV's Hardware Client.

[REV Hardware Client](https://docs.revrobotics.com/rev-hardware-client/)
