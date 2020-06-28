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
The following is the most relevant machine learning approache for the implementation of hand detection in Android application.

### Hand Detection Models trained using TensorFlow Object Detection API
[**Real-time Hand Detector using Neural Networks (SSD) on Tensorflow**](https://github.com/victordibia/handtracking) [@victor-hand-detection] by Victor Dibia

The hand detection model trained in this project produces excellent results in tracking hand, with an average precision of 96.86%. It is a re-trained model based on a pre-trained standard SSD MobileNet V1 model with 200,000 training steps using the technique of Transfer Learning. Standard SSD MobileNet V1 model was selected because it is one of the fastest pre-trained object detection model, with a speed of 30 milliseconds for inference. The training process was run on a cloud GPU machine which takes around 5 hours to complete it.

The hand detection process is targeted to be run on videos or personal computers with a web camera capturing video stream using a [custom Python script](https://github.com/victordibia/handtracking/blob/master/utils/detector_utils.py) in the project. Multiple hands can be detected in each frame of the video or video stream from the web camera. Bounding boxes will be drawn in each frame to visualize the detection results.

![VictorDibia-HandTrack-demo](https://lh3.googleusercontent.com/E9T-y33nTByciI60NNBlJuNJfRxrx2csj85gv_CIK9oPmmGaWX7NPlRInPr4dwDbftXU4kfeOoVs=s300 "VictorDibia-HandTrack-demo"){width=55%} ![VictorDibia-HandTrack-video-demo](https://lh3.googleusercontent.com/FfdBQBQ6__LyzcfYffHRIbM4JiMerzaA_yggnboNCs34bL_9Dom7OaWofOph5w1y7PYVORkypeWP=s250 "VictorDibia-HandTrack-video-demo"){width=45%}

The dataset being used to train this model is [EgoHands](http://vision.soic.indiana.edu/projects/egohands/) [@Bambach_2015_ICCV] prepared by Indiana University. Hands were annotated and assigned to 4 classes, `own left`, `own right`, `other left`, and `other right`. In the project, the 4 classes are merged into one class --- `hand` so there is just one output label in the detection results.
