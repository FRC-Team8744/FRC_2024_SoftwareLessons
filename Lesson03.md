# Lesson 3: Random stuff!  10/31/2023
Since this meeting falls on Halloween, I'm not expecting too many people to show up.  If you do, we might do any of the following:

* Get everyone signed up for a Github account (required next meeting)
* Practice tuning our simple turn-to-zero robot from last time
* Possibly re-locating the IMU on Thomas
* Redo of the PID lecture if someone needs it
* Practice low-level programming of SparkMax drivers with the REV software
* Practice direct cable control of Thomas (needed when at competition)
* Learning basic Java?
* Add our own field image to a Shuffleboard field object

## Git Overview
[Git in VSCode Documentation](https://code.visualstudio.com/docs/sourcecontrol/overview)

## Quick Java refresh
[Java Structure Video](https://www.youtube.com/watch?v=eIrMbAQSU34&t=239s)  <!-- 4 min -->


## Make Thomas turn!

1. Open up [WPILib Example Projects: Basic Examples](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#basic-examples) in a separate tab.
2. Take a quick look at the code by right-clicking on Gyro "Java" in Sensor Examples.
3. After we discuss it, switch to VSCode.

> Do not save any code to the Desktop!  It will quickly become difficult to organize and find what you need.

> Each laptop has a folder called C:\Users\\[PabloComputer].
> Create a folder named “FRC2024” and save all your code there.

4. Type `Ctrl-Shift-P` (or push the little red hexagon with a W) and select `WPILib:Create a new project`.
    * Select Example -> Java -> Gyro.
    * The base folder is your FRC2024 folder!
    * Yes, create a new folder from the project name (your choice).
    * Team Number: 8744 (This tells VSCode how to communicate with the robot!)
    * Do **Not** enable desktop support.
    * When you generate the project, say 'Yes' to open the code in a new VS Code window.
    * Wait until the "Build" process is complete!

> If any windows pop up about 'trust', say yes! (Also yes to trusting the parent directory)

5. Edit Robot.java to use CAN communication for the SparkMax drivers:
```java
private CANSparkMax m_leftDrive = new CANSparkMax(9, MotorType.kBrushless);
private CANSparkMax m_rightDrive = new CANSparkMax(8, MotorType.kBrushless);
```
6. You will need to add the library for the REV motor driver:
    * Copy the JSON line from [Rev Robotics REVLib](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
    * In the VS Code file explorer (list on left side), right-click on `build.gradle`
    * Select `Manage Vendor Libraries`
    * Select `Install new libraries (online)`
    * Paste the JSON line.
    * Build your project.
    * Hover the cursor over the errors and use "Quick Fix" to import the right libraries.
7. Now we have to change the analog gyro to the NavX gyro:
```java
private final AHRS m_gyro = new AHRS(SerialPort.Port.kUSB);
```
8. You will need to add the library for the Kauai Labs gyro driver:
    * Copy the JSON line from [Kauai Labs](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
    * In the VS Code file explorer (list on left side), right-click on `build.gradle`
    * Select `Manage Vendor Libraries`
    * Select `Install new libraries (online)`
    * Paste the JSON line.
    * Build your project.
    * Hover the cursor over the errors and use "Quick Fix" to import the right libraries.
9. If all errors and warnings are handled, download it to Thomas and give it a try.
10. Make sure Thomas won't hit anything! Turn the robot on.
11. Find Thomas' WiFi access point (called "8744_Radio_2") and connect to it. (you will temporarily lose Internet access)
12. Make sure you have a joystick or controller connected to your laptop.
13. Thomas will turn to angle 0 when you enable it - be ready to turn him off!
14. Does Thomas settle to the setpoint?

> ### Shuffleboard for debug
>
> How do you see what is going on when the robot is running?  We send data to a display program called Shuffleboard.
>
> Add this line at the end of `teleopPeriodic()`:
> ```java
> SmartDashboard.putNumber("Gyro Angle", m_gyro.getAngle());
> ```
> Watch the numbers move in Shuffleboard (when the robot is disabled!).
