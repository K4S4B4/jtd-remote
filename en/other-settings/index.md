---
title: Other settings
parent: English
has_children: true
nav_order: 5
---

# Other settings

## Capture hand and face
- Turn on "Settings > General > Capture hand" to capture finger movements
- Turn on "Settings > General > Capture face" to capture facial expressions

Note that these are experimental features. The accuracy is low and CPU / GPU usage is high.

### Cropping size for hand / face

You can adjust the crop size of the image used to capture your hands and face from "Settings > Advanced > Cropping size for hand" and "Cropping size for face". Since different people have different hand and face sizes, adjusting these values may improve accuracy.

### Hand capture Full / Lite

You can select the hand tracking model from Full, Lite or Legacy from the pulldown "Settings > General > Capture hand".

- "Full": More accurate. Uses more CPU/GPU.
- "Lite": Less accurate. Uses less CPU/GPU.
- "Legacy": Uses the model used in v1.13 and before. For backward compatibility.

### Specify facial morph target names

You can specify the names of facial morph targets if you are using VRM models.

- Turn on "Settings > Advanced > Specify facial morph target names"
- Input morph target names



## Lock-on to the character

- Turn on "Settings > General > Look at character" to make the view follow the movement of the character.



## Display the tracking positions

- Turn on "Settings > General > Draw tracking points" to show the yellowish cubes to display the tracking positions.



## Language

- You can change the language from the earth icon on the upper right corner of the window. Currently, Japanese, English, and French are supported.



## Reset all the settings

Delete "C:\Users\\[User name]\AppData\Local\MocapForAll" to reset all the settings

If MocapForAll does not launch for some reason, resetting the settings may solve the problem.


## Motion capture from recorded videos

From v1.12, you can capture motion using recorded videos.

### How to use

- Select "Recorded video" from the "▼" next to "Add camera" at the top of the MocapForAll window.   
- Press the "..." button and select the video file.  
- After that, the usage is basically the same as a normal webcam.

### Playback positions

- The video will automatically loop.
- The playback position of the video will return to the beginning of the video when you press "Start Capture".

### Tips

- It is recommended to separate the video files for intrinsic parameter calibration, extrinsic parameter calibration, and actual motion capture.
- There is no function to analyze the videos and synchronize the movements between them, so it is necessary to edit the videos in advance so that the movements will be synchronized when they are played at the same time from the beginning. When recording videos, it is recommended to clarify the starting point with a clapperboard or the sound of clapping your hands.



## Execute some actions at startup by command line arguments

From v1.13, you can execute a specific operation at startup by specifying the following phrase in the arguments. 

- "StartCapture":  Execute the same process as "Start Capture" button is pressed immediately after startup.
- "LoadAllCameras": Execute the same process as "Load All Cameras" button is pressed immediately after startup.

Example:

![App-Commandline-Args]({{ site.url }}{{ site.baseurl }}/assets/images/App-Commandline-Args.png)  


## Apply motion only to the upper body

With the following settings, the motion will be applied only to the upper body of the character.
- Turn off "Settings > General > Capture body > Apply to lower body".

The lower body stops at the position of the last frame, so adjust the relative position of the upper and lower body with "Settings> Coordinates" if necessary.