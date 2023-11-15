# Lesson 8: Working together through GitHub  11/16/2023

## Java Number Types Review
There are different types for bigger variables and smaller ones, but this should cover 99.99% of what we do.
* `int` whole counting numbers, including negative numbers.
  - Range: -2,147,483,648 to 2,147,483,647
* `double` numbers with a decimal or exponent in them.  
  - Range: 4.94065645841246544e-324 to 1.79769313486231570e+308
* `boolean` just true or false, that's it.
  - Range: 0 or 1

Why does the number type matter?
* 7.0 / 3.0 = 2.333
* 7 / 3 = 2
* 7 % 3 = 1  (if you want the remainder)

## Start with a clean slate
1. For this to work, you MUST have your GitHub account linked to FRC-Team8744.  Talk to me!  I have to send you an invite from FRC-Team8744 to give you editting access.  I must have your full and complete GitHub user name.
2. Go to VSCode, open the project `ThomasCanGo` from last week and make sure you have the `main` branch open!
3. Create a NEW branch for YOUR code.  Think about the name!  This will be uploaded to GitHub, where the public can see it!  Do not give our team a bad reputation or you will lose access!
4. Pick something out of the following list and experiment with making it work:

### Pick your subsystem or sensor
Subsystems will require creating a new subsystem file.  Sensors can just be added into `DriveSubsystem`.
* Spin the intake (if you missed Tuesday's meeting - you know who you are!)
* Track an AprilTag with the Limelight
  - [Limelight Docs](https://docs.limelightvision.io/docs/docs-limelight/getting-started/programming)
  - [Example from Thomas](https://github.com/FRC-Team8744/Scrappy_2023/blob/main/src/main/java/frc/robot/subsystems/Limelight.java)
* Measure distance with an ultrasound sensor.
  - [Ultrasound Sensor Example](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#sensor-examples) - this also shows an example of how to use "Test" in the Driver Station.
* Measure current flowing through different channels of the Power Distribution Panel (PDP)
  - [PDP measurement example](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#sensor-examples)
* Blink LEDs! Pick a pattern and light it up!
  - [LED code from last season](https://github.com/FRC-Team8744/Scrappy_2023/blob/main/src/main/java/frc/robot/subsystems/LEDLights.java)
  - [WPILib example](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#miscellaneous-examples)
  - [WPI docs](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/misc/addressable-leds.html)
* Let's get ready to RUMBLE!  Make the XBoxJoystick rumble if... what?
  - [HID Rumble example](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#miscellaneous-examples)
* Use tabs to separate data on Shuffleboard. (CAUTION: Errors with this WILL crash your program)
  - [Shuffleboard example](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#miscellaneous-examples)
* Moving a hobby servo with the RoboRio.
  - [WPILib example](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/servos.html#repeatable-low-power-movement-controlling-servos-with-wpilib)

## Upload to Github
1. Once you have it working, upload your changes to GitHub.
2. We probably won't try to Merge today, so look forward to that on the Tuesday after Thanksgiving!

### Other examples and resources
* [SparkMax Code Examples](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-code-examples)
