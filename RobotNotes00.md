# Robot notes 00: Tasks for 1/20/2024 meeting

## Ben - Drivetrain
1. In DriveSubsystem, delete `import frc.robot.Constants.DriveConstants;` and change all references to `SwerveConstants`.
2. In Constants, delete all of `public static final class DriveConstants`. (this was old code for Thomas)
3. Constants and calculations to correct:
   1. In Constants.ConstantsOffboard, add kMaximumSparkMaxRPM = 5700;
   2. In HolonomicPathFollowerConfig, calculate true maxModuleSpeed as DRIVE_RPM_TO_METERS_PER_SECOND * kMaximumSparkMaxRPM
   3. Next to that, calculate correct driveBaseRadius as `sqrt(kWheelBase*kWheelBase + kTrackWidth*kTrackwidth)`
   4. Actually, we need to do a DEEP CLEAN of all the constants.  Right click on EACH constant name, and use the `Find All References` option.  Anything that is not used in our code needs to be commented out.  Ask me about anything questionable.
   5. Set the `MaxSpeed` constants for driving (not turning) to 4.0 (absolute max is 4.4).
4. Filter and limit the joystick signals. (Hint below from [old Swivels code](https://github.com/FRC-Team8744/Swivels_SwerveBot/blob/main/src/main/java/frc/robot/Robot.java))
```java
  // Slew rate limiters to make joystick inputs more gentle; 1/3 sec from 0 to 1.
  private final SlewRateLimiter m_xspeedLimiter = new SlewRateLimiter(3);
  private final SlewRateLimiter m_yspeedLimiter = new SlewRateLimiter(3);
  private final SlewRateLimiter m_rotLimiter = new SlewRateLimiter(3);

    // Get the x speed. We are inverting this because Xbox controllers return
    // negative values when we push forward.
    final var xSpeed =
        -m_xspeedLimiter.calculate(MathUtil.applyDeadband(m_controller.getLeftY(), 0.02))
            * SwerveConstants.kMaxSpeedMetersPerSecond;

    // Get the y speed or sideways/strafe speed. We are inverting this because
    // we want a positive value when we pull to the left. Xbox controllers
    // return positive values when you pull to the right by default.
    final var ySpeed =
        -m_yspeedLimiter.calculate(MathUtil.applyDeadband(m_controller.getLeftX(), 0.02))
            * SwerveConstants.kMaxSpeedMetersPerSecond;

    // Get the rate of angular rotation. We are inverting this because we want a
    // positive value when we pull to the left (remember, CCW is positive in
    // mathematics). Xbox controllers return positive values when you pull to
    // the right by default.
    final var rot =
        -m_rotLimiter.calculate(MathUtil.applyDeadband(m_controller.getRightX(), 0.02))
            * SwerveConstants.kMaxAngularSpeed;
```
5. Download to Swivels and see if all that fixed any problems.
6. We still need to tune the drive and angle motor PIDs.  Consider adding the ability to update PIDs (of all 4 motors in each group) like in [REV's SparkMax examples](https://github.com/REVrobotics/SPARK-MAX-Examples/blob/master/Java/Velocity%20Closed%20Loop%20Control/src/main/java/frc/robot/Robot.java):
```java
    // read PID coefficients from SmartDashboard
    double p = SmartDashboard.getNumber("P Gain", 0);
    double i = SmartDashboard.getNumber("I Gain", 0);
    double d = SmartDashboard.getNumber("D Gain", 0);
    double iz = SmartDashboard.getNumber("I Zone", 0);
    double ff = SmartDashboard.getNumber("Feed Forward", 0);
    double max = SmartDashboard.getNumber("Max Output", 0);
    double min = SmartDashboard.getNumber("Min Output", 0);

    // if PID coefficients on SmartDashboard have changed, write new values to controller
    if((p != kP)) { m_pidController.setP(p); kP = p; }
    if((i != kI)) { m_pidController.setI(i); kI = i; }
    if((d != kD)) { m_pidController.setD(d); kD = d; }
    if((iz != kIz)) { m_pidController.setIZone(iz); kIz = iz; }
    if((ff != kFF)) { m_pidController.setFF(ff); kFF = ff; }
    if((max != kMaxOutput) || (min != kMinOutput)) { 
      m_pidController.setOutputRange(min, max); 
      kMinOutput = min; kMaxOutput = max; 
    }

    SmartDashboard.putNumber("SetPoint", setPoint);
    SmartDashboard.putNumber("ProcessVariable", m_encoder.getVelocity());
```
7. If by some miracle we get things working, add a constant to Enable or Disable telemetry logging.
8. After that, let's figure out how to trigger a command with Path Planner.

## Preston & Carson - LEDs and Vision
1. Goal: make an LED command that will light up a specific LED when triggered by a joystick button press.
2. You already have a subsystem for the LEDs that can light up one LED.  Expand this so the LED subsystem method takes in the LED position and color as parameters.  Test.
3. Now, create a command that will control the LED subsystem.  Hint: you must pass in the LED subsystem object from `RobotContainer` like we did when we passed in the drive subsystem [here.](https://github.com/FRC-Team8744/ThomasCanGo/blob/main/src/main/java/frc/robot/commands/DriveDistance.java)
4. Once you have a command set up, bind a joystick trigger to the command: [Binding commands to joystick triggers.](https://docs.wpilib.org/en/stable/docs/software/commandbased/binding-commands-to-triggers.html)  Have the middle LED light up green when the button is not pressed, and red when the button is pressed. Test.
5. Back to vision!  Pass in the vision subsystem like you did the LED subsystem.  When the button is pressed, the LED should turn yellow if the Limelight does NOT have a target in view.  Test target or no target.
6. If a target is in view, the LED should turn red and move along the bar depending on the Y axis angle of the target.  Demo!
7. Bask in the glory...

## Henrik - Mechanisms
1. Create motor control prototype subsystems for the intake, shooter (flywheels, feeder, shooter angle) and climber.
2. Think about what each subsystem will need to do and create methods to control them (spinMotor, stopMotor, setVelocity, moveToAngle, etc.)
3. Here are some references for guidance and help:
   * [Creating an intake subsystem example](https://github.com/FRC-Team8744/FRC_2024_SoftwareLessons/blob/main/Lesson07.md#intake-subsystem-steps)
   * [Command we used to drive Thomas an exact distance](https://github.com/FRC-Team8744/ThomasCanGo/blob/main/src/main/java/frc/robot/commands/DriveDistance.java)

## Trenton - Read a digital input
1. Your goal is to read the state of a digital input so that we can learn to use the optical interruptors for detecting game Notes (orange rings).
2. Hook up a switch [like this](https://docs.wpilib.org/en/stable/docs/hardware/sensors/digital-inputs-hardware.html#connecting-a-simple-switch-to-a-dio-port).  Ask for help, I have hardware for you (if I am unavailable, ask Jacob).
3. Read the state of the switch in the `periodic()` method of `DriveSubsystem` and send the state to Shuffleboard.  Here are references:
   * [Digital Inputs - Software](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/sensors/digital-inputs-software.html)
   * [Sending data to Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/dashboards/shuffleboard/getting-started/shuffleboard-displaying-data.html#displaying-values-in-normal-operating-mode-autonomous-or-teleop)
4. Demonstrate the measurement change on Shuffleboard when someone pushes the switch.
5. Wire up an infrared optical interruptor.  I have hardware for you.  Ask questions.  Lots of questions.  Don't burn out any hardware:
   * [IR beam break sensors](https://docs.wpilib.org/en/stable/docs/hardware/sensors/proximity-switches.html#photoelectric-proximity-switches)
   * [Adafruit optical interruptor](https://www.adafruit.com/product/2167)
6. Find out from Jacob how the beam break sensor will be mounted on the shooter.  (Does it interrupt when the Note is in the correct position, or just when it is picked up from the floor)
7. Think of other uses for these sensors.  Could we determine the velocity of a ring by how long the sensor is blocked?
