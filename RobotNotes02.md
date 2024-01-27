# Robot notes 02: Tasks for 1/27/2024 meeting

## Carson - New robot bring up
1. The new robot with the red bumper needs to be checked out.  I updated the motor driver firmware, RoboRio, and radio.  But my first test at deploying code did not seem to work - figure it out!
2. When we can download code, we need to verify the steering offsets that Jacob calculated.  This involves turning off swerve optimization and locking the wheels in place - talk to me.
3. Make the bumper fit!

## Trenton - Merge Ben's drivetrain work into Main
1. Do not add anything that is still experimental.  At the time of writing, I see your DigitalInput code in Ben's DriveSubsystem.  This is not where it will eventually end up, so don't add that to Main right now.  Ask questions.
2. Think about what the driver will want to see on the Shuffleboard screen at competition.  How do we get there?
3. Figure out how to send Shuffleboard data to a different tab without crashing the program.  This is kind of an issue...

## Ben - Drivetrain
1. The drive motors are reasonably tuned for now, but we haven't checked the turning motors.  Send "commanded angle" and "measured angle" data for the FrontLeft wheel to Shuffleboard like we did for the drive wheels.
2. We will need to collect data with the "drive in a square" path and check how well the wheels turn to target.

3. Gyro debug - send the absolute angle of the gyro to shuffleboard so we can start analyzing the "drift" issue.

5. We need to write up a tuning procedure (comments in DriveSubsystem) for the drive motors.
6. Constants clean up - if constants are commented out and it doesn't seem like something we will use in the future, delete them.

## Preston - Lock on to Vision target!
1. Now you need to add code to turn the robot with a PID controller.  You remember when we did that, [right?](https://github.com/FRC-Team8744/ThomasCanTurn2/blob/Command-with-generic-PID/src/main/java/frc/robot/commands/TurnSimplePID.java)  Add these changes (non-useful code removed):
```java
public class TurnSimplePID extends CommandBase {
  private final DriveSubsystem m_drive;
  PIDController m_turnCtrl = new PIDController(DriveConstants.kTurnP, DriveConstants.kTurnI, DriveConstants.kTurnD);
  private static final double kTurnFF = 0.0;
  private double m_output;
  private double m_heading;
  private double m_goalAngle;

  public TurnSimplePID(double targetAngleDegrees, DriveSubsystem drive) {
    m_drive = drive;
    addRequirements(m_drive);

    m_goalAngle = targetAngleDegrees;
  }

  @Override
  public void initialize() {
    m_turnCtrl.enableContinuousInput(-180, 180);
    m_turnCtrl.setTolerance(1.0);
    m_turnCtrl.setSetpoint(m_goalAngle);
    m_turnCtrl.reset();

    SmartDashboard.putData("PID", m_turnCtrl);
  }

  @Override
  public void execute() {
    m_heading = m_drive.getHeading();
    m_output = MathUtil.clamp(m_turnCtrl.calculate(m_heading) + kTurnFF, -1.0, 1.0);
    // Send PID output to drivebase
    m_drive.arcadeDrive(0.0, -m_output, false);

    // Debug information
    SmartDashboard.putNumber("PID setpoint", m_goalAngle);
    SmartDashboard.putNumber("PID output", m_output);
    SmartDashboard.putNumber("PID setpoint error", m_turnCtrl.getPositionError());
    SmartDashboard.putNumber("PID velocity error", m_turnCtrl.getVelocityError());
    SmartDashboard.putNumber("PID measurement", m_heading);
  }

  @Override
  public void end(boolean interrupted) {
    m_drive.arcadeDrive(0.0, 0.0);
  }

  @Override
  public boolean isFinished() {
    return m_turnCtrl.atSetpoint();
  }
```
2. No, that's not how you make a swerve robot turn.  How does one do that? (this is the code to run teleop in `RobotContainer`)
```java
    // Configure default commands
    m_robotDrive.setDefaultCommand(
        // The left stick controls translation of the robot.
        // Turning is controlled by the X axis of the right stick.
        new RunCommand(
            () ->
                m_robotDrive.drive(
                    -m_driverController.getLeftY(),
                    -m_driverController.getLeftX(),
                    -m_driverController.getRightX(),
                    false),
            m_robotDrive));
```
3. Set all the PID parameters to zero. Slowly increase the FeedForward parameter (kTurnFF) until the robot turns to the angle.
4. Start tuning!
5. Once it is working, convert the program so that it tracks a ring by color - not an AprilTag.

* [Tips for tuning the vision pipeline](https://docs.limelightvision.io/docs/docs-limelight/getting-started/pipelines)
* [Tuning for a color](https://docs.limelightvision.io/docs/docs-limelight/pipeline-retro/color-filtering)

## Henrik - Mechanisms
4. Wire in a dummy motor and test it?
5. Start studying [Command sequencing](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html).  You will need to spin up flywheels, set the shooter angle based on distance supplied by vision, feed in the Note for shooting, and shut things down based on DigitalInput triggers and flags for 'Flywheel at correct velocity'.
