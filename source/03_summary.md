# Abstract {.unnumbered}

<!-- This is the abstract -->

Nowadays, AR technology has been applied in many innovative projects across the industries to enhance user experience for tasks or products. Taking the advantage of Projection-based AR, screens are no longer needed. Instead, computer-generated graphics are projected onto real-world surfaces and human can directly interact with the virtual graphics.

The current Interactive Information Wall which uses a Kinect Sensor and computer system to detection hand motions could be replaced by the Android Lantern, a portable interactive device which consists of a Raspberry Pi 3 single-board computer, a laser projector and a camera module turning any surface into a fully interactive user interface. Users are able to "click" on the virtually projected graphics using their hands and receive real-time response from the Lantern.

Hand detection machine learning models are trained using TensorFlow Object Detection API to recognize human hands in images captured by the camera module embedded in the Lantern. An Android application is deployed on the Raspberry Pi 3 to process the images, trigger the "click" event and provide the user interface for the Interactive Information Wall.

In view of the low applicability of the existing hand dataset and time-consuming manual image annotation procedures, an auto CSV and TFRecord files generator for hand detection model training is developed in order to build a hand dataset which tailors to the needs of the Interactive Information Wall for certain gestures to achieve the "click" event and automates the procedures of annotating the collected images by using OpenCV API for hand detection.

<!-- ## Summary of chapters -->

<!-- 
For italic, add one * on either side of the text
For bold, add two * on either side of the text
For bold and italic, add _** on either side of the text
-->

This is a brief outline of what went into each chapter. **Chapter 1** gives a background on the application of AR in various industries and the use of Android Lantern to enhance the current Interactive Information Wall.  **Chapter 2** provides an Application Overview including the architecture and the existing methods for the implementation of objection detection.  **Chapter 3** explains the proposed methods and actual implementation of Android application as well as hand detection model training for the Interactive Information Wall. It also covers the implementation of additional Python scripts for building a custom hand dataset. **Chapter 4** shows the performance results of different hand detection models deployed on the Android applications running on smartphones and Raspberry Pi 3 Model B+. **Chapter 5** discusses the limitations of the hand detection models trained as well as the Android application developed, and the possible further developments for this project.

\pagenumbering{roman}
\setcounter{page}{1}

\newpage
