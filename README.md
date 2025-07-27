# RAPID-Programs

Much of this GitHub branch covers the programs I wrote for simulations in my internship at ABB Nelamangala in Bangalore. 

While most simulations in the office are made non-iteratively, and therefore required significant repetitive effort, I chose to customise the structure of my RAPID programs by self-learning certain elements of RAPID syntax, in order to make my programs easier to write and navigate. With time and experimentation, I found, however, that non-iterative program structures lent us more control over the joint configuration the robot adopted at each target. I was therefore able to exercise significantly more discretion in choosing which method to use for simulation design. 

One of the more valuable tasks I pursued in this internship, was the design of station logic. Below, I present and briefly explain the station logic for some salient stations. It is noteworthy that the station logics presented below as well as the RAPID programs in this GitHub branch were made purely on the basis of my independent experimentation with RAPID syntax and the available Smart Components in RobotStudio. 

# Visual Inspection Station Logic
<img width="1337" height="639" alt="Screenshot 2025-07-27 115501" src="https://github.com/user-attachments/assets/c334b20a-2d24-4a83-a945-18f126f4dc2f" />

This station involves the placement of 5 sensors, 2 at visual inspection fixtures and 3 at the entry points for this station. This station logic involves multiple signals, the purposes of which are listed below
1. Identification of which part is being transferred so that only the appropriate attacher and detachers are signalled
2. Set or Reset signals: PlaneSensors in RobotStudio are event-triggered, so they do not maintain a constant output of 1 even if they are being intersected. Consequently, something with a more persistent output state was required to supplement this behaviour, and so LogicSR Latches were chosen to do so. Thes blocks output 1 even if the Set signal is pulsed instead of set to 1, and output 0 when Reset is pulsed.
3. Operational completion signals: Signals like T1sdone or T1edone reflect that the first track robot has finished picking and placing a workpiece into the relevant station, which is logically when the next robot in line should commence its operations
4. Attachment or Detachment Signals: RobotStudio offers attacher and detacher blocks to simulate the physical process of carrying workpieces in assembly lines. These blocks require trigger to perform their operations. These triggers' timings, however, can be fixed within the RAPID programs themselves based on when in a path a signal needs to be triggered

Finally, the programming approach for the Visual Inspection is noteworthy. As I could not produce a stack  using RAPID functionalities, I opted for a WHILE loop that checks a sequence of IF...THEN conditions in sequence of importance. While this sequencing is sub-optimal, as all checking should be done on a temporal basis and parallelly in practical contexts, it is as close to  continuous task sequencing as one can come with RobotStudio's RAPID kernel. Additionally, fusing uni-directional conditional logic with WHILE loops allows the robot to simulate constant monitoring of other robots, and also allows it to maintain memory of which tasks it is assigned, as the conditional logic would respond to instruction signals until those signals are turned off by the executing robot or sensors in the station, which is a very achievable architecture in RobotStudio and RAPID. 

<img width="864" height="561" alt="Screenshot 2025-07-27 120503" src="https://github.com/user-attachments/assets/74d2b36c-5fa6-44a9-8bfa-dd31462d22c3" />

This image contains all the Attachment and Detachment blocks used  in the Pick and Place operations for the two track robots in this station (note: Track robots refer to IRB robots mounted to an external axis, in particular a robot track which adds a linear axis to the robot's frames). 

# Assembly Line Station Logic
<img width="1334" height="685" alt="Screenshot 2025-07-27 114650" src="https://github.com/user-attachments/assets/89796bdc-12ec-408b-b5a4-47c3a18e708b" />

This Station Logic construction is visibly less complicated than the one detailed above, as this construction was used as a testbed for connecting RobotStudio controllers to each other. This station logic construction was in fact defined to execute a single process cycle for one set of workpieces. As a consequence, there was no need to establish handshakes between different robots, and signals reflecting permission to commence an operation, or the conclusion of one, were used. 

This station logic reflects less parallelism  than for the Visual Inspection station as well, as the central track robot responsible for all pick and place operations in this station did not maintain a memory of other robots' states. I attempted to experiment with creating such a memory using an array, and using robot signals to create a stack or queue of tasks that the pick and place robot would need to execute based purely on recency. However, as RAPID is unable to execute programs in parallel (other than the CONC block, which, however, only allows one to quasi-concurrently run two Robot Tasks concurrently, and not two sub-processes under the same task, which was my requirement) and one cannot append data to RAPID arrrays as one does in Python, I was inspired to create a newer quasi-concurrent architecture for the Visual Inspection Station. 
