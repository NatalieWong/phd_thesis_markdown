# Application Overview

<!--
After the introductory chapter, it seems fairly common to 
include a chapter that reviews the literature and 
introduces methodology used throughout the thesis.
-->

## Application Architecture
### Hardware
Android Lantern is a portable interactive device comprised of

- a single-board computer --- Raspberry Pi 3 Model B+
- a palm-sized laser projector, and
- a Raspberry Pi camera module.

The Raspberry Pi camera module is the Computer Vision used for the implementation of interactive feature on the Information Wall.

### Software
An Android application is developed to provide the user interface and implement the interactive feature of the Interactive Information Wall.

Android 10 is installed on the Raspberry Pi to enable the deployment of the Android application. 

Kotlin and Java are the main programming languages for the Android application development. Android Studio is used as the tool for building the application.

#### User Interface
The Information Wall displays three kinds of information, which are

- News,
- Gallery, and
- Department Staff Information.

Each of the above information has its own landing page showing thumbnails of the available items of such information, and inner pages (except Gallery) showing the details of each item. There is a home page which displays three titles corresponding to the above information, allowing users to navigate in the application.

![Interactive Information Wall with Android Lantern - Home page](https://lh3.googleusercontent.com/iBDx8eS1i1CpxqKxhzJKJGr1sKch_AM_5Mvb0G721HXkvmEKhfkFuRICF_RAdmTfDPAdwoJhV7w0=s500 "Interactive Information Wall with Android Lantern - Home page") \

|  | News | Gallery | Department Staff Information |
|--|--|--|--|
| Landing page | ![LanternInfoWall-News-landing-page](https://lh3.googleusercontent.com/i7CrDAuHVSScWDzjZnheLwtrn-i09QcI0-1PWf1t3g_O1lU_mvoAIqrus6p0uRF5hRWjqXr3rXjn=s200 "LanternInfoWall-News-landing-page") | ![LanternInfoWall-Gallery-page](https://lh3.googleusercontent.com/uZCQLEMfPCjq_cg3fX96f-TWWXcnCgomiu5N0AQBLEmPome8yM5GNXQtNhCZ9V8EZ2eIiX7OJat0=s200 "LanternInfoWall-Gallery-page") | ![LanternInfoWall-staff-landing-page](https://lh3.googleusercontent.com/hIup2Q8xPCl4ckWM4nddT91vz2xYK6xUW4VO8_tWpUld2lr-Nz5cpsfrgr6gDrcy55b2P6lFFSyT=s200 "LanternInfoWall-staff-landing-page") |
| Item Details page | ![LanternInfoWall-News-item-page](https://lh3.googleusercontent.com/5ZIqnP5MH869gI_EM1h9nQYesO4zcOtOae8FJj4neu8VzVzYdlD9XKdpS-XFuIyC1aaOivXbqmMu=s200 "LanternInfoWall-News-item-page") |  | ![LanternInfoWall-staff-item-page](https://lh3.googleusercontent.com/_FU8V4agTnFd0HH33sDg47obU4nGRkSCqGg6W4lTMgoNiNNY90QWternPMwME7rAnXxzjjcwe4D4=s200 "LanternInfoWall-staff-item-page") |

#### Interactive Feature brought by Computer Vision and Machine Learning
On the Information Wall displayed by the Android Lantern, user can interact with the virtually projected graphics by "clicking" them using a hand. 

The Raspberry Pi camera module is an essential component enabling the Lantern to "see" human hands by capturing images from the real world. The technique of object detection from ML is applied to train a model which can recognize human hands in the images captured by the camera module.

The Android application contains all the programming logic to perform the "click" event and update the UI.

## Existing Methods for Object Detection
I had researched into different machine learning approaches for the implementation of hand detection in Android application.

### ML Kit's Object Detection and Tracking API
**ML Kit** is a mobile SDK developed by Google. It is one of the best tools for programmers who are new to ML compared to other ML frameworks because of its provision of the ready-to-use API. This enables faster mobile application development progress.

Bounding box surrounding a detected object in each image can be obtained through the following lines of code.
``` kotlin
for (obj in detectedObjects) {
	...
	val bounds = obj.boundingBox // returns a Rect object
	...
}
```
I can make use of the bounding box to obtain the location of a user's hand on the projected area of the Lantern so as to implement a click event allowing the user to interact with the virtually projected graphics.

#### Problems with the ML Kit's ODT model
The ready-to-use API provided by ML Kit hinders variations during on-device inference.

The existing ODT model can only classify objects into 5 coarse categories which are `FASHION_GOOD`, `FOOD`, `HOME_GOOD`, `PLACE`, `PLANT` and `UNKNOWN`. Therefore, I cannot directly apply this model for hand detection. I have to train a custom hand detection model by myself using TensorFlow Object Detection API.

\newpage

### Hand Detection Models trained using TensorFlow Object Detection API
[**Real-time Hand Detector using Neural Networks (SSD) on Tensorflow**](https://github.com/victordibia/handtracking) [@victor-hand-detection] by Victor Dibia

The hand detection model trained in this project produces excellent results in tracking hand, with an average precision of 96.86%. It is a re-trained model based on a pre-trained standard SSD MobileNet V1 model with 200,000 training steps using the technique of Transfer Learning. Standard SSD MobileNet V1 model was selected because it is one of the fastest pre-trained object detection model, with a speed of 30 milliseconds for inference. The training process was run on a cloud GPU machine which takes around 5 hours to complete it.

The hand detection process is targeted to be run on videos or personal computers with a web camera capturing video stream using a [custom Python script](https://github.com/victordibia/handtracking/blob/master/utils/detector_utils.py) in the project. Multiple hands can be detected in each frame of the video or video stream from the web camera. Bounding boxes will be drawn in each frame to visualize the detection results.

![VictorDibia-HandTrack-demo](https://lh3.googleusercontent.com/E9T-y33nTByciI60NNBlJuNJfRxrx2csj85gv_CIK9oPmmGaWX7NPlRInPr4dwDbftXU4kfeOoVs=s300 "VictorDibia-HandTrack-demo"){width=55%} ![VictorDibia-HandTrack-video-demo](https://lh3.googleusercontent.com/FfdBQBQ6__LyzcfYffHRIbM4JiMerzaA_yggnboNCs34bL_9Dom7OaWofOph5w1y7PYVORkypeWP=s250 "VictorDibia-HandTrack-video-demo"){width=45%}

The dataset being used to train this model is [EgoHands](http://vision.soic.indiana.edu/projects/egohands/) [@Bambach_2015_ICCV] prepared by Indiana University. Hands were annotated and assigned to 4 classes, `own left`, `own right`, `other left`, and `other right`. In the project, the 4 classes are merged into one class --- `hand` so there is just one output label in the detection results.

[**Hands Detection in Video Stream**](https://github.com/loicmarie/hands-detection) [@loic-hands-detection] by Loïc Marie

The hand detection model in the project was re-trained from a pre-trained Faster R-CNN model using the [Hand Dataset](http://www.robots.ox.ac.uk/~vgg/data/hands/index.html) [@oxford_hand_dataset] prepared by the University of Oxford. All hands in the images of this dataset were labeled as `hand` only. The model was trained on the Google CloudML.

From the project's demo video, hands in each frame captured by web camera were detected and bounding boxes were drawn in each frame to visualize the detection results. The average precision of this hand detection model is 99%, however, the hand detection process was a bit slow and not smooth enough.

![Loïc Marie-hands detection](https://lh3.googleusercontent.com/kyw5Vk1Ph4mAQfj8xvyop374O70pTUWVfewdgXRSwbC58m45LSvAlqgkeTp_o33etrqH_qD3i1KU=s200 "Loïc Marie-hands detection") ![Loïc Marie-hands detection-2](https://lh3.googleusercontent.com/Cs73DJ6ga0kKDgAlCi-e1nQ-3GTWSU4Wl5APFmxNNH53kJOp8Fec82C6XNAMHrj4xL4L1hnkFAlE=s200 "Loïc Marie-hands detection-2") ![Loïc Marie-hands detection-3](https://lh3.googleusercontent.com/orMS2eYMX5Wm2KHPc0UvxbRW5yTvQ4b77gh5RL_BlZYI_bVY_GwqNW9PhE8nEIx0cNdIq45GGcCb=s200 "Loïc Marie-hands detection-3")

This is a tradeoff of the Faster R-CNN model. Although the average precision of the detection results is improved, the detection speed is at least the double of a SSD MobileNet model, ranging from 58 to 1,833 milliseconds [@tf-model-rcnn].

[**Hand Tracking (GPU)**](https://github.com/google/mediapipe/blob/master/mediapipe/docs/hand_tracking_mobile_gpu.md) in Google MediaPipe framework

This project is developed and maintained by Google. This project is a bit different than the other hand detection project as it uses two TensorFlow Object detection in TFLite format, one for [palm detection](https://github.com/google/mediapipe/blob/master/mediapipe/docs/hand_detection_mobile_gpu.md), and another one for hand landmark detection which visualizes the hand skeleton.

The following is a brief hand detection process in the project.

1. `palm_detection.tflite`, the palm detection model, is being used first. It defines a cropped image region of a hand.
2. Then, `hand_landmark.tflite`, the hand landmark model, is being used to operate on the cropped image region and visualize the keypoints of a hand skeleton.

This Hand Tracking project aims to detect only one hand. There is another [Multi-Hand Tracking](https://github.com/google/mediapipe/blob/master/mediapipe/docs/multi_hand_tracking_mobile_gpu.md) project performing multi-hand detection and tracking. The `hand_landmark_3d.tflite` supports visualizing hand landmarks in 3D.

![Google-mediapipe-palm-detection-demo](https://lh3.googleusercontent.com/EMf52dPIm_73lYlvRvRDxFtOMZXxY6YvQcewpTcfFiisBofrWnsd2OMWC9gHGPp_e9-jLy1R1gX3=s250 "Google-mediapipe-palm-detection-demo"){width=42%} 	![Google-mediapipe-hand-tracking-gpu-demo](https://lh3.googleusercontent.com/KDScEy0ifWCZpaJ46-Wz0i7m5HuvmXOpIBotak4P85S-XGpPcJXIHMaNNFkSV7Tj0AUl1Uxxm_kZ=s250 "Google-mediapipe-hand-tracking-gpu-demo"){width=40%}

![Google-mediapipe-multi-hand-tracking-gpu-demo](https://lh3.googleusercontent.com/irS4YRqlzuxQgBocYQIdiYzIK7H94MfOkH84Th9o4Vo5hjmxpxxoGjf_oZkogaoqjV22QpyFtdp6=s250 "Google-mediapipe-multi-hand-tracking-gpu-demo"){width=40%} ![Google-mediapipe-hand-tracking-gpu-3D-demo](https://lh3.googleusercontent.com/WuhEfN3_sBJK5uCe_D046eqzufV1Re4a_yg_Sv-lEs__zB4wNO6f7CD657pas1c2o026eNTjy4uS=s250 "Google-mediapipe-hand-tracking-gpu-3D-demo"){width=48%}

The hand detection models in the project are custom models and trained from scratch. The palm detection model has an average precision of 95.7% [@google-ai-mp]. The hand landmark model is trained based on a dataset of around 30,000 real-world images and a number of synthetic hand images rendered by computers. All the images were annotated with 21 3D coordinates showing the keypoints of a hand skeleton. The hand landmark model could be used for gesture recognition based on the predicted hand skeleton.
