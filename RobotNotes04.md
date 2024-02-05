# Robot notes 04: Tasks for 2/06/2024 meeting

## We're all in this together!
I'm going to try switching to a "Things that need doing" list instead of individual assignments.  If you see something that needs attention and you have the time to work the problem - go for it!

## UPDATES!
* Update your [Game Tools](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html) to fix the "internal issue with print error and tags" problem
* Click open the "vendordeps" list in VSCode file explorer.  Open the "Phoenix6.json" and "Phoenix5.json" files.  If either of them say "beta" in the "version" at the top of the file, you MUST uninstall them (Manage Vendor Libraries in right-click menu for `build.gradle`) and them re-install the [online library](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#vendor-libraries). This fixes the Pigeon communication issue.
* Use the "online check for updates" in `build.gradle` vendor libraries - PathPlanner updated again!
* Merge `origin/main` branch into your branch.  Some of us are diverging too much and that will make re-integration more difficult later.  You've been syncing your code with GitHub, right?

## Countdown to Scrimmage!
* Build week 5
    * Tues 2/6, Room 322
    * Thurs 2/8, Room Riverfront
    * Sat 2/10, Room Riverfront, must be out by 5p
* Build week 6
    * Tues 2/13, Room 322
    * Thurs 2/15, Room 322, pack for Eagan!
    * Sat 2/17, Scrimmage in Eagan

## High priority issues
* Move the carpet and the Speaker up to 322!
    * Add [tags for the speaker](https://firstfrc.blob.core.windows.net/frc2024/FieldAssets/Apriltag_Images_and_User_Guide.pdf)
    * Add tape marks on the carpet for [Speaker position and floor Notes (page 6)](https://firstfrc.blob.core.windows.net/frc2024/FieldAssets/2024LayoutMarkingDiagram.pdf).  BE PRECISE!
    * These must be returned to the makerspace after EVERY MEETING.
* Driving in a straight line.  We need to track down the issue with the weird driving directions on NoNo.
    * Verify that Phoenix5 and Phoenix6 libraries are installed correctly (see Updates).
    * Verify wheel offsets are correct.
        * Do they all line up when the robot is flipped over?
        * Verify that the absolute encoders start in "boot to absolute" mode
            * [See issue on ChiefDelphi](https://www.chiefdelphi.com/t/official-sds-mk3-mk4-code/397109/99)
        * Verify that the wheels turn the correct directions after warming up.
        * Verify that we are pulling offset data for the correct robot.
            * Possibly debug with: `System.out.println("Robot ID: " + RobotName);`
* Shooter manual control
    * Command for spin up & fire
    * Command for set shooter angle (an external encoder will measure the angle)
> Note: Once the shooter is installed, the drive team will definitely want the robot in a drivable condition at the end of every meeting!
* Vision "turn to colored ring" command
    * This needs to turn faster
    * We will require this for the auto routine, so it has to work as a command
    * Can we make this work while the robot is moving forward?
* Vision "set shooter angle at Speaker" command

## Lower priority tasks
* Driving speed scaling
    * Move the ANGULAR speed limit up to the top of `Constants` with the others.
* Switch CANcoder code over to Phoenix6 library.  This will clean up all the "depreciated" warnings in `SwerveModuleOffboard.java`.  Here is [example code for the CANcoder](https://github.com/CrossTheRoadElec/Phoenix6-Examples/blob/main/java/CANcoder/src/test/java/CANcoderTest.java).  Comment out the old code!  The firmware on the CANcoders will need to be updated before this will work!
* Continue trying to add tabs to Shuffleboard. We will need this to make a display that only shows what the driver needs to see. [Found a possible example](https://www.chiefdelphi.com/t/shuffleboard-crashes-on-launch-for-no-apparent-reason/430714/6)
* Joystick rumble works on the new XBox controllers! Jacob would like "ready to shoot" feedback at competition (aimed at speaker and angle is correct). After testing the joystick rumble (click on the Rumble bars in the driver station joystick debug window), [try making the joystick rumble from code](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#miscellaneous-examples).
* Calibrate the cameras so that we can use 3D measurements of AprilTags.
    * [Limelight calibration routine](https://docs.limelightvision.io/docs/docs-limelight/performing-charuco-camera-calibration)
    * [Photonvision calibration routine](https://docs.photonvision.org/en/latest/docs/calibration/calibration.html)
* Clean up "unused" warnings in `Robot.java` when logging is not enabled with: `@SuppressWarnings("unused")`
