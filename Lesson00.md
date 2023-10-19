# Setting up for the new season!
* Select a laptop (not the driver station or a CAD laptop).  This will be your computer for the season.  Take care of it.
* If you like using a mouse, claim one and keep it with your laptop when you put it away after meetings.

Do not save any code to the Desktop!  It will quickly become difficult to organize and find what you need.

> Each laptop has a folder called C:\Users\\[PabloComputer].
> Create a folder named “FRC2024” and save all your code there.

## Github lessons
* Lessons for the season will be uploaded to: [https://github.com/FRC-Team8744/FRC_2024_SoftwareLessons](https://github.com/FRC-Team8744/FRC_2024_SoftwareLessons)

## Software checklist to setup or update
* Update the operating system!
* FRC Game Tools (Driver Station, Shuffleboard, roboRio & radio imager): [https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html)
* WPILib VSCode: [https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html)
* Set up a Github account for yourself, then log in to [Github Desktop](https://desktop.github.com/)

## Visual Studio Shortcuts
* `Ctrl+Shift+P` brings up the command palette. (or click the red hexagon with the W)
* All First Robotics commands will start with `WPILib:`
* `F5` simulates the code (or runs a Romi)
* `Ctrl+F5` loads the code into a RoboRio (the competition robot)

## Documentation
https://docs.wpilib.org

This is your best resource.  Remember it!
(Make sure you are reading the most up to date version)
We will be using Java to program the robots to keep things simple.  If you see a code example, sometimes you have to switch it to a Java view.

## Quick Start
1. We will run through the simplest possible example.  This is not the type of programming that we will focus on, because it makes autonomous routines DIFFICULT, but it is a fast way to get the robot moving.
2. Pull up [WPILib Example Projects: Basic Examples](https://docs.wpilib.org/en/stable/docs/software/examples-tutorials/wpilib-examples.html#basic-examples) in a separate tab.
3. Take a quick look at the code by right-clicking on Arcade Drive "Java".
4. After we discuss it, switch to VSCode.
5. Type `Ctrl-Shift-P` (or push the little red hexagon with a W) and select `WPILib:Create a new project`.
    * Select Example -> Java -> Arcade Drive.
    * The base folder is your FRC2024 folder!
    * Yes, create a new folder from the project name (your choice).
    * Team Number: 8744 (This tells VSCode how to communicate with the robot!)
    * Do **Not** enable desktop support.
6. Edit Robot.java to use CAN communication for the SparkMax drivers:
```
  private CANSparkMax m_leftMotor = new CANSparkMax(xx, MotorType.kBrushless);
  private CANSparkMax m_rightMotor = new CANSparkMax(xx, MotorType.kBrushless);
```
6. Hover the cursor over the errors and use "Quick Fix" to import the right libraries. (Let me know if this doesn't seem to work, we might have to import the REV motor library)
7. If all errors and warnings are handled, download it to Thomas and give it a try.
8. Turn on Thomas, but make sure the wheels are off the ground!
9. Find Thomas' WiFi access point (called "8744_Radio_2") and connect to it. (you will temporarily lose Internet access)
10. Make sure you have a joystick or controller connected to your laptop.
11. Switch over to the Driver Station and attempt to burn carpet.

### Further reading:
* [Zero to Robot](https://docs.wpilib.org/en/stable/docs/zero-to-robot/introduction.html)
* [VS Code Overview](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/index.html)
* [Basic Programming](https://docs.wpilib.org/en/stable/docs/software/basic-programming/index.html)
* [Getting Started with Romi](https://docs.wpilib.org/en/stable/docs/romi-robot/index.html)
* [Command-based programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)
* [Inroduction to FRC software development (old, but still very useful)](https://youtu.be/64hPDvphcfA)
