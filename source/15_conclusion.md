# Conclusion

<!-- 
A chapter that concludes the thesis by summarising the learning points
and outlining future areas for research
-->

I have successfully built a portable interactive device --- Android Lantern which consists of Raspberry Pi 3 B+ single-board computer, a palm-sized laser projector and a camera module in order to enhance the current Interactive Information Wall by making use of the projection-based AR technology.

The real-time interactive feature provided by the Android Lantern is achieved by a hand detection model, an Android application and the camera module for computer vision. Android OS is installed on the Raspberry Pi 3 for the deployment of Android application.

To allow users interacting with the virtually projected graphics with the "clicking" action, the Android application plays an important role in obtaining the images captured by the camera module and passing them to the model file for inference. Through using both Camera and Camera2 API to obtain the images, I have discovered the shortcomings of Camera2 API and merits of using Camera API to obtain as many images as possible from a preview view.

For the hand detection model, I have mastered the training process of an object detection model, from generating TFRecord files based on the existing datasets to starting a model training and converting the model in TFLite format which is optimized for mobile applications. To build a dataset which includes more images with gestures suitable for the Interactive Information Wall, I have created Python scripts to automatically annotate a hand in video frames using OpenCV API for hand detection and generate TFRecord files for model training. Several hand detection models are trained using different datasets as well as pre-trained models and their performances on the inference are compared. A good hand detection model can only be trained from a quality hand dataset and pre-trained object detection model with high accuracy for inference. A quality dataset should have more than 4,400 images in which the bounding boxes are perfectly fit in the size of the hands. SSD MobileNet V2 is the best pre-trained model to high accuracy for hand detection.

The hand detection model in TFLite format is added to the Android application to perform inference on the images captured by the camera module. A "clicking" action can be triggered by a custom MotionEvent based on the location of a hand detected in each image.

The user interface of the Interactive Information Wall is revised. Three kinds of information --- News, Gallery and Department Staff Information are shown on the Wall. In the Android application, fragments are used to enable easy navigation in the hierarchy and data binding with MVVM architecture as well as observable data objects are used to achieve real-time updates on the UI components' data fields and manageable Android application in which UI's logic and program's logic are decoupled.
