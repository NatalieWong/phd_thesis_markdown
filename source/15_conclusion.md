# Conclusion

<!-- 
A chapter that concludes the thesis by summarising the learning points
and outlining future areas for research
-->

I have successfully built a portable interactive device --- Android Lantern which consists of Raspberry Pi 3 B+ single-board computer, a palm-sized laser projector and a camera module in order to enhance the current Interactive Information Wall by making use of the projection-based AR technology.

The real-time interactive feature provided by the Android Lantern is achieved by a hand detection model, an Android application and the camera module for computer vision. Android OS is installed on the Raspberry Pi 3 for the deployment of Android application.

The user interface of the Interactive Information Wall is revised. Three kinds of information --- News, Gallery and Department Staff Information are shown on the Wall. In the Android application, fragments are used to enable easy navigation in the hierarchy.

To allow users interacting with the virtually projected graphics with the "click" event, the Android application plays an important role in obtaining the images captured by the camera module and passing them to the model file for inference. I have been using Camera API to obtain as many images as possible from a preview view.

For the hand detection model, I have mastered the training process of an object detection model, from generating TFRecord files based on the existing datasets to starting a model training and converting the model in TFLite format which is optimized for mobile applications.

The hand detection model in TFLite format is added to the Android application to perform inference on the images captured by the camera module. A "clicking" action can be triggered by a custom MotionEvent based on the location of a hand detected in each image.

Several hand detection models are trained using different pre-trained models and their performances on the inference are compared. SSD MobileNet V2 is the best pre-trained model for high accuracy in hand detection.

<!-- \appendix -->
\setcounter{chapter}{1}
\renewcommand{\thechapter}{A\arabic{chapter}}
