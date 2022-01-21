---
title: Manual
parent: English
nav_order: 1
---





## Settings on the app

In addition to the procedure described up to the previous section, you need to adjust some parameters on the app.

### Move viewport

As a prerequisite for subsequent operations, use the WASD key and mouse drag to move the viewport on MocapForAll.  
Screen maximization can be turned on and off with F11 key.  

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/App-wasd.png" alt="App-wasd" style="zoom:50%;" />　

### Adjust the scales

![Settings-Coordinates-Scales]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-Coordinates-Scales.png) 

Click "Start capture" at the top of the window and let the cameras to see your whole body to capture your motion.
By observing the captured result, adjust the scales as shown below to animate the character correctly.   

If you are using Virtual Motion Tracker and SteamVR, the scales will be adjusted in [Align coordinates of SteamVR and MocapForAll](#align-coordinates-of-steamvr-and-mocapforall) section described later, so you can skip this section.

The reason why the scale is divided between the upper body and the lower body is that the two-dimensional characters generally have longer legs than the real humans. (Therefore, the ratio of the upper body scale to the lower body scale depends on the character you want to apply.)

#### Lower body scale

Affects to pelvis, hips, knees, feet, as well as the root position of the whole movement.

- If the character is always crunching, increase the value at "Settings > Coordinates > Scale > Lower body".
- If the character is floating in the air, decrease the value at "Settings > Coordinates > Scale > Lower body".

#### Upper body scale

Affects to chest, neck, head, shoulders, elbows, hands, fingers.

- If the character's shoulder is always facing down,  increase the value at "Settings > Coordinates > Scale > Upper body".
- If the character's shoulder is always facing up,  decrease the value at "Settings > Coordinates > Scale > Upper body".

### Adjust animation post process

![Settings-AnimationPostProcess]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-AnimationPostProcess.png) 

The captured motion will be displayed on the screen after the following post processes.  

#### Whether to force feet grounded

There is a function to control the positions of the feet so that they do not float in the air or sink into the floor.

- If the **contact between feet and ground** is important to you, turn **on** "Settings > Animation Post Process > Force feet grouded".
- If the **natural movement of the whole leg** is important to you, turn **off** "Settings > Animation Post Process > Force feet grouded".  

Note that data sent when "Settings > Data export > VMT protocol > Send tracking points" turned on is one **before** applying the above adjustment. (In other words, **if you use with VMT and SteamVR, you do not need to care about this setting.**)

The threshold of the distance between the foot and the ground to determine whether to apply this adjustment is automatically calculated using the value "Settings > Coordinates > Scale > Lower body".  
If [Preparation for camera calibration 2: Measure the marker size](#preparation-for-camera-calibration-2-measure-the-marker-size) was not done properly and the scale of "Lower body" was too big, the character's feet may not be able to get off the ground at all. In that case, redo [Preparation for camera calibration 2: Measure the marker size](#preparation-for-camera-calibration-2-measure-the-marker-size) and [Calibrate extrinsic parameters](#calibrate-extrinsic-parameters), or turn off "Force feet grounded".  

#### Adjust the smoothing

There is a function to smooth the captured motion.  
After applying this smoothing, data is exported externally.  

##### Turn on/off the smoothing

If the application that receive the captured motion performs its own smoothing, you may want to turn off the smoothing on the MocapForAll.
You can do it by turning off "Settings > Animation Post Process > Smoothing on body", "Smoothing on finger" and "Smoothing on facial expression".

##### Adjust the smoothing intensity
You can change the intensity of the smoothing.
###### How it works
[One euro filter](http://cristal.univ-lille.fr/~casiez/1euro/) is used for the smoothing. One euro filter is simply "low-pass filter whose cutoff frequency increases in proportion to speed". Normal low-pass filter cannot keep up with quick movements when trying to suppress jitter. On the other hand, one euro filter can **loosen noise suppression only for quick movements** to ensure tracking for them, as well as suppressing noise at low speeds in the same way as normal low-pass filter.  

You can adjust the following 3 parameters:

- fc0: Cutoff frequency at speed is zero. **The smaller this value, the less noise at low speeds.**
- Beta: How much the cutoff frequency increases in proportion to the speed. **The higher this value, the quicker movement can be tracked.**
- fcv: Cutoff frequency for speed. For speed, a normal lowpass filter with the cutoff frequency specified by this value is applied.

###### How to adjust the smoothing intensity
- Start the capture and observe the noise when standing upright without moving. If you want less noise, decrease the value of fc0.  

- Next, move your body at a speed which is appropriate for your purpose. If the captured motion cannot keep up with your body, increase the value of Beta.

### Performance optimization

Setup to reduce the CPU / GPU usage according to the environment and usage.

![Settings-General+Performance]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-General+Performance.png)  

#### Check the frame rate

- By turning on "Settings > Performance > Show frame rate", current frame rate will be displayed at the bottom of the window.

#### Use GPU rather than CPU

MocapForAll uses AI to estimate a person's posture. AI calculations are often faster on the GPU than on the CPU.
From "Settings > General > Run DNN on", you can choose whether to use CPU or GPU. If you have multiple GPUs, you can also choose which GPU to use.

When using a GPU, you have two options. TensorRT is difficult to install, takes a long time to load, and has little reward, so it is recommended to use "**GPU_DriectML**" in many cases.

- GPU_DriectML
  DirectML provides GPU acceleration on DirectX 12 capable GPUs. Examples of compatible hardware include:
  - NVIDIA Kepler (GTX 600 series) and above
  - AMD GCN 1st Gen (Radeon HD 7000 series) and above
- GPU_TensorRT
  TensorRT provides GPU acceleration on supported NVIDIA GPUs.
  To use this, you need to install CUDA, cuDNN, TensorRT, and "Appendix4_TensorRT_mode". See [Installation of TensorRT](#installation-of-tensorrt).

#### Speed mode

From "Settings > General > Capture body", you can choose what to prioritize.

- In many cases, "**Speed**" is recommended.
- "Speed+" is recommended when using a PC with low performance such as a laptop PC or when using it together with a very heavy VR application.  
- Use "Precision" mode when you want to capture motion precisely such as for movie production. You need to install "[Appendix1_Precision_mode](#appendix-optional)".

#### Reduce drawing

With the following settings, you can reduce the drawing on MocapForAll and reduce the CPU / GPU usage.

- Select "**Empty**" character in "Settings > General > Character".
- Select "**Minimum**" map in "Settings > General > Map".
- Set small value (30% for example) and turn **on** "Settings > Performance > Set screen percentage to",
  or turn **off** "Settings > Performance > Render the viewport"

#### Set the upper limit of frame rate

- Set the target frame rate (30FPS for example) and turn **on** "Settings > Performance > Limit framerate to:".  
    MocapForAll runs up to this frame rate.



# How to use 2: Export captured motion

## Use in SteamVR via VMT
You can use captured motion as virtual trackers in SteamVR by using "[Virtual Motion Tracker](https://gpsnmeajp.github.io/VirtualMotionTrackerDocument/)" created by gpsnmeajp.    

Please note that Virtual Motion Tracker is a separated program from MocapForAll. DO NOT contact the author of Virtual Motion Tracker for any issues you encountered when using MocapForAll and Virtual Motion Tracker together.

### Install Virtual Motion Tracker

- Follow the [setup procedure](https://gpsnmeajp.github.io/VirtualMotionTrackerDocument/setup/) of Virtual Motion Tracker.  
  - If you [align automatically](#align-automatically) the [coordinates of SteamVR and MocapForAll](#align-coordinates-of-steamvr-and-mocapforall), you need to install modified version of Virtual Motion Tracker for MocapForAll. [Download it from here](https://github.com/KenjiAsaba/VirtualMotionTracker/releases), and set it up in the same way above. Also, you need to use MocapForAll v1.10 or above.
- Some application requires [settings of controller binding](https://gpsnmeajp.github.io/VirtualMotionTrackerDocument/advanced/#how-to-set-the-controller-bainding).

### MocapForAll's settings to send to VMT

![Settings-DataExport-VMT-SteamVR]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-DataExport-VMT-SteamVR.png)  

- Set "Settings > Data export > Destination IP address for VMT and VMC" as follows:
  - If the destination is the same PC: "127.0.0.1"
  - If the destination is another PC: IP address of the destination PC
    - You can check it by typing "ipconfig / all" in the command prompt
    - By running MocapForAll and SteamVR on different PCs, you can offload the mocap calculation and play VR games comfortably.
- Turn on "Settings > Data export > VMT protocol > Send tracking points"
- Set the port of "Settings> Data export> VMT protocol> Send tracking points" to "39570".
- Turn on the required parts under "Settings > Data export > VMT protocol > Send tracking points > Tracking points to be sent".
  - For example, if you want to use it in VRChat, turn on "Pelvis" and "Feet"
  - You can adjust the offset of the virtual tracker position by clicking the ">" on the left side of each.

For the following settings, please decide on/off by your preference. We recommend that you try turning on first, and then try turning off if you are really serious about the motion.

- "Settings > Data export > VMT protocol > Send tracking points > As relative position to HMD"

  - When turned **off**, the actual tracking positions will be used as the positions of the virtual trackers.  
    - The delay of the movements of the virtual trackers with respect to the head-mounted display will be **visible as it is**.  
      (For example, when you start walking, it looks like only the head moves first and the rest moves later.)
    - You **need to adjust the scale and the positions properly** in [Align coordinates of SteamVR and MocapForAll](#align-coordinates-of-steamvr-and-mocapforall) section described below.  
      (If the scale is not set properly, the horizontal position of the virtual trackers relative to the HMD shifts as you move in the room.)
  - When turned **off**, the relative positions of your head and the rest parts will be transformed to the relative positions of HMD and the virtual trackers.  
    - The delay of the movements of the virtual trackers with respect to the HMD will be **less noticeable**.  
      (For example, when you start walking, it looks like the rest part slides along with the head.)
    - You **do not need to input precise number to the scale, and do not need to input the positions** in [Align coordinates of SteamVR and MocapForAll](#align-coordinates-of-steamvr-and-mocapforall) section described below.  

### Align coordinates of SteamVR and MocapForAll

The coordinate of the HMD and controller tracked by SteamVR and the coordinate of the body positions tracked by MocapForAll do not match as they are. You need to align them manually or automatically.

#### Align manually

- Preparation
  - Click "Start capture" at the top of the MocapForAll window to start motion capture
  - Put on your head-mounted display, quit apps running on SteamVR, including SteamVR Home, and make your virtual tracker visible on the SteamVR default screen.

- Adjust the scale
  - In MocapForAll,
    - Turn on "**Head**" under "Settings > Data export > VMT protocol > Send tracking points > Tracking points to be sent"
    - Tunr **off** "Settings > Data export > VMT protocol > Send tracking points > As relative position to HMD"
  - Set the values of "Settings > Coordinates > Scale > Upper body" and "Lower body" so that the height of your eye level matches the height of the "Head" virtual tracker. "Upper body" and "Lower body" scales should basically have the same value.
  
- Adjust the orientation
  - In MocapForAll,
    - Turn on "**Feet**" under "Settings > Data export > VMT protocol > Send tracking points > Tracking points to be sent"
    - Tunr **on** "Settings > Data export > VMT protocol > Send tracking points > As relative position to HMD"
  - Move your foot back and forth and set the value of "Settings > Coordinates > Coord. rotation" so that the direction of the movement of "Foot" virtual tracker matches the direction of the actual movement of your foot.
  
- Adjust the position

  - In MocapForAll,
    - Turn on "**Feet**" under "Settings > Data export > VMT protocol > Send tracking points > Tracking points to be sent"
    - Tunr **off** "Settings > Data export > VMT protocol > Send tracking points > As relative position to HMD"
- Set the value of "Settings > Coordinates > Origin position" so that the position of your feet matches the position of the virtual tracker of "Feet".

#### Align automatically

As described in [Install Virtual Motion Tracker](#install-virtual-motion-tracker) section, you need to install [the modified version of Virtual Motion Tracker for MocapForAll](https://github.com/KenjiAsaba/VirtualMotionTracker/releases). Also, you need to use MocapForAll v1.10 or above.

- Close application which uses port 39571 including VMT Manager.
- Turn on "Settings > Data export > VMT protocol > Send tracking points"
- Put on the head-mounted display and keep SteamVR running.
- Stand in a position where HMD is tracked by SteamVR and your body is tracked by MocapForAll.
- In MocapForAll, click "Align coord. To VR" at the top of the window.
- Walk around. After a while, the value of "Settings > Coordinates" will be updated automatically, and the coordinates of SteamVR and MocapForAll will be aligned.

![Settings-DataExport-VMT-SteamVR-Align]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-DataExport-VMT-SteamVR-Align.gif)　　



## Use in other apps via VMC protocol
### Load VRM models

- Load the same VRM model with the other app linking via VMC protocol.  
  To load the VRM model, drag and drop the VRM file into the MocapForAll window when the capture stopped. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Settings-General-Character-VRM.gif" alt="Settings-General-Character-VRM" style="zoom:50%;" />　　

### Send motion data

![Settings-DataExport-VMC]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-DataExport-VMC.png)　　

- Set "Settings > Data export > Destination IP address for VMT and VMC" as follows:
  - If the destination is the same PC: "127.0.0.1"
  - If the destination is another PC: IP address of the destination PC
    - You can check it by typing "ipconfig / all" in the command prompt
- Turn on "Settings > Data export > VMC protocol > Send bones""
- Set the port of "Settings> Data export> VMC protocol> Send tracking points" according to the destination port.

If you want to send as trackers (/VMC/Ext/Tra/Pos), not bones (/VMC/Ext/Bone/Pos), set the following:  
(For example, this setting is required to send data to VirtualMotionCapture.)

- Turn on "Settings > Data export > VMC protocol > Send bones > As trackers (/VMC/Ext/Tra/Pos)"

### Receive and send facial expression morphs

To receive facial expression morph data, set as follows:

- Turn on "Settings > Data export > VMC protocol > Receive facial morphs"
- Set the port of "Settings > Data export > VMC protocol > Receive facial morphs" according to the destination port specified by the source app.
- Set "Settings > Data export > VMC protocol > Receive facial morphs > From other device" as follows:
  - If the source app is running on the same PC: Off
  - If the source app is running on other device: On

The received facial expression morph data will be sent as it is if [Send motion data](#send-motion-data) is set.

## Save as BVH files
### Bone structure is based on VRM

After retargeting the animation to the VRM model on MocapForAll, the animation is exported in BVH format.  
Therefore, when exporting data in BVH format, it automatically switches to the mode that uses VRM.  

If you do not own VRM models, please select "VRM runtime load" in "Settings > General > Character" without actually loading a model.  
A dummy VRM model runs in background.  

### How to save as BVH files

![Settings-DataExport-BVH]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-DataExport-BVH.gif)

- Turn on "Settings > Data export > Record to BVH file"
  - "Settings  > General > Character" will be automatically set to "VRM runtime load"
- Click "Start capture" to start saving the data in BVH format.
- Click "Stop capture" to generate the final BVH file.
  - In the case of the free trial version, a BVH file will be automatically generated after 300 frames have passed, and data saving will be stopped.

### The unit of length is meters

The unit of bone length in the generated BVH file is meters. Set the appropriate unit in the application which load the BVH file.

## Use in UE4, UE5, Unity
### Direct link plugins

Plugins for linking data directly to UE4, UE5, or Unity are available [here](https://booth.pm/ja/items/3026430). 

- These plugin uses VMT protocol for the communication, though they do not need Virtual Motion Tracker itself and SteamVR. 

See the followings for how to use:

- [How to use data receiving plugin for UE4](https://github.com/Akiya-Research-Institute/MocapForAll-Receiver-Plugin-for-UE4/wiki)
- [How to use data receiving plugin for UE5](https://github.com/Akiya-Research-Institute/MocapForAll-Receiver-Plugin-for-UE5/wiki)
- For Unity：  Set as follows in MocapForAll:
  - Set "Settings > Data export > Destination IP address for VMT and VMC" as follows
    - If the destination is the same PC: "127.0.0.1"
    - If the destination is another PC: IP address of the destination PC
      - You can check it by typing "ipconfig / all" in the command prompt
  - Set one of the followings:
    - Turn on "Settings > Data export > VMT protocol > Send bones" and set the port according to the destination app
    - Turn on "Settings > Data export > VMT protocol > Send tracking points" and set the port according to the destination app

### Link by VMC4UE or EVMC4U

- As described in [Use in other apps via VMC protocol](#use-in-other-apps-via-vmc-protocol-1) section, set MocapForAll settings.
- VMC4UE
  - As described in "UE4 の使い方" of https://github.com/HAL9HARUKU/VMC4UE/wiki, set up UE4 and click "Play" on UE4 editor. (You do not need last  "VirtualMotionCapture の使い方" section.)
- EVMC4U
  - Execute "0. Open Unity project" to "4. Let's play" of https://github.com/gpsnmeajp/EasyVirtualMotionCaptureForUnity/wiki/How-to-use#externalreceiverpack-easy



## Export to shared memory

You can export bone transform and facial expression raw data to shared memory.

### Turn on writing to shared memory

- Enable "Settings > Data export > Shared Memory > Export to shared memory"

### Change the name of destination shared memory

- Data will be written to the shared memory of the name specified in "Settings > Data export > Shared Memory > Names of shared memories"

### Format of output data

- The first 1 byte is a counter that is incremented by 1 each time the data is updated (after 255, it returns to 0).
- The next 4 bytes are the int type value which describes the length of the remaining data in bytes.
- The rest are array of 4-byte float values.

#### Body

![DataFormat-SharedMemory-Body]({{ site.url }}{{ site.baseurl }}/assets/images/DataFormat-SharedMemory-Body.png)

#### Hand

The orientation and position of 15 bones are exported in the similar format as the Body data. The arrangement of bones is as follows.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/DataFormat-SharedMemory-Hand.png" alt="DataFormat-SharedMemory-Hand" style="zoom: 33%;" /> 

#### Face

Only positions (no rotations) are exported for 468 face landmarks. The position is represented by a three-dimensional value with the XY coordinates in pixels in the camera image cropped to the area of the face and the Z coordinate of the same scale in the depth direction.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/DataFormat-SharedMemory-Face1.png" alt="DataFormat-SharedMemory-Face1" style="zoom: 65%;" /> 

Enlarge the image below to see where each landmark corresponds to your face.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/DataFormat-SharedMemory-Face.jpg" alt="DataFormat-SharedMemory-Face" style="zoom: 10%;" /> 

#### Eye

In the same format as the Face data, the positions of 5 points for the iris and 71 points for the eyelid are exported.

### Example for getting the value from shared memory

Click [here](https://github.com/Akiya-Research-Institute/MocapForAll-SharedMemory-Plugin-for-Unity) for a sample project to read the transform data from shared memory with Unity and apply it to GameObjects.



# How to use 3: Other settings
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


# FAQ
## The camera does not work

- Select UE4 media player in [Select camera control framework](#select-camera-control-framework)
  - Try selecting different formats in "Image size"
- Select Direct Show in Select camera control framework
  - Select proper image size based on the camera specification

If none of these work, unfortunately MocapForAll will not be able to use the camera.

## Multiple cameras of the same model are not recognized
When connecting multiple cameras of the same model to a PC, there is a problem that some of them are not displayed (not detected) in the pull-down of camera selection in the app. In that case, try setting an arbitrary FriendlyName from the registry editor to the camera device that is not displayed.  
The method of setting / changing the FriendlyName differs depending on your environment, so please google it yourself. Since you are editing the registry, it is recommended to make a backup.  

## Virtual camera is not recognized

The default virtual camera in OBS does not work. Install [OBS-VirtualCam plugin](https://obsproject.com/forum/resources/obs-virtualcam.539/) and select Direct Show in [camera control framework](#select-camera-control-framework).

## Can I use a black-white camera?
No, you can't use a monochrome camera with this app. Please use an RGB camera. 
## AR marker is not recognized

- Make sure the camera image is not a mirror image.
- Make sure the camera image is in focus on the AR marker.
- Make sure the AR marker is large enough in the camera image.
  - Printing a larger size of the AR marker may help
  - For camera calibration of extrinsic parameters, see [If the marker cannot be read properly](#if-the-marker-cannot-be-read-properly) section.

## Camera calibration of extrinsic parameters does not work

See [If the marker cannot be read properly](#if-the-marker-cannot-be-read-properly) section.

## It does not capture even after camera calibration
- At the start of capture, only a person standing upright can be recognized. So, rotate the image appropriately according to the actual orientation of the camera.
- Make sure that two or more cameras that have been calibrated can see your whole body.
- It will not work if the final frame rate is below 4 fps. [Check the frame rate](#check-the-frame-rate).

## Does it support capturing multi-person?
No, currently only one person can be captured. Therefore, only one person should be shown on the camera.  
## VMT doesn't work

Check errors in VMT Manager. Especially, check if [Room Matrix](https://gpsnmeajp.github.io/VirtualMotionTrackerDocument/trouble/#room-matrix-is-not-set) is set.

## The motion of Virtual trackers are wrong

Make sure that the [alignment of coordinates between SteamVR and MocapForAll](#align-coordinates-of-steamvr-and-mocapforall) is correct.

## There is no serial number input field
Please install the latest MocapForAll. From version 1.0 and above, you do not need to input the HMD serial number.  
## Difference between MocapForAll and others

- [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/advanced/3d_reconstruction_module.md)： The basic idea is similar, but the licenses are very different. Basically, you can use OpenPose only for "ACADEMIC OR NON-PROFIT ORGANIZATION NONCOMMERCIAL RESEARCH USE ONLY"

  

# Known issues
## TensorRT mode does not work on RTX30xx series

Use DirectML mode.



# Acknowledgements

We express our sincere thanks to Gonzales kévin, "Raybox41130" for the French translation.
