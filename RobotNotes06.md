# Robot notes 06: Tasks for 2/22/2024 meeting

What needs to be done?  Here are some suggestions:

## Ben - rotation speed
Use `Go to References` to find where the constants are in the code.  It looks to me like `MAX_ANGULAR_RADIANS_PER_SECOND` is only used by the driver joystick, so it should be safe to scale.  What is the maximum the robot is capable of?  Calculate the circumference of the circle a wheel would travel through if its axle was pointed at the center of the robot.  How many turns of the wheel will cover that distance?  What is the highest RPM possible for the motor? (Don't forget gearing - all the numbers are in the constants file)  Math is awesome!  Use a whiteboard, show your work!

## Henrik - flywheel speed
You will need to adjust your code so that you are setting a PID for the wheel velocity - not just raw power.  We will have to tune it with the Rev software before we know what the constants are, but you can set up the framework.  Where else do we do a similar thing?  Look at how the drive motor is set up in `SwerveModuleOffboard`.  Look at the configuration in `configureDevices()` and how the velocity setpoint is programmed in `setDesiredState()`.

## Trenton - modify code to work with Swivels
Before we can fix Auto, we have to fix the code so that it will run on Swivels without complaining.  You will need to do the same thing in `RobotContainer` that we did in `Multi_IMU`.  Read in the RobotName, then decide if we should instantiate the `Shooter` subsystem.
* Look at how the `Shooter` subsystem is created in `RobotContainer`:
```java
public class RobotContainer {
  // The robot's subsystems
  public final DriveSubsystem m_robotDrive = new DriveSubsystem();
  public final Intake m_intake = new Intake();
  public final Shooter m_shooter = new Shooter();
  public final Vision m_vision = new Vision();
  public final LEDS m_leds = new LEDS();
```
* Now look at `Multi_IMU` - see how the `new` part gets moved:
```java
public class Multi_IMU extends SubsystemBase {
// Stuff removed

  // The imu sensors
  private PigeonIMU m_imu_pigeon1;
  private AHRS m_imu_navX2_micro;
  private Pigeon2 m_imu_pigeon2;

  // Robot type
  private String m_whoami;
  private int m_imuSelected;
  private boolean m_pigeon1_Enable = false;
  private boolean m_pigeon2_Enable = false;
  private boolean m_navx2_Enable = false;

  public Multi_IMU() {
    m_whoami = Preferences.getString("RobotName", "Undefined");
    switch (m_whoami) {
      case "Swivels":
          m_imuSelected = PIGEON1;
          m_pigeon1_Enable = true;
        break;
    
      case "NoNo":
          m_imuSelected = PIGEON2;
          m_pigeon2_Enable = true;
        break;
    
      default:
          m_imuSelected = PIGEON1;
          m_pigeon1_Enable = true;
        break;
    }

    // Attempt to communicate with new sensor (may not exist)
    if (m_pigeon1_Enable) m_imu_pigeon1 = new PigeonIMU(PIGEON1_kIMU_CAN_ID);
```
* See the change?  Ask questions.


## Preston - camera calibration
You know what to do!  Work on it and tell us if there are any problems.  If you finish the PhotonVision camera, work on the Limelight.
