Software
## Modular Code Structure

The software serves as the intelligence center of the project, employing a modular design approach. Each class and task are meticulously organized into individual files. The main code, responsible for orchestrating the entire system, instantiates these classes and initiates the scheduler.

## Task Management

The priority multi-tasking functionality is facilitated by two essential files: cotask.py and taskshare.py, provided by Dr. JR Ridgely. The task diagram below visually represents the architecture of the software.

<p align="center">
  <img src="/docs/assets/images/task_diagram.JPG" />
</p>

## Task Prioritization and Frequency

Tasks are treated as external peripherals, either under control or providing data for analysis. Each task is assigned a priority and frequency, establishing a hierarchy in task importance. Higher priority values indicate greater significance. Tuning the period of each task through testing is crucial for optimal robot performance. For instance, motors are given higher priority and frequency compared to reflectance sensors, influencing the robot's responsiveness on the track.

## Task Intercommunication

Arrows in the task diagram represent 'queues,' enabling tasks to communicate vital information about their current state and requirements. For instance, sensor data is communicated to the motors to determine the appropriate velocity, controlling the robot's speed and direction. A limit switch communicates the proximity of obstacles, prompting the motors to adjust accordingly.

## Control Algorithm

The project operates on an open-loop control algorithm, where no attribute or variable is actively controlled or monitored for errors. Although an ideal scenario involves controlling either motor velocity or sensor data for precise line tracking, the decision was made to adopt a finite state machine (FSM) approach within an open-loop system due to the project's scope.

## Finite State Machines

### Sensor Task FSM
<p align="center">
  ![406034740_1016743952745843_1284910993470777946_n](https://github.com/Ayush17318/line-follower/assets/124316330/8aba5927-d3cb-467c-81cb-a483d2d0164a)

</p>

This task continuously reads the data from each of the six sensor values and classifies these into many different logic states. Most of this classification occurs in State 0 and then, the task sends the command to the motor tasks using a right and left motor queue. The sensor logic is outlined in the following table, where 0 means the sensor is reading white under it and 1 means the sensor is reading black under it:

| Logic State           | 11 | 9 | 7 | 5 | 3 | 1 |
|-----------------------|----|---|---|---|---|---|
| Straight              | 0  | 0 | 0 | 0 | 0 | 0 |
|                       |    | 0 | 1 | 1 | 0 |   |
|                       | 1  | 1 | 1 | 1 | 1 | 1 |
|                       |    |   |   |   |   |   |
| Soft Left (1)         |    |   | 1 | 1 |   |   |
| Left (2)              |    |   | 1 | 0 |   |   |
| Hard Left (3)         | 1  | 0 | 0 | 0 | 0 | 0 |
| Harder Left (4)       |    | 1 | 1 | 1 |   |   |
| Pivot Left (5)        |    | 1 | 1 |   |   |   |
| Hard Pivot Left (6)   | 1  | 1 | 1 |   |   |   |
|                       |    |   |   |   |   |   |
| Soft Right (1)        |    |   | 1 | 1 |   |   |
| Right (2)             |    |   | 0 | 1 |   |   |
| Hard Right (3)        | 0  | 0 | 0 | 0 | 0 | 1 |
| Harder Right (4)      |    |   | 1 | 1 | 1 |   |
| Pivot Right (5)       |    |   |   | 1 | 1 |   |
| Hard Pivot Right  (6) |    |   |   | 1 | 1 | 1 |



### Motor Task FSM
<p align="center">
  <img src="/docs/assets/images/mot_gen_fsm.JPG" />
</p>

This motor task reads integer values from the turn_q_r and turn_q_l queues to inform the right and left motors what the sensor is reading respectively. The integer from the queues is the corresponding state the motor task generator moves into. All states immediately return to the main hub state after one passthrough except for state 15, which involves the obstacle detection path. This task remains in this state for the total duration of the robot going around the obstacle, which involves backing up, turning, driving forward, turning, driving forward, and completing a large arc until the robot meets up with the line on the other side of the wall. During this task, the motor task sends a signal to the sensors in sensors_q to "turn off", or to skip collecting data and filling up the sensor queue while the robot is following the pre-programmed path. 

### Switch Task FSM
<p align="center">
  ![405903779_319239134335880_1500942994180926342_n](https://github.com/Ayush17318/line-follower/assets/124316330/8cc112bc-252b-492d-9e5a-41c96575ae9a)

</p>

This simple task continuously checks to see if the limit switch was triggered. Once triggered, it moves into states 1 and 2, where the limit switches are disabled for the remainder of the robot's run

## Code Documentation
Comprehensive Doxygen comments have been provided in the codebase for each Python file. The detailed documentation is accessible [here](https://ayush17318.github.io/Term-Project/).

#### [Home](./README.md) 
