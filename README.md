# multimodal fencing action dataset（MFAD）
​	In universal intelligent training sport systems, only basic action recognition tasks can be accomplished, and they lack the ability to provide effective advice to coaches and trainers to assess the level of athletes. In order to refine the shortcomings of fencing training systems and to address the problem of limited attention of coaches, as well as to promote research on intelligent training systems, we contribute a multimodal fencing action recognition dataset (MFAD).MFAD is a novel multimodal fencing action dataset that is mainly used for action recognition and level assessment. We collected data from 27 fencers and obtained 928 action sequences including 329 lunges, 324 one-step forward lunges (OSFL), and 275 jumps across three different fencing movements and three levels. The data patterns we collected consisted of two types of data, sensor data and skeletal data, which were collected via sensors and depth cameras, respectively.



## Devices

### sensor(IMUs)

During the data collection process, we used two IMUs devices to collect motion signals from the fencers.

 Our first device consisted of an IMU bracelet and an app for mobile phone. As shown in Fig. 1, the smart wrist bracelet included an on/off switcher, a triaxial accelerator to measure angular velocity, acceleration, and speed in the $x, y, z$ axes. The IMU (2.5mm $\times$ 3.3mm) integrated into the bracelet provided specific parameters, measuring angular velocity and acceleration within a range of 16g and 2000°/s. For data transmission, the  Bluetooth chip (5mm $\times$ 5mm) within the bracelet sampled data at a 50Hz rate, suitable for most sports motion capture.

<img src=".\images\bracelet .jpg" alt="bracelet " style="zoom: 25%;" />

​																			Fig.1 smart bracelet 

Secondly, the smart insoles, we used the same IMU to capture gait features during different phases of fencing movements. To ensure IMU stability, we enclosed it in a hard casing, as shown in Fig. 2, designed to fit into the concave notch inside each insole. The insoles accommodates various shoe sizes, allowing athletes of different heights and foot lengths to participate naturally. This approach is advantageous and more accurate compared to solid pressure platforms, as fencers' movements may be altered when deliberately stepping on pressure mats. The smart insoles connects to the server and mobile device like the smart bracelet but specifically captures details such as foot lifting angle, speed, and foot strike angle readings.

<img src=".\images\insole.jpg" alt="insole " style="zoom: 25%;" />

​																				Fig2. smart insoles

### Depth Camra

We use a high-speed motion camera to extract human skeleton motion data. Beside it can validate the inertial data from the IMU sensors in the smart bracelet and  insoles and to assist in data segmentation. We chose the depth camera for its suitable depth range (0.2-20m), high sampling rate (100 frames per second), and compact size (175.25 $\times$ 30.25 $\times$ 43.10 mm), ideal for capturing fast fencing motions. The advanced camera configurations offer detailed information about lunge depth, thigh-lower leg angle, arm extension angle, etc, providing valuable insights into performance differences among athletes of varying skill levels. Fig. 4 shows the extracted skeleton joints from human body.

<img src=".\images\depth camra.jpg" alt="depth camra " style="zoom: 100%;" />

​																				Fig3. depth camra

<img src=".\images\skeleton.jpg" alt="skeleton " style="zoom: 25%;" />

​																				Fig4. Human skeleton joints

## Information for "MFAD" dataset

### 3 actions

​	In fencing, we consider 3 common basic actions: lunge, one-step forward lunge, and fleche. 

The lunge is the most basic and most used attacking action in fencing. It involves thrusting the fencer's front leg forward and moving the arm and body along, as shown in Fig. 5. 

<img src=".\images\Lunge.jpg" alt="Lunge" style="zoom: 25%;" />

​																Fig.5 The action of Lunge

​	In addition, one-step forward lunge includes one more step than lunge. From 1 to 3 frame, The "one-step" is done to generate more momentum and inertia for faster and farther lunges, however the basic lunge remains the same, as shown in Fig. 6. 

<img src=".\images\One-Step Forward Lunge.jpg" alt="One-Step Forward Lunge" style="zoom:25%;" />

​													Fig.6 The action of One-step forward lunge

​	Finally, the fleche, also known as a "running attack", consists shifting all of the body weight to the front leg, which then propels the fencer forward in a streamline motion, as shown in Fig. 7. From 1 to 3 frame, the fencer runs quickly on tiptoe compared to one-step forward lung.

<img src=".\images\Fleche.jpg" alt="Fleche" style="zoom:25%;" />

​														Fig.7 The action of Fleche

### 3 levels

​	For level assessment, we classify all subject into 3 levels, elite, sub-elite and amateur athletes. See Table for the detailed definition.

| Levels   | Definition                                                   |
| -------- | ------------------------------------------------------------ |
| Amateur  | Started learning fencing and have no experiences in any major competitions |
| Subelite | Received top eight recognition in regional competitions at least  once, but have no experiences competing  in  provincial  or national competitions |
| Elite    | Have won the top three places in provincial  or national  matches at least one time |


## Data File

### skeleton data file

​	The skeleton data files are in the 'skeleton' folder. We keep each action sample in a separate npy data file.

````python
'''
--skeleton 
	-- 1_p12_a1_1.npy
    -- 1_p12_a1_2.npy
    .
    .
    .

filename: "1_p12_a1_1.npy" ,where each key piece of information is separated by a "_".
    "1":means that this fencer is an amateur,"2" means Subelite,"3" means Elite.
    "p12":represents that this sample is a action sequences of subject 12.
    "a1":means that action is lunge. "a1" means lunge, "a2" means One-step forward lunge, "a3" means fleche.
    "1": The "1" represents the number of times the action was collected.
'''

import numpy as np  
  
 
skeleton_data = np.load('1_p12_a1_1.npy')  
  
C,T,V,M=data.size()  
# C,T,V,M representing how many channels, how many frames, how many joints and how many subjects activities are included
````

### sensor data file

​	Inside the sensor folder are a number of folders, each folder representing: all the times each athlete had all the movement data samples for each movement.

````python
'''
--sensor
	-- 1121
    	-- hand.txt
        -- left_foot.txt
        -- right_foot.txt
    -- 1122
    	-- hand.txt
        -- left_foot.txt
        -- right_foot.txt
    -- 1123
        -- hand.txt
        -- left_foot.txt
        -- right_foot.txt
    .
    .
    .

Take one of the folders as an example:
Folder name: "1123".
"1|12|3". There are three important parts of this information. They are located in the first number and the middle two numbers and the last number.
    "1": represents that this fencer is an amateur .
    "12": represents that this sample is from subject 12.
    "3": represents that this action sample performs action number one, fleche.
'''
````

​	The folder should contain three txt files inside: 'hand.txt', 'left_foot.txt', 'right_foot.txt'. Each movement attempt by the subject was segmented by "0 0 0 0 0 0 1000". Each sensor recorded two main elements, angular velocity and acceleration, containing the xyz-axis, $A_x,A_y,A_z,G_x,G_y,G_z$, respectively.

> ​	If one of the files is missing, it means that there was a problem such as equipment failure during data collection and the data was lost.

```
2022-08-19-14:58:53-584 0 0 0 0 0 1000	
2022-08-19-14:58:54-600 -0.88916016 0.096191406 -0.31689453 -2.5 1.8292683 2.9268293
.
.
.
2022-08-19-14:58:57-692 -0.53759766 -0.5654297 0.5439453 -3.7804878 37.560978 18.353659	
2022-08-19-14:58:57-712 0 0 0 0 0 1000	
2022-08-19-14:59:04-096 0 0 0 0 0 1000	
2022-08-19-14:59:04-238 -0.92041016 0.22314453 -0.15527344 -4.1463413 -3.1707318 6.7073174	
.
.
.
2022-08-19-14:59:07-768 0 0 0 0 0 1000	
```