# Abstract {.unnumbered}

<!-- This is the abstract -->

Nowadays, Augmented Reality (AR) has become popular and many of the innovative projects across the industries have been applying the AR technology to enhance user experience for a task or product. In the coming future, taking the advantage of Projection-based AR, screens are no longer needed. Instead, computer-generated graphics are projected onto real-world surfaces and human are allowed to directly interact with the virtual graphics.

The current Interactive Information Wall which uses a Kinect Sensor and computer system to detection hand motions could be replaced by the Android Lantern, a portable interactive device which consists of a Raspberry Pi 3 single-board computer, a laser projector and a camera module turning any surface into a fully interactive user interface. Users are able to "click" on the virtually projected graphics using their hands and receive real-time response from the Lantern.

Hand detection machine learning models are trained using TensorFlow Object Detection API to recognize human hands in images captured by the camera module embedded in the Lantern. An Android application is deployed on the Raspberry Pi 3 to process the images, trigger the "clicking" actions and provide the user interface for the Interactive Information Wall.

In view of the low applicability of the existing hand dataset and time-consuming manual image annotation procedures, an auto CSV and TFRecord files generator for hand detection model training is developed in order to build a hand dataset which tailors to the needs of the Interactive Information Wall for certain gestures to achieve the "clicking" action and automates the procedures of annotating the collected images by using OpenCV API for hand detection.

\pagenumbering{roman}
\setcounter{page}{1}

\newpage
