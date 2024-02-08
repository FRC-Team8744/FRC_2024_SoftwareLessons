# Robot notes 05: Tasks for 2/08/2024 meeting

## Good News!
I was able to fix the gyro startup error and the problem with the wheels pointing the wrong direction!

## Bad News!
All your code branches are now BROKEN and will NOT load on any robot.  I had to upgrade the CANcoders on all the swerve modules to the Phoenix 6 library.  They won't talk to your code until you merge `main` into your branch.

## Countdown to Scrimmage!
* Build week 5
    * Thurs 2/8, Room Riverfront
    * Sat 2/10, Room Riverfront, must be out by 5p
* Build week 6
    * Tues 2/13, Room 322
    * Thurs 2/15, Room 322, pack for Eagan!
    * Sat 2/17, Scrimmage in Eagan

## High priority issues
* Move the carpet and the Speaker to Riverfront!
    * Add [tags for the speaker](https://firstfrc.blob.core.windows.net/frc2024/FieldAssets/Apriltag_Images_and_User_Guide.pdf)
    * Add tape marks on the carpet for [Speaker position and floor Notes (page 6)](https://firstfrc.blob.core.windows.net/frc2024/FieldAssets/2024LayoutMarkingDiagram.pdf).  BE PRECISE!
    * These must be returned to the makerspace after EVERY MEETING.
* Shooter manual control
    * Command for spin up & fire
    * Command for set shooter angle (an external encoder will measure the angle)
> Note: Once the shooter is installed, the drive team will definitely want the robot in a drivable condition at the end of every meeting!
* Vision "turn to colored ring" command
    * This needs to turn faster
    * We will require this for the auto routine, so it has to work as a command
    * Can we make this work while the robot is moving forward?
* Vision "turn to ApritTag" command
    * This needs to turn faster
    * We will require this for the auto routine, so it has to work as a command
    * Can we make this work while the robot is moving forward?
* Vision "set shooter angle at Speaker" command

## Lower priority tasks
* Driving speed scaling
    * Move the ANGULAR speed limit up to the top of `Constants` with the others.
* Continue trying to add tabs to Shuffleboard. We will need this to make a display that only shows what the driver needs to see. [Found a possible example](https://www.chiefdelphi.com/t/shuffleboard-crashes-on-launch-for-no-apparent-reason/430714/6)
* Joystick rumble works on the new XBox controllers! Jacob would like "ready to shoot" feedback at competition (aimed at speaker and angle is correct). After testing the joystick rumble (click on the Rumble bars in the driver station joystick debug window), [try making the joystick rumble from code](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#miscellaneous-examples).
* Calibrate the cameras so that we can use 3D measurements of AprilTags.
    * [Limelight calibration routine](https://docs.limelightvision.io/docs/docs-limelight/performing-charuco-camera-calibration)
    * [Photonvision calibration routine](https://docs.photonvision.org/en/latest/docs/calibration/calibration.html)
* Clean up "unused" warnings in `Robot.java` when logging is not enabled with: `@SuppressWarnings("unused")`
