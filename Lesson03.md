[Git in VSCode Documentation](https://code.visualstudio.com/docs/sourcecontrol/overview)


Don't forget: 
[Trapezoidal Motion Profiles](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/trapezoidal-profiles.html)
[Turret Simulation](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-turret.html)

# Lesson 2: Thomas can turn! (again)

We were able to cover the technical explaination of PID last meeting, but we didn't to the actual robot!  With so many new members, we will have to backtrack a little to make sure everyone understands how to download code to the robot.

## Quick Java refresh
[Java Structure Video](https://www.youtube.com/watch?v=eIrMbAQSU34&t=239s)  <!-- 4 min -->

## FRC Robot Code Structure (Command-Based Java)
All the robot base code is built from a set of libraries maintained by WPI (Worcester Polytechnic Institute).  The library handles the many small details of getting your robot to work with your team and allowing many teams to run robots together in a competition.

* **Main.java** - DON'T CHANGE ANYTHING IN IT! :)  All this file does is start up an object called Robot.
* **Robot.java** - This is responsible for the program’s control flow and should be mostly empty.  Notice how the names of the methods declared (autonomous, teleop, test) match the operations in the FRC Driver Station.  Two important points: the CommandScheduler.getInstance().run() call in the robotPeriodic() method is essential, and the teleopInit() method cancels any still-running autonomous commands.  Only advanced programmers should modify this file.
* **RobotContainer.java** - Use this to define the basic capabilities of your robot - what the RoboRio is connected to and how to use those resources.  This is where you will bind commands to triggering events (such as buttons), and specify which command you will run in your autonomous routine.
* **Constants.java** - Non-changing robot values (such as speeds, unit conversion factors, PID gains, and sensor/motor port numbers) are stored here.
* **Subsystems** - These files encapsulate parts of the robot into easy to use software objects.  For instance, "Drivetrain" is usually used for moving the robot base around.  In your code, you just send the object a throttle and turning speed.
* **Commands** - These files make it easy to group together sequences of robot actions.  For instance, driving up to the scoring station on the playing field and throwing in a ball.

## Why do we need all this complexity?
* Robot actions happen over time (raise the arm, move to a new spot), so act/wait looping is always there
* Multiple actions need to occur at once (raise the arm, open the gripper, move forward)
* If something is not working correctly, it is easy to isolate one action
* Robot behavior needs to be changed quickly at competition

## Software checklist to setup or update
- [ ] Update the operating system!
- [ ] [FRC Game Tools](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html) (Driver Station, Shuffleboard, roboRio & radio imager)
- [ ] [WPILib VSCode](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html)
- [ ] Set up a Github account for yourself, then log in to [Github Desktop](https://desktop.github.com/)

## Visual Studio Shortcuts
* `Ctrl+Shift+P` brings up the command palette. (or click the red hexagon with the W)
* All First Robotics commands will start with `WPILib:`
* `F5` simulates the code (or runs a Romi)
* `Ctrl+F5` loads the code into a RoboRio (the competition robot)

## Documentation
https://docs.wpilib.org

## Simple Download And Test
We will now create a very simple "Timed Robot" example that will demonstrate the Proportional control principles we learned last time.  This means that we will only use "Main.java" and "Robot.java" to run the robot.  It's a very fast way to get a simple program up and running - but it makes autonomous mode DIFFICULT (we will start that later).

> [!IMPORTANT] Axis Conventions
>
>When you are writing code for your robot, we must match the standard directions used by the libraries, or our code will have serious problems.  For FRC robot bases, the positive X axis is "forward" on the robot, the positive Y axis points to the left, and the positive Z axis points up.  Rotating the robot base "to the right" is NEGATIVE rotation!
>
>Joysticks are different! The positive X axis points ahead, the positive Y axis points right, and the positive Z axis points down. However, it’s important to note that axes values are rotations around the respective axes, not translations. When viewed with each axis pointing toward you, CCW is a positive value and CW is a negative value. Pushing forward on the joystick is a CW rotation around the Y axis, so you get a negative value. Pushing to the right is a CCW rotation around the X axis, so you get a positive value.
>
>[Axis Conventions Reference](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/wpi-drive-classes.html#axis-conventions)

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

## Challenge Code
Finished with the quick test? Time for a challenge!

1. In VS Code, type `Ctrl-Shift-P` (or push the little red hexagon with a W) and select `WPILib:Create a new project`.
    * Select Example -> Java -> Gyro Drive Commands.
    * The base folder is your FRC2024 folder!
    * Yes, create a new folder from the project name (your choice).
    * Team Number: 8744 (This tells VSCode how to communicate with the robot!)
    * Do **Not** enable desktop support.
    * When you generate the project, say 'Yes' to open the code in a new VS Code window.
    * Wait until the "Build" process is complete!
2. You will need to add the library for the REV motor driver:
    * Copy the JSON line from [Rev Robotics REVLib](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
    * In the VS Code file explorer (list on left side), right-click on `build.gradle`
    * Select `Manage Vendor Libraries`
    * Select `Install new libraries (online)`
    * Paste the JSON line.
    * Build your project.
3. You will need to add the library for the Kauai Labs gyro driver:
    * Copy the JSON line from [Kauai Labs](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
    * In the VS Code file explorer (list on left side), right-click on `build.gradle`
    * Select `Manage Vendor Libraries`
    * Select `Install new libraries (online)`
    * Paste the JSON line.
    * Build your project.
4. In Constants.java, add this right after `class DriveConstants`:
```java
public static final int kLeftFrontCAN = 9;
public static final int kLeftRearCAN = 10;
public static final int kRightFrontCAN = 8;
public static final int kRightRearCAN = 7;
```
5. Also in Constants.java, add this right after `class OIConstants`:
```java
public static final int kButtonA = 1;
public static final int kButtonB = 2;
public static final int kButtonX = 3;
public static final int kButtonY = 4;
public static final int kButtonLeftBumper = 5;
public static final int kButtonRightBumper = 6;
```
6. In DriveSubsystem.java, add this right after `public class DriveSubsystem`:
```java
  // Create CAN motor objects
  private CANSparkMax leftFrontSparkMax = new CANSparkMax(DriveConstants.kLeftFrontCAN, MotorType.kBrushless);
  private CANSparkMax leftRearSparkMax = new CANSparkMax(DriveConstants.kLeftRearCAN, MotorType.kBrushless);
  private CANSparkMax rightFrontSparkMax = new CANSparkMax(DriveConstants.kRightFrontCAN, MotorType.kBrushless);
  private CANSparkMax rightRearSparkMax = new CANSparkMax(DriveConstants.kRightRearCAN, MotorType.kBrushless);

  // The motors on the left side of the drive.
  private final MotorControllerGroup m_leftMotors =
      new MotorControllerGroup(leftFrontSparkMax, leftRearSparkMax);

  // The motors on the right side of the drive.
  private final MotorControllerGroup m_rightMotors =
      new MotorControllerGroup(rightFrontSparkMax, rightRearSparkMax);
```
7. Hover the cursor over the errors and use "Quick Fix" to import the right libraries.
8. Comment out the lines which reference `PWMSparkMax`.
> [!NOTE] To comment out a group of lines in VS Code, select the lines and press `Ctrl-/`
9. Now we have to replace the encoder definitions with:
```java
RelativeEncoder m_leftEncoder = leftFrontSparkMax.getEncoder();
RelativeEncoder m_rightEncoder = rightFrontSparkMax.getEncoder();
```
10. Comment out the old encoder definitions and import the `RelativeEncoder` library with "Quick Fix".
11. Next we update the gyro:
```java
private final AHRS m_gyro = new AHRS(SerialPort.Port.kUSB);
```
12. Comment out the old gyro definition and import the right library.
> [!IMPORTANT] `SerialPort` doesn't always work right with "Quick Fix".  You might have to manually add:
> ```java
> import edu.wpi.first.wpilibj.SerialPort;
> ```
13. Comment out the `setDistancePerPulse` error lines.
14. Replace the `Encoder.reset()` errors with:
```java
m_leftEncoder.setPosition(0);
m_rightEncoder.setPosition(0);
```
15. Replace the errors `getDistance` with `getPosition`.
16. Finally, fix the last `Encoder` errors by replacing `public Encoder` with `public RelativeEncoder`.
17. Comment out any yellow warnings and build your code!
18. Next problem - in `RobotContainer.java` replace the `PS4Controller` with a `Joystick`:
```java
// The driver's controller
// PS4Controller m_driverController = new PS4Controller(OIConstants.kDriverControllerPort);
public final Joystick m_driverController = new Joystick(OIConstants.kDriverControllerPort);
```
19. Replace `getLeftY` with `getY`, and the others as appropriate.
20. If all errors and warnings are handled, download it to Thomas and give it a try.

### Independent Study
* [Zero to Robot](https://docs.wpilib.org/en/stable/docs/zero-to-robot/introduction.html)
* [VS Code Overview](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/index.html)
* [Basic Programming](https://docs.wpilib.org/en/stable/docs/software/basic-programming/index.html)
* [Inroduction to FRC software development video (old, but still very useful)](https://youtu.be/64hPDvphcfA)
* [Java Tutorial Video](https://youtu.be/eIrMbAQSU34?si=19GT3g_hVQqpSmk7)
