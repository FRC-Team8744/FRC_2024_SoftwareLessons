# Robot notes 03: Tasks for 1/30/2024 meeting

## Countdown to Scrimmage!
* Build week 4
    * Tues 1/30
        * Mechanical - intake parts to be done
    * Thurs 2/1
        * Mechanical - shooter parts to be done
        * Drive team selection starts - robot must be drivable in FieldOriented mode!
    * Sat 2/3
        * Mechanical - assembly of intake & shooter to be done
* Build week 5
    * Tues 2/6
    * Thurs 2/8
    * Sat 2/10, must be out by 5p
* Build week 6
    * Tues 2/13
    * Thurs 2/15
    * Sat 2/17, Scrimmage in Eagan

## Goals for Scrimmage
* Teleop driving needs to be rock-solid
    * Speed optimized for sprints and turning
        * Do we need to square the joystick inputs for better control?
    * Absolutely NO jittering on any joystick
    * Button assignments - check with drive team
        * Trigger intake sequence
            * Turn on intake motors at correct speed (match robot velocity?)
            * Stop when Note is loaded, or timeout
            * Some sort of feedback if timeout occurs
        * Trigger shooter sequence
            * Verify Note is loaded
            * Spin up flywheels (use SparkMax velocity PID)
            * When flywheels stable, feed in Note (motion profile so we don't tear up Note)
                * Measure Note exit velocity with beam break sensor.
            * Brake mode on all motors
        * Trigger auto-intake sequence
            * Turn to closest Note, estimate distance
            * Trigger intake sequence
            * Drive forward estimated distance
            * Return control to driver (remember to time out if something goes wrong)
        * Trigger shooter angle adjustment (second camera!)
            * Identify AprilTag on nearest target
            * Adjust shooter angle based on AprilTag ID and distance (math required!)
            * Feedback when done - green lightbar?
* Autonomous (listed in order of priority)
    * Top corner sequence (score pre-load Note, pick up nearby Note and score again)
        * Trigger shooter angle adjustment
        * Trigger shooter
        * Path to Note nearest the sidewall.
        * Trigger auto-intake sequence
        * Path back to Speaker
        * Trigger shooter angle adjustment
        * Trigger shooter
        * Path to reload location for human?
    * Top corner mid-field Note sequence (score pre-load Note, pick up far Note and score again)
        * Trigger shooter angle adjustment
        * Trigger shooter
        * Path to Note in the middle of the field (selected by strategy team)
        * Trigger auto-intake sequence
        * Path back to Speaker
        * Trigger shooter angle adjustment
        * Trigger shooter
        * Path to Note nearest the sidewall.
        * Trigger auto-intake sequence
        * Path back to Speaker
        * Trigger shooter angle adjustment
        * Trigger shooter
        * Path to reload location for human?
    * Make sure robot drive motors switch to braking mode if human presses Auto E-stop

## Trenton - Wire up dummy motors for mechanism tests
1. Find a plastic board you can mount to Swivels. Strap two intake motors, one flywheel motor, and one shooter angle motor to it.
2. Use USB-C cable to verify they are all set to the correct CAN address.
3. Wire them all into power (each gets it's own channel) and CAN (daisy-chain).
4. If done, continue trying to add tabs to Shuffleboard. [Found a possible example](https://www.chiefdelphi.com/t/shuffleboard-crashes-on-launch-for-no-apparent-reason/430714/6)

## Henrik - Mechanisms
1. Work on the intake sequence (a command). Two motors are set to different speeds (ask CAD if you don't know what it looks like).  Have the command time out after two seconds. [Timeout example](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html#adding-command-end-conditions)
2. Set the command to end early if the optical interruptor goes dark (a Note is loaded).
3. Start building the auto-intake sequence (a SequentialCommandGroup with parallel sections).  You will need to trigger multiple commands in parallel and serial.  At the start, the intake sequence and the "vision turn to Note" must run in parallel.  Once ready, a short drive sequence into the Note is needed. This ends on sensing a Note is loaded, or a time out if we miss. [Command sequencing](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html).

## Ben - Drivetrain
1. The NoNo robot will be under construction this week, but we still need to make the new gyro work!  Find NoNo.  Take the Pigeon2 off of it.
2. Mount the Pigeon2 on Swivels.  Connect it to power and CAN (Trenton can help).
3. Using the Phoenix Tuner X program, connect to the RobotRio with a USB cable and the diagnostic server.  Change the Pigeon2's CAN address to something that doesn't conflict (14?).
4. Use the code in the "Alex_IMU" branch to verify that we can talk to it.  Things will need to be commented out.
5. If you are stuck with extra time, check for swerve jitter on all of the XBox controllers and see if [input squaring](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/wpi-drive-classes.html#squaring-inputs) helps driving control.  (input squaring can be inserted before applying the joystick deadband in `DriveSubsystem.drive()`)
>By the way, it looks like we doubled up on deadband in your code.  Please remove the deadband from `RobotContainer` and add a constant for the actual deadband in `DriveSubsystem.drive()`

## Preston - Estimate shooter firing angle
1. First things first - we need a better mount for the camera.  Work with the build team to create a simple mount that we can bolt to the frame and adjust the camera angle so that we can see rings in front of the bumper.  This can be as simple as a metal plate with some holes drilled in it (it's just temporary).
2. You will need to mount [tags for the speaker](https://firstfrc.blob.core.windows.net/frc2024/FieldAssets/Apriltag_Images_and_User_Guide.pdf) at the correct height on the wall for testing.
3. Learn to estimate shooter aiming angle with [point of interest tracking](https://docs.limelightvision.io/docs/docs-limelight/pipeline-apriltag/apriltag-3d).

## Carson - Turn to target speed
1. Improve the turn to target speed.  It does not matter what target you track.  I think an AprilTag on a wall will be easiest for development.
2. [Practice switching pipelines](https://docs.limelightvision.io/docs/docs-limelight/apis/complete-networktables-api#camera-controls), so that we can switch between AprilTags, point of interest, and color tracking.
