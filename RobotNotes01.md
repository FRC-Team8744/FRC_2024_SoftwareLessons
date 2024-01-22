# Robot notes 01: Tasks for 1/23/2024 meeting

First off - fantastic work on Saturday!  We got a lot done.  What are the next goals?

Let's refresh our memory on the task at hand: [2024 FRC Crescendo Kickoff Video](https://www.youtube.com/watch?v=9keeDyFxzY4) 

What have we done already?
* Drivetrain Teleoperation (manual driving)
* Path following for Auto
* Limit switch detection
* Lightbar feedback

What skills do we need to master going forward?
* Adding Command triggers to paths
* Vision turn to target
* Halt intake motor when Note is sensed
* Sequence commands for shooter
* What else?

## FOR EVERYONE
* If you code does not build, do not upload it to GitHub!!!

## Trenton - Merge Ben's drivetrain work into Main
1. Do not add anything that is still experimental.  At the time of writing, I see your DigitalInput code in Ben's DriveSubsystem.  This is not where it will eventually end up, so don't add that to Main right now.  Ask questions.
2. Think about what the driver will want to see on the Shuffleboard screen at competition.  How do we get there?
3. Figure out how to send Shuffleboard data to a different tab without crashing the program.  This is kind of an issue...

## Ben - Drivetrain
1. The drive motors are reasonably tuned for now, but we haven't checked the turning motors.  Send "commanded angle" and "measured angle" data for the FrontLeft wheel to Shuffleboard like we did for the drive wheels.
2. We will need to collect data with the "drive in a square" path and check how well the wheels turn to target.
3. Gyro debug - send the absolute angle of the gyro to shuffleboard so we can start analyzing the "drift" issue.
4. Create a "DebugLevel" constant and use it to disable telemetry logging (we might want to turn it off at competition).
5. We need to write up a tuning procedure (comments in DriveSubsystem) for the drive motors.
6. Constants clean up - if constants are commented out and it doesn't seem like something we will use in the future, delete them.
7. We should only have two constants for MaxSpeed (one for teleop, one for auto), located at the top of the file.  (do the same for acceleration) I like the idea of adding the `MAX_SPEED_IN_PERCENT` option that is in the Swerve constants.

## Preston & Carson - LEDs and Vision
1. Set up your `auto_led` command so that it will track an AprilTag with the lightbar.  Do the following:
    1. Add the Vision and LEDS subsystems to the command.  Here is an [example.](https://github.com/FRC-Team8744/ThomasCanTurn2/blob/Command-with-generic-PID/src/main/java/frc/robot/commands/TurnSimplePID.java)
        * Both subsystems must have a private variable just after the class declaration. (Like `private final DriveSubsystem m_drive;` in the example)
        * Pass in parameters for both subsystems. (Like `public TurnSimplePID(double targetAngleDegrees, DriveSubsystem drive)`)
        * Copy the parameters to the internal variables. (Like `m_drive = drive;`)
        * Tell the scheduler which subsystems are required. (Like `addRequirements(m_drive);`)
    2. In `initialize()`, set only the middle LED to yellow.
    3. In `end()`, turn all the LEDs off.
    4. In `execute()`, create an `if` statement.  If a target is in view, set the middle LED to green.  If not, set the middle LED to red.
        * [if statement syntax](https://www.w3schools.com/java/java_conditions.asp)
        * The Limelight value for 'target in view' is `tv`. [Read it in with this syntax](https://docs.limelightvision.io/docs/docs-limelight/getting-started/programming)
        * Set the LED color with the `LEDS.java` method that you created.
    5. TEST
2. AFTER the first part works, display the target angle offset on the lightbar.
    * The Limelight value for 'target horizontal offset' is `tx`.
    * The offset will have to be scaled to the size of the lightbar.  According to the [API](https://docs.limelightvision.io/docs/docs-limelight/apis/complete-networktables-api), the value for `tx` will range over -29.8 to 29.8.  How do you convert this value to a position on the lightbar?
        * Hint 1: Multiply by a ratio to fit the range of `tx` to the range of the lightbar
        * Hint 2: [Round to the nearest integer](https://www.w3schools.com/java/java_ref_math.asp)
    * IF you have a valid target, set the position of the red dot to the offset (light in the center is zero).

* [Tips for tuning the vision pipeline](https://docs.limelightvision.io/docs/docs-limelight/getting-started/pipelines)
* [Tuning for a color](https://docs.limelightvision.io/docs/docs-limelight/pipeline-retro/color-filtering)

## Henrik - Mechanisms
1. Add a public method to your Intake subsystem that will check the status of a DigitalInput. (Trenton has working code)
2. Create a command that will start the Intake motors during `initialize` and will stop them in `end` once the DigitalInput is triggered (or the command times out).  The references I gave Preston above may be helpful in remembering how to pass in a subsystem to the command.
3. Figure out how to get the RoboRio to ignore the missing motor driver.  Start by reading back the `RobotName` preference and turning off code if it matches `Swivels`.
4. Wire in a dummy motor and test it?
5. Start studying [Command sequencing](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html).  You will need to spin up flywheels, set the shooter angle based on distance supplied by vision, feed in the Note for shooting, and shut things down based on DigitalInput triggers and flags for 'Flywheel at correct velocity'.

