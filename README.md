# dVPose

![rgb](readme_files/172_rgb.png)

![depth](readme_files/172_depth.png)


A sample of the dataset is currently available for download
[here](https://livejohnshopkins-my.sharepoint.com/:f:/g/personal/ngreen29_jh_edu/EqHw6Tmr4i1JhgNVFGwaJWkBQ513636EmeaKoPBhBmLp4w?e=1seJnl).
Additional will be added soon.


# Cad Models
In the cad models folder, various formats are available for the da Vinci
endoscope (the instrument models will be added soon). Also, the optical tracking
marker registrations are provided as 4x4 matrices.


# Raw Data Format
The raw data from the Atracsys_Hololens_streamer tool is structured as follows:

```
root
|
└───run1
│   │   HMD_POSE_00000.json
│   │   INST_POSE_00000.json
│   │   SPHERES_POSE_00000.json
│   │   AHAT_AB_00000.png
│   │   AHAT_DEPTH_00000.png
│   │   AHAT_00000.json
│   │   VIDEO_00000.png
│   │   VIDEO_00000.json
│   │   VLC_LF_00000.png
│   │   VLC_LF_00000.json
│   │   VLC_RF_00000.png
│   │   VLC_RF_00000.json
|   |   ...
│   
└───run2
    │   HMD_POSE_00000.json
    │   INST_POSE_00000.json
    │   SPHERES_POSE_00000.json
    │   AHAT_AB_00000.png
    │   AHAT_DEPTH_00000.png
    │   AHAT_00000.json
    │   VIDEO_00000.png
    │   VIDEO_00000.json
    │   VLC_LF_00000.png
    │   VLC_LF_00000.json
    │   VLC_RF_00000.png
    │   VLC_RF_00000.json
    |   ...   
```



The file prefixes correspond to the following things:



`HMD_POSE.json`: Contains the pose of the Atracsys marker attached to the
HoloLens, along with a timestamp.

`INST_POSE.json`: Contains the pose of the Atracsys marker attached to the
Instrument, along with a timestamp.

`SPHERES_POSE.json`: Contains a list of the 3D position of all reflective
spheres detected by the Atracsys. The position of these spheres sometimes does not seem to
perfectly correspond with the position of the marker frames themselves. For
example, the sphere positions for one frame are drawn however they are slightly
misaligned from the reflective spheres in the image here: ![Misaligned Spheres](./data_format_imgs/misaligned_spheres.png)


`AHAT_AB.png`: Contains the Depth Camera's reflectivity image

`AHAT_DEPTH.png`: Contains the Depth Camera's depth image

`AHAT.json`: Contains the 4x4 transformation matrix which describes the pose of
the HoloLens2 Depth camera relative to the HoloLens2 world origin (This matrix is returned by
the HoloLens2 during the image capture. It is not calculated), along with image metadata.

`VIDEO.png`: Contains the RGB Grayscale camera image

`VIDEO.json`: Contains the 4x4 transformation matrix which describes the pose of
the HoloLens2 RGB camera relative to the HoloLens2 world origin (This matrix is returned by
the HoloLens2 during the image capture. It is not calculated), along with image metadata.

`VLC_LF.png`: Contains the Right-Front Grayscale camera image

`VLC_LF.json`:Contains the 4x4 transformation matrix which describes the pose of
the HoloLens2 Left-Front Grayscale camera relative to the HoloLens2 world origin (as returned by
the HoloLens2), along with image metadata.

`VLC_RF.png`: Contains the Right-Front Grayscale camera image

`VLC_RF.json`: Contains the 4x4 transformation matrix which describes the pose of
the HoloLens2 Right-Front Grayscale camera relative to the HoloLens2 world origin (as returned by
the HoloLens2), along with image metadata.


## Important Note
The three atracsys readings `HMD_POSE.json`, `INST_POSE.json`, and
`SPHERES_POSE.json` are returned at once as a single data frame by the atracsys.

Each Hololens camera image's `.png` file and corresponding `.json` file together
are returned as a single data frame.

Each HoloLens camera returns asynchronously, so for example, the
`VIDEO.png/json` data frame may have occured at a different time (on the
order of seconds) than the the
`AHAT.png/json` pair, even if they have a corresponding index.

Also, there is no synchronization between the
atracsys marker readings and HoloLens images, although the atracsys update
frequency is much faster than the images

To emulate synchronicity, each frame should be collected while the objects
remain fixed with no motion