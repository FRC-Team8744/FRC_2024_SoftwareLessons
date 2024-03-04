# Robot notes 08: Tasks for 3/05/2024 meeting

# Vision code
* Do not use Shuffleboard to pass data into vision COMMANDs.  All access to the camera should be through the subsystem methods.  This will reduce errors and delays.  For instance, don't send "height" data to Shuffleboard - update a private local variable and access the value with a subsystem method like getTargetHeight().
* In the vision subsystem, ALL vision data will need to be filtered.
  * Use a median filter for all aiming angles. [Median Filter Docs](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/filters/median-filter.html#median-filter)
  * We can also try a debouncer for simple on/off signals. The library allows us to specify which edge to filter.  [Debouncer Filter] (https://docs.wpilib.org/en/stable/docs/software/advanced-controls/filters/debouncer.html#debouncer)
  * If the debouncer is not working, use a low pass filter (moving average) for InView/NotInView signals.  [Moving Average Filter](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/filters/linear-filter.html#movingaverage)
* Once you commit to moving the robot, capture a COPY of your measurement information - you don't want your signal to drop out which you are controlling the robot.

# Climber
  * It's ready, right?  Make it move!

# Driver display tab
  * Only the data that the driver wants

