# RAPID-Programs

Much of this GitHub branch covers the programs I wrote for simulations in my internship at ABB Nelamangala in Bangalore. 

While most simulations in the office are made non-iteratively, and therefore required significant repetitive effort, I chose to customise the structure of my RAPID programs by self-learning certain elements of RAPID syntax, in order to make my programs easier to write and navigate. With time and experimentation, I found, however, that non-iterative program structures lent us more control over the joint configuration the robot adopted at each target. I was therefore able to exercise significantly more discretion in choosing which method to use for simulation design. 

One of the more valuable tasks I pursued in this internship, was the design of station logic. Below, I present and briefly explain the station logic for some salient stations. 

# Visual Inspection Station Logic
<img width="1337" height="639" alt="Screenshot 2025-07-27 115501" src="https://github.com/user-attachments/assets/c334b20a-2d24-4a83-a945-18f126f4dc2f" />

This station involves the placement of 5 sensors, 2 at visual inspection fixtures and 3 at the entry points for this station. This station logic involves multiple signals, the purposes of which are listed below
1. Identification of which part is being transferred so that only the appropriate attacher and detachers are signalled
2. Set or Reset signals: PlaneSensors in RobotStudio are event-triggered, so they do not maintain a constant output of 1 even if they are being intersected. Consequently, something with a more persistent output state was required to supplement this behaviour, and so LogicSR Latches were chosen to do so. Thes blocks output 1 even if the Set signal is pulsed instead of set to 1, and output 0 when Reset is pulsed.
3. Operational completion signals: Signals like T1sdone or T1edone reflect that the first track robot has finished picking and placing a workpiece into the relevant station, which is logically when the next robot in line should commence its operations
4. Attachment or Detachment Signals: RobotStudio offers attacher and detacher blocks to simulate the physical process of carrying workpieces in assembly lines. These blocks require trigger to perform their operations. These triggers' timings, however, can be fixed within the RAPID programs themselves based on when in a path a signal needs to be triggered 
