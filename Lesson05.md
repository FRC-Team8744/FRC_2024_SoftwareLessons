# Lesson 5: Spin Thomas, spin! 11/07/2023
So I figured out a couple of things from last lesson, and... oops.  Anyone remember the PID control talk?  In there I mentioned that PID control theory assumes that the system under control is **LINEAR**.  Do you remember how the robot was throwing itself back & forth when trying to reach the setpoint?  That wasn't too much P gain - it was oscillation.

There is a problem with humans - we're not linear.  That volume knob on your music?  It has a logarithmic response curve:

We do the same thing with velocities - it's easier to drive a remote control vehicle if there is less joystick sensitivity at lower speeds.  So, WPILib embeds joystick input squaring in the `arcadeDrive` method on the drivebase - by default.

![]()

I had to dive deep into the library to find it, but if you don't want the "arcade" inputs to be squared then you have to create a drivebase method that adds an extra parameter.  This is called "overloading" in programming terms - the same function but with different numbers of parameters.  We have to add an overloaded method to the drivebase to allow shutting off input squaring:
```java
  public void arcadeDrive(double fwd, double rot, boolean sqr) {
    m_drive.arcadeDrive(fwd, rot, sqr);
  }
```

I also found another misguided assumption - you are able to edit the parameters of a PID controller on Shuffleboard, but the robot must be disabled.  If that sentence doesn't make sense, don't worry - we will work through an example.

## Turn & burn!
1. Open up GitHub Desktop.  Make sure `ThomasCanTurn2` is updated.
2. Switch to the `Command-with-generic-PID` branch in VSCode.
3. Look in `RobotContainer` - buttons A and B do what?
4. Load up the program and tune the PID for the turn.
> Note! The PID will not show up on Shuffleboard until you have triggered the turn at least once, because the command has not been loaded into memory yet.
5. Get it "close enough" and let's move on.

## Enough turning! Let's go back to a straight line!

Ok, we can turn to an angle.  Now what?  What do we need for autonomous mode?  **AUTONOMY!**

We're going to move precisely from one location to another in a straight line.  Monitoring the wheel encoders will keep track of how far we have travelled.

A nice side-effect of this setup is that we can start with the robot wheels off the ground - so put Thomas on the cart!

## Programming: Download code and review changes
1. Open GitHub Desktop and log in with your account.
2. Click on `File` -> `Clone repository...`
3. Select the `URL` tab and clone the following:
```
https://github.com/FRC-Team8744/ThomasCanGo
```
4. Now switch to VSCode.
5. With Thomas up on the cart, test out the `DriveDistance` command using the buttons on the joystick.
6. If it is working well, try it out on the ground.

## Profiled motion
So how does it work?  Does it do what you expect?

Something you may have noticed is a tendency for Thomas to try to topple over when you start the command, and a difficulty coming to a stop.  How can we fix this?  We will try to use a "profiled" motion.  This means that we will plan out the speed required at every point in time, so we can limit it.

## Trying profiled motion
1. Put Thomas back on the cart.
2. Go back to ThomasCanGo and uncomment the extra button definitions in `RobotContainer`.
3. Take a look inside `DriveDistProfiled`.  Where is the speed limited?
4. Give it a try and experiment with limiting both the speed and acceleration (one at a time).
5. Once you are happy with it.  Put Thomas on the floor and test.  What happens?

## Challenge!
* Make your own command to turn the intake motor when you press a button.

### Independent Learning
* [Profiled Motion](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/trapezoidal-profiles.html)
