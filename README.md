# skeleton3d
Render OpenPose3D skeleton data in ARENA

To use: run e.g. `python skel.py` 

This reads from MQTT topic /topic/skeleton (host oz.andrew.cmu.edu) where each message published to this topic is a comma separated list of coordinates from OpenPose3d, prefaced by a person ID, and ending with a string ("on" or "off"), e.g
```
mosquitto_pub -h oz.andrew.cmu.edu -t /topic/skeleton -m Person0,2.122,2.238,0.005,1.493,2.122,0.009,2.082,2.415,0.008,2.454,3.414,0.008,2.905,3.845,0.009,0.868,1.886,0.007,0.054,1.984,0.006,0.396,2.631,0.008,0.984,3.982,0.006,1.357,4.060,0.006,1.396,5.275,0.004,1.514,6.666,0.004,0.534,3.923,0.006,0.632,5.255,0.003,0.867,6.744,0.001,2.218,2.101,0.007,0.000,0.000,0.000,2.101,1.944,0.010,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,0.000,1.964,6.823,0.004,1.846,6.882,0.003,1.435,6.823,0.003,on
```
