# skeleton3d
Render OpenPose3D skeleton data in ARENA

To use: run e.g. `python skel.py` 

This reads from MQTT topic /topic/skeleton (host oz.andrew.cmu.edu) where each message published to this topic is a comma separated list of coordinates from OpenPose3d, prefaced by a person ID, and ending with a string ("on" or "off"), e.g
```
mosquitto_pub -h oz.andrew.cmu.edu -t /topic/skeleton -m Person0,2.122,2.238,0.005,1.493,2.122,0.009,2.082,2.415,0.008,2.454,3.414,0.008,2.905,3.845,0.009,0.868,1.886,0.007,0.054,1.984,0.006,0.396,2.631,0.008,0.984,3.982,0.006,1.357,4.060,0.006,1.396,5.275,0.004,1.514,6.666,0.004,0.534,3.923,0.006,0.632,5.255,0.003,0.867,6.744,0.001,2.218,2.101,0.007,0.000,0.000,0.000,2.101,1.944,0.010,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,1.964,6.823,0.004,1.846,6.882,0.003,1.435,6.823,0.003,on
```

It interprets these according to OpenPose skeleton data format in terms of body parts; Head, Arms, Legs, etc. and IF coordinates are all zeroes (happens a lot with OpenPose data!) ... does NOT draw the particular body limb, but DOES draw the other body limbs. It tries to keep track of multiple people from OpenPose e.g. "Person0", "Person1"... in order to not overlap or multiplex the 'same' skeleton multiple times. It tries to re-use A-Frame "thickline" objects that have already been defined, updating their coordinates, rather than deleting and re-instantiating every frame, using the "on/off" feature of ARENA objects.

ToDo: make the MQTT broker and topics be specified on command line, for now they're hard coded at the top of `skel.py`

Extra .mqtt data: some very crude testing data from back when OpenPose was only 2D: skeletons are upside-down, and too big, and kind of flat. But useful for testing :)

Here's a dump of MQTT topic & message data for one person for one frame, showing the naming conventions, and showing that some of the data resulted in 'off' rather than 'on' because it contained zeroes. (sure beats drawing lines to the origin!)
```
('/topic/openpose/thickline_Person1_leftHead_0', 'thickline_Person1_leftHead_0,3.472,3.315,0.008,3.472,3.315,0.008,0,12,1,1,#280B68,on')
('/topic/openpose/thickline_Person1_leftHead_1', 'thickline_Person1_leftHead_1,3.472,3.315,0.008,3.414,3.197,0.009,0,12,1,1,#280B68,on')
('/topic/openpose/thickline_Person1_leftHead_2', 'thickline_Person1_leftHead_2,3.414,3.197,0.009,3.649,3.061,0.009,0,12,1,1,#280B68,on')
('/topic/openpose/thickline_Person1_rightHead_0', 'thickline_Person1_rightHead_0,3.472,3.315,0.008,3.472,3.315,0.008,0,12,1,1,#740C66,off')
('/topic/openpose/thickline_Person1_rightHead_1', 'thickline_Person1_rightHead_1,3.472,3.315,0.008,0.000,0.000,0.000,0,12,1,1,#740C66,off')
('/topic/openpose/thickline_Person1_rightHead_2', 'thickline_Person1_rightHead_2,0.000,0.000,0.000,0.000,0.000,0.000,0,12,1,1,#740C66,off')
('/topic/openpose/thickline_Person1_torso_0', 'thickline_Person1_torso_0,3.472,3.315,0.008,3.472,3.315,0.008,0,12,1,1,#790A07,on')
('/topic/openpose/thickline_Person1_torso_1', 'thickline_Person1_torso_1,3.472,3.315,0.008,4.100,3.532,0.007,0,12,1,1,#790A07,on')
('/topic/openpose/thickline_Person1_torso_2', 'thickline_Person1_torso_2,4.100,3.532,0.007,3.825,4.727,0.007,0,12,1,1,#790A07,on')
('/topic/openpose/thickline_Person1_leftArm_0', 'thickline_Person1_leftArm_0,4.100,3.532,0.007,4.100,3.532,0.007,0,12,1,1,#429A1D,on')
('/topic/openpose/thickline_Person1_leftArm_1', 'thickline_Person1_leftArm_1,4.100,3.532,0.007,4.178,3.533,0.008,0,12,1,1,#429A1D,on')
('/topic/openpose/thickline_Person1_leftArm_2', 'thickline_Person1_leftArm_2,4.178,3.533,0.008,4.315,4.451,0.009,0,12,1,1,#429A1D,on')
('/topic/openpose/thickline_Person1_leftArm_3', 'thickline_Person1_leftArm_3,4.315,4.451,0.009,3.552,4.275,0.009,0,12,1,1,#429A1D,on')
('/topic/openpose/thickline_Person1_rightArm_0', 'thickline_Person1_rightArm_0,4.100,3.532,0.007,4.100,3.532,0.007,0,12,1,1,#818525,on')
('/topic/openpose/thickline_Person1_rightArm_1', 'thickline_Person1_rightArm_1,4.100,3.532,0.007,3.983,3.531,0.007,0,12,1,1,#818525,on')
('/topic/openpose/thickline_Person1_rightArm_2', 'thickline_Person1_rightArm_2,3.983,3.531,0.007,3.982,4.295,0.002,0,12,1,1,#818525,on')
('/topic/openpose/thickline_Person1_rightArm_3', 'thickline_Person1_rightArm_3,3.982,4.295,0.002,3.355,4.374,0.003,0,12,1,1,#818525,on')
('/topic/openpose/thickline_Person1_leftLeg_0', 'thickline_Person1_leftLeg_0,3.825,4.727,0.007,3.825,4.727,0.007,0,12,1,1,#113887,on')
('/topic/openpose/thickline_Person1_leftLeg_1', 'thickline_Person1_leftLeg_1,3.825,4.727,0.007,3.942,4.785,0.007,0,12,1,1,#113887,on')
('/topic/openpose/thickline_Person1_leftLeg_2', 'thickline_Person1_leftLeg_2,3.942,4.785,0.007,2.748,5.510,0.007,0,12,1,1,#113887,on')
('/topic/openpose/thickline_Person1_leftLeg_3', 'thickline_Person1_leftLeg_3,2.748,5.510,0.007,3.942,5.824,0.008,0,12,1,1,#113887,on')
('/topic/openpose/thickline_Person1_rightLeg_0', 'thickline_Person1_rightLeg_0,3.825,4.727,0.007,3.825,4.727,0.007,0,12,1,1,#1B9E92,on')
('/topic/openpose/thickline_Person1_rightLeg_1', 'thickline_Person1_rightLeg_1,3.825,4.727,0.007,3.669,4.629,0.007,0,12,1,1,#1B9E92,on')
('/topic/openpose/thickline_Person1_rightLeg_2', 'thickline_Person1_rightLeg_2,3.669,4.629,0.007,2.611,5.256,0.008,0,12,1,1,#1B9E92,on')
('/topic/openpose/thickline_Person1_rightLeg_3', 'thickline_Person1_rightLeg_3,2.611,5.256,0.008,3.238,5.902,0.006,0,12,1,1,#1B9E92,on')
('/topic/openpose/thickline_Person1_leftFoot_0', 'thickline_Person1_leftFoot_0,4.100,5.745,0.007,4.100,5.745,0.007,0,12,1,1,#0A0769,on')
('/topic/openpose/thickline_Person1_leftFoot_1', 'thickline_Person1_leftFoot_1,4.100,5.745,0.007,3.942,5.824,0.008,0,12,1,1,#0A0769,on')
('/topic/openpose/thickline_Person1_leftFoot_2', 'thickline_Person1_leftFoot_2,3.942,5.824,0.008,3.688,6.333,0.008,0,12,1,1,#0A0769,on')
('/topic/openpose/thickline_Person1_leftFoot_3', 'thickline_Person1_leftFoot_3,3.688,6.333,0.008,3.845,6.313,0.007,0,12,1,1,#0A0769,on')
('/topic/openpose/thickline_Person1_rightFoot_0', 'thickline_Person1_rightFoot_0,3.375,5.903,0.005,3.375,5.903,0.005,0,12,1,1,#1B9D87,on')
('/topic/openpose/thickline_Person1_rightFoot_1', 'thickline_Person1_rightFoot_1,3.375,5.903,0.005,3.238,5.902,0.006,0,12,1,1,#1B9D87,on')
('/topic/openpose/thickline_Person1_rightFoot_2', 'thickline_Person1_rightFoot_2,3.238,5.902,0.006,2.885,6.196,0.005,0,12,1,1,#1B9D87,on')
('/topic/openpose/thickline_Person1_rightFoot_3', 'thickline_Person1_rightFoot_3,2.885,6.196,0.005,2.905,6.137,0.005,0,12,1,1,#1B9D87,on')
```
