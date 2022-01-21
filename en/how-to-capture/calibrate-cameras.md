---
title: Calibrate cameras
parent: How to capture motion
grand_parent: English
nav_order: 2
---

# Calibrate cameras

## Contents
{: .no_toc }

## What is camera calibration?
Camera calibration is the process of obtaining information about the relationship between the positions in the camera image and the positions in the real world. This section describes the concept of what you are doing with camera calibration and some tips for it.

In camera calibration in MocapForAll, the following 2 types of information are obtained:

- Intrinsic parameters (Characteristics of the camera itself)
- Extrinsic parameters (Position of the camera in the real world)

### What is intrinsic parameters?
Intrinsic parameters are the focal length *f* of the lens and the position of the optical axis *Cx, Cy*, which describes the **characteristics of the camera itself**.  
These are unique to the camera (lens) and basically do not change. Therefore, **once you get them correctly, you don't need to get them again.**  
Mathematically, it is represented by a matrix that describes the relationship between "positions in the camera image" and "positions in the camera coordinate". This is the matrix displayed on the app screen.  
![Camera-Calibration-Intrinsic-Equation]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Intrinsic-Equation.png)  

![Camera-Calibration-Intrinsic-Geometry]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Intrinsic-Geometry.png)  

#### Notes on autofocus
If you use a camera with autofocus, keep in mind that as the focus changes, the focal length changes, so the intrinsic parameter also changes.  
In our experiences, it does not cause much problems when using regular webcams or mobile phones, but if high accuracy is not obtained, you may be need to disable autofocus.  

### What is extrinsic parameters?
Extrinsic parameters are **the position and the orientation of the camera in the real world**.  
In theory, once the camera is completely fixed in your room, it can be treated as a fixed value. But in practice, it sometimes shifts little by little over time. So, **it is recommended to obtain it again every time you start using MocapForAll**.   
Mathematically, it is represented by a matrix that describes the relationship between "positions in the camera coordinate" and "positions in the world coordinate". This is the matrix displayed on the app screen.

![Camera-Calibration-Extrinsic-Equation]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Equation.png)  

![Camera-Calibration-Extrinsic-Geometry]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Geometry.png)  

#### 4 methods to get extrinsic parameters
In MocapForAll, there are 4 methods to get extrinsic parameters. Please note that the preparation and execution procedures are different for each.

|  Method  |  Accuracy  |  Ease of preparation  |  The size of the usable space  | 　Comment  |
| ---- | ---- | ---- | ---- | ---- |
| 1. Method using ChArUco board |  👍👍👍  |  👍  |  👍  | The most accurate, but requires a little work to prepare. **I think this is the easiest way in the long run.** |
|  2. Method using ArUco cluster  |  👍👍  |  👍👍  |  👍👍  | This is a method with good accuracy and easy preparation. **For beginners, I recommend that you try this method first.** |
| 3. Method using Diamond cluster |  👍👍👍  |  💀  |  👍👍👍  | This method allows capturing in a large space with many cameras by measuring the relative positions of multiple markers, though It takes time to prepare. |
|  4. Method using human motion  |  👍  |  👍👍👍  |  👍👍  | This method allows capturing in an environment where the marker cannot be printed or placed (for example, outdoors). |



## Preparation for camera calibration 1: Print AR markers
In order to calibrate a camera, it is necessary to "find the positions of points in the camera image whose positions in the real world are known". To do that, you need to take pictures of the "specific images" described below with the camera that actually you are using, so that the application can calculate the before-mentioned camera information from the pictures.  

In this section, we will print the "specific images" in preparation for camera calibration.

### Print the image for intrinsic parameter calibration
We will use [this image](https://raw.githubusercontent.com/Akiya-Research-Institute/MocapForAll-Wiki/main/resources/calibration/IntrinsicCalibration.png).  
If you haven't fixed the camera in the room yet, you don't need to print it out. We will use the above image by showing on the PC display.  
If you have already fixed the camera, print the above image in A4 size. The size does not have to be exact. Then tape it to a cardboard box to keep it flat.  
![Camera-Calibration-Intrinsic-Marker]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Intrinsic-Marker.jpg)  

### Print the image for extrinsic parameter calibration
The image differs depending on the [4 methods to get extrinsic parameters](#4-methods-to-get-extrinsic-parameters) explained before.

Either way, the size does not have to be exact.

1. Method using ChArUco board  
    We will use [this image](https://raw.githubusercontent.com/Akiya-Research-Institute/MocapForAll-Wiki/main/resources/calibration/ExtrinsicCalibration.png). Print this in **A2 or larger**.  
    You don't have a printer which can print A2? (Me too) Then, it is recommended to divide the image into two pieces, print them on two sheets of A3 paper, and tape them together.  
    ![Camera-Calibration-Extrinsic-Marker-Charuco]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Marker-Charuco.jpg)  
    If you tape it on a cardboard like this so that it keeps in a clean flat state, you can continue to use it for a long time.  
    （The one in the photo above has been used for about 3 months, but it is still good to use.)
2. Method using ArUco cluster  
    We will use "arucoMarker0.png", "(same)1.png" and "(same)2.png" in [this zip](https://github.com/Akiya-Research-Institute/MocapForAll-Wiki/raw/main/resources/calibration/ArucoMarkers.zip). Print them in A4 or A3. A4 is enough for a room that is not very large.   
    ![Camera-Calibration-Extrinsic-Marker-Aruco]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Marker-Aruco.jpg)  
3. Method using Diamond cluster  
    We will use "diamondMarker0.png" and others in [this zip](https://github.com/Akiya-Research-Institute/MocapForAll-Wiki/raw/main/resources/calibration/DiamondMarkers.zip). Print them in A2 or larger in the same way as "1. Method using ChArUco board".  
    ![Camera-Calibration-Extrinsic-Marker-Diamond]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Marker-Diamond.jpg)
5. Method using human motion  
    There is nothing to print.



## Preparation for camera calibration 2: Measure the marker size
You need to measure the size of the actual printed image for extrinsic parameter calibration. The measured values will be used to define the scale of the captured movement.  
The part to measure differs depending on the [4 methods to get extrinsic parameters](#4-methods-to-get-extrinsic-parameters) explained before.

1. Method using ChArUco board  
![Which_side_to_measure_-_ChArUco_board]({{ site.url }}{{ site.baseurl }}/assets/images/Which_side_to_measure_-_ChArUco_board_(default).png)   
Put the value in "Settings > Calibration > Maker size (affects to coord. scale) > **ChArUco board** [m]". The unit is meters.  
![Which_side_to_measure_-_ChArUco_board]({{ site.url }}{{ site.baseurl }}/assets/images/Settings-Calibration.png)   

2. Method using ArUco cluster    
![Which_side_to_measure_-_ArUco_marker]({{ site.url }}{{ site.baseurl }}/assets/images/Which_side_to_measure_-_ArUco_marker.png)  
Put the value in "Settings > Calibration > Maker size (affects to coord. scale) > **ArUco marker** [m]". The unit is meters.

3. Method using Diamond cluster  
![Which_side_to_measure_-_Diamond_marker]({{ site.url }}{{ site.baseurl }}/assets/images/Which_side_to_measure_-_Diamond_marker.png)  
Put the value in "Settings > Calibration > Maker size (affects to coord. scale) > **Diamond marker** [m]". The unit is meters.

4. Method using human motion  
Measure the height of your body. Put the value in "Settings > Calibration > Maker size (affects to coord. scale) > **Human hight** [m]". The unit is meters.


## Connect cameras
Connect at least 2 cameras to your PC.  
Click "Add camera" button at the top of the MocapForAll window.   
Select the combo box next to "Camera:" to find the connected camera.   
 ![Camera-Add]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Add.gif)

You can change the image size of the camera by entering the dimensions and clicking "Apply" if camera supports the specified image size.
Somtimes it fails to change the image size. In that case, please close camera, wait for a moment, and try again.

You can flip the image horizontally. Note that some cameras have a mirror image by default. If the image is a mirror image, the AR marker cannot be read.  

You can rotate the image. At the start of capture, only a person standing upright can be recognized. So, rotate the image appropriately according to the actual orientation of the camera.


### Select camera control framework
You can select the framework to control the camera by pressing the "▼" next to "Add camera" at the top of the MocapForAll window.    

 ![Camera-Add-Framework]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Add-Framework.png)

- **Direct show:** Microsoft's media framework. You can use the OBS-VirtualCam plugin with this.
- **UE4 media player:** UE4's media framework. Better performance at high resolution. Some cameras don't work with this.
- **Recorded video**: Use recorded video instead of camera. See [Motion capture from recorded videos](#motion-capture-from-recorded-videos).

If the camera works with UE4 media player, it is recommended to use it. If it doesn't work, use Direct Show.


## Calibrate intrinsic parameters
If you have not printed the image in [Print the image for intrinsic parameter calibration](#print-the-image-for-intrinsic-parameter-calibration) section, display [this image](https://raw.githubusercontent.com/Akiya-Research-Institute/MocapForAll-Wiki/main/resources/calibration/IntrinsicCalibration.png) on your PC's screen as large as possible.

In MocapForAll, click "Start" button under "Camera > Calibration > Intrinsic".   
Take images with the camera from various angles for about 10 seconds. If the camera is already fixed, move the image instead of the camera.  
When calibration is completed, intrinsic parameters will be displayed on the app's screen and "Intrinsic  ☑Calibrated" will be shown.  

![Camera-Calibration-Intrinsic-Execution]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Intrinsic-Execution.gif)　

Save the camera calibration result from "Save".

## Calibrate extrinsic parameters

The procedure differs depending on the [4 methods to get extrinsic parameters](#4-methods-to-get-extrinsic-parameters) explained before.

1. Method using ChArUco board  
    Place the image printed in [Print the image for extrinsic parameter calibration](#print-the-image-for-extrinsic-parameter-calibration) section on the floor. This will be the origin of the captured motion.  
    Place the cameras where they can see the placed image.  
  
    In MocapForAll, select "**ChArUco board (default)**" in "Settings > Calibration > Extrinsic calibration method".  
    Click "Start" button under "Camera > Calibration > Extrinsic".  When calibration is completed, extrinsic parameters and the camera position will be displayed, and it will shows "Extrinsic  ☑Calibrated".  
  
    If it does not read the AR marker properly, try [If the marker cannot be read properly](#if-the-marker-cannot-be-read-properly).

    ![Camera-Calibration-Extrinsic-Execution-Charuco]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Execution-Charuco.gif)
    
    
    
2. Method using ArUco cluster   
   
    Place the images printed in [Print the image for extrinsic parameter calibration](#print-the-image-for-extrinsic-parameter-calibration) section on the floor. "arucoMarker0.png" will be the origin of the captured motion.  
    Place the cameras where they can see the placed images.  
    
    In MocapForAll, select "**ArUco cluster**" in "Settings > Calibration > Extrinsic calibration method".  
    Click "**Scan markers**" button under "Camera > Calibration > Extrinsic".  This will scan the relative positions of the markers. After a while, the markers will appear in the 3D viewport.
    
    After all markers appeared, click "**Stop scanning**" and then "**Start**" under "Camera > Calibration > Extrinsic".  When calibration is completed, extrinsic parameters and the camera position will be displayed, and it will shows "Extrinsic  ☑Calibrated".  
    
    If it does not read the AR marker properly, try [If the marker cannot be read properly](#if-the-marker-cannot-be-read-properly).
    
    ![Camera-Calibration-Extrinsic-Execution-Aruco]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Execution-Aruco.gif)
    
    
    
3. Method using Diamond cluster 

    Place the images printed in [Print the image for extrinsic parameter calibration](#print-the-image-for-extrinsic-parameter-calibration) section at a distance that fits in a frame of the camera. It does not have to be in the same plane. "diamondMarker0.png" will be the origin of the captured motion, so place this on the floor.  

    In MocapForAll, select "**Diamond cluster**" in "Settings > Calibration > Extrinsic calibration method".  
    Click "**Scan markers**" button under "Camera > Calibration > Extrinsic" in one of the cameras. Take pictures of "diamondMarker0.png" and one of the markers at the same time. After a while, the position of another marker will be fixed with "diamondMarker0.png" as the origin, and the marker will be displayed on the 3D viewport. 

    Repeat the above step for markers whose positions have been fixed and markers whose positions have not been fixed, and click "Stop scanning" when the positions of all markers have been fixed.

    Place the cameras where they can see at least one of the markers whose position is fixed. 

    Click "**Start**" under "Camera > Calibration > Extrinsic".  When calibration is completed, extrinsic parameters and the camera position will be displayed, and it will shows "Extrinsic  ☑Calibrated".  

    ![Camera-Calibration-Extrinsic-Execution-Diamond]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Execution-Diamond.gif)



4. Method using human motion 
    Place the cameras so that at least two of them can see your whole body at the same time.

    In MocapForAll, select "**Human motion**" in "Settings > Calibration > Extrinsic calibration method".  
    Select "**GPU_DirectML**" in "Settings > General > Run DNN on" if your hardware support it. (If you have already installed the "GPU_TensorRT" mode in the Appendix, you can also use it.）  
    Select "**Speed**" in "Settings > General > Capture body" if your PC's performance is enough for it. (If you have already installed the "Precision" mode in the Appendix, you can also use it.）  

    Click **"Start calibration" button at the top of the window**, and walk around on the floor where the cameras can see your whole body.  
    Your motion will be captured and the positions of your joints will be used to find the cameras relative positions.
    
    After all cameras' extrinsic parameters calibrated, click the "**Find Ground**" button at the top of the window and walk around in the same way as [If the marker cannot be read properly](#if-the-marker-cannot-be-read-properly) described below. This will determine the absolute position of the cameras.  
    
    ![Camera-Calibration-Extrinsic-Execution-Human]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Execution-Human.gif)
    

### If the marker cannot be read properly

Since the position of the marker will be the plane with zero height of the captured motion, it is basically recommended to place the marker on the floor, but it may be difficult to read the marker on the floor depending on the placement and specification of the camera.

In that case, you can **hang the marker on the wall to calibrate the camera and adjust the floor level later**.

Procedure:
- Replace the word "on the floor" with "on the wall" and execute the calibration procedure.  
- Select "**GPU_DirectML**" in "Settings > General > Run DNN on" if your hardware support it. (If you have already installed the "GPU_TensorRT" mode in the Appendix, you can also use it.）  
  Select "**Speed**" in "Settings > General > Capture body" if your PC's performance is enough for it. (If you have already installed the "Precision" mode in the Appendix, you can also use it.）  
- Click the "**Find Ground**" button at the top of the window and walk around where at least 2 cameras can see your whole body. After a while, the position of the floor level will be adjusted automatically.  

![Camera-Calibration-Extrinsic-Execution-FindGround]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Extrinsic-Execution-FindGround.gif)

### Confirm the calibration results

If you have input the correct value in [Preparation for camera calibration 2: Measure the marker size](#preparation-for-camera-calibration-2-measure-the-marker-size), you can see the position of the camera in the 3D viewport. For how to move the viewpoint, refer to [Move viewport](#move-viewport).

Note that the camera may appear below the floor (in this case, the calibration has failed).

## Save and load the calibration results

After calibration is completed, save the calibration result of each camera by clicking "**Save**" under "Camera > Calibration > Intrinsic" and "Extrinsic". The saved result can be loaded by clicking "**Load**" button.  

Also, you can save and load the whole camera setup by clicking "**Save All Cameras**" and "**Load All Cameras**" buttons.  
Note that the camera selection is saved as an index inside the combo box. If you remove the cameras from your PC, the order of the cameras in the combo box will change and you will not be able to load the cameras properly.

![Camera-Calibration-Save]({{ site.url }}{{ site.baseurl }}/assets/images/Camera-Calibration-Save.gif)