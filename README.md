# First-Order Boustrophedon Navigator

# SETTING UP THE PROJECT - 

Step 1: Fork and Clone the Repository

Since the assignment is hosted in a GitHub repository, started by forking it:

    DREAMS-lab/RAS-SES-598-Space-Robotics-and-AI.
    Click Fork (top-right) to create a personal copy of the repository.
    Cloned the forked repository:
    git clone https://github.com/YOUR_USERNAME/RAS-SES-598-Space-Robotics-and-AI.git


Step 2: Setting up the ROS2 Environment

Since I already installed ROS2(Humble) proceeding to create a symlink -

    cd ~/ros2_ws/src
    ln -s ~/RAS-SES-598-Space-Robotics-and-AI/assignments/first_order_boustrophedon_navigator 


Step 3: Building the package-
         
    colcon build --packages-select first_order_boustrophedon_navigator
    
    source install/setup.bash


# PD CONTROLLER TUNING-

Tuning the PD (Proportional-Derivative) controller is essential to minimize cross-track error while maintaining smooth motion. The process involves adjusting Kp (Proportional Gain) and Kd (Derivative Gain) for both linear and angular velocities.


*Final PD Controller Parameter Values* -

    Kp_linear - 10  - Ensures fast resopnse
    Kd_linear - 0.1 - Reduces Oscillations
    Kp_angular -7.0 - Keep Turns smooth and accurate
    Kd_angular - 0.05 - Minimizes Overshoot in tuning
    spacing - 0.8 - Maintains the spacing accurately

Tuning Approach -
# Kp_linear -
Started with low values for Kp to prevent aggressive movements.
Ensuring the robot moves forward efficiently while maintaining minimal deviation from the desired trajectory.

Procedure:

    Started with Kp_linear = 2.0 → Robot moved slowly but didn't correct path deviations     effectively.
    Increased Kp_linear = 5.0 → Improved path tracking but led to oscillations.
    Further increased Kp_linear = 10.0 → Optimal balance between speed and accuracy.

Final Value - Kp_linear = 10.00;
Justification: Ensures the robot moves at an optimal speed without excessive overshoot

# Kd_linear - 
Dampening Oscillation and preventing Overshoot.

Procedure:

    Started with Kd_linear = 0.0 → Large overshoots due to aggressive correction from Kp_linear.
    Increased Kd_linear = 0.05 → Reduced overshoot slightly.
    Finally Set the Kd_linear = 0.1 → Achieved smooth, damped motion.

Final Value - Kd_linear = 0.1;
Justification: Ensures smooth velocity transitions and prevents jerky motion

# Kp_angular -
Improving turning precision, ensuring the robot dosent follows sharp turns with excessive corrections.Decreasing the value resulted in sharper turns which resulted in less area being covered by the bot while taking turns.

Procedure:

       
    Started with Kp_angular = 3.0 → Robot turned too slowly, missing waypoints.
    Increased Kp_angular = 6.0 → Improved turning accuracy.
    Set Kp_angular = 7.0 → Achieved optimal turns with excessive corrections.

Final Value - Kp_angular = 7.0;
Justification: Ensures precise and efficient turns,covering much area and reducing unnecessary deviations.

# Kd_angular -
Prevent excessive oscillations in movment.

Procedure:

    Started with Kd_angular = 0.0 → Robot wobbled excessively in turns.
    Increased Kd_angular = 0.02 → Minor reduction in oscillations.
    Set Kd_angular = 0.05 → Optimal balance between damping and responsiveness.

Final Value - Kd_angular = 0.05;
Justification: Damps excessive heading oscillations, ensuring stable turns.

# Spacing -
Space between adjacent path was reduced to cover as much area as possible for much accurate results while using boustrophedon motion.

Procedure:

    Started with spacing = 1.0 → Left gaps between coverage paths.
    Reduced spacing = 0.6 → Caused redundant overlapping paths.
    Set spacing = 0.8 → Achieved near-complete coverage with efficient path traversal.

Final Value - Spacing = 0.8;
Justification: Ensures maximum coverage while maintaining efficient path execution.
   


# ANALYZING PERFOMANCE METRICS -
    
  ![Screenshot from 2025-01-27 20-12-26](https://github.com/user-attachments/assets/41c9fc4c-5aec-4ff1-b48b-d0da4dbf12b4)


    Average Cross-Track Error : 0.158
    Maximum Cross-Track Error : 0.310 



# Plotting Values -
Graphs for -

    /turtle1/pose/x
    /turtle1/pose/y
    /turtle1/cmd_vel/linear/x
    /turtle1/cmd_vel/angular/z
    /cross_track_error
![image333](https://github.com/user-attachments/assets/8f4aa32b-4928-46a2-ab65-bcf90fb2d0eb)


![image222](https://github.com/user-attachments/assets/055c6c5b-c9ea-4982-b3f3-57cf57018724)






# Image of the boustrophedon_navigator

![Screenshot from 2025-01-27 18-58-05](https://github.com/user-attachments/assets/ba733d35-6f24-4db4-a9bd-efb1477f92f8)


Spacing Justification (spacing = 0.8)

    The spacing parameter determines the distance between adjacent sweeps in the boustrophedon pattern.
    A spacing too small (<0.6) would result in redundant coverage, causing inefficiency.
    A spacing too large (>1.0) could leave uncovered areas, leading to gaps in the path.
    Optimal Value: 0.8
    Ensures full coverage while minimizing redundant movements.
    Results in an efficient and compact path, as seen in the image.


# Challenges and Solutions

Challenges and Solutions in Tuning the PD Controller for Boustrophedon Navigation

1. High Cross-Track Error in Initial Runs

Challenge:

    During early tests, the robot deviated significantly from the desired path, especially at turns.
    The cross-track error sometimes exceeded limit of 5.0, making the trajectory inaccurate.

Solution:

    Decreased Kp_linear from 10.0 to 8.0 to make the robot respond faster to path deviations.
    Fine-tuned Kd_linear to 0.1 to dampen oscillations and prevent overcorrections.
    Adjusted Kp_angular to 7.0 to improve turn stability, ensuring smoother transitions between rows.

2. Overshooting and Oscillations at Turns

Challenge:

    The robot tended to overshoot at sharp turns due to excessive angular momentum.
    The turns were not smooth, causing erratic movement.

Solution:

    Lowered Kd_angular to 0.05, reducing aggressive damping that slowed down corrections.
    Slowing the robot before a turn.
    Verified smooth transitions using rqt_plot to monitor velocity changes.

3. Uneven Coverage Due to Improper Spacing

Challenge:

    Initially, the spacing was set at 1.0, causing slight gaps between coverage lines.
    Reducing it to 0.6 resulted in excessive overlap, wasting time.

Solution:

    Optimized the spacing to 0.8, achieving full coverage without redundant passes.
    Ensured that coverage efficiency was maximized by visualizing the trajectory in rqt_plot.

4. Velocity Instability in Straight-Line Motion

Challenge:

    At high speeds, the robot showed unstable velocity fluctuations, especially on straight segments.

Solution:

    Tuned Kd_linear to 0.1, reducing oscillations without affecting speed.
    Verified stability using velocity profiles plotted with rqt_plot.
    Ensured a smooth velocity ramp-up instead of sudden acceleration changes.

5. Debugging and Visualization Difficulties

Challenge:

    Analyzing real-time performance required monitoring multiple ROS2 topics simultaneously.
    Debugging errors in trajectory tracking was difficult without proper visualization.

Solution:

    Used rqt_plot to track:
        /turtle1/pose/x and /turtle1/pose/y for real-time trajectory monitoring.
        /cross_track_error to identify deviations from the planned path.
        /turtle1/cmd_vel/linear/x and /turtle1/cmd_vel/angular/z to analyze velocity profiles.
    Improved logging and debugging by printing key control values at each step.

