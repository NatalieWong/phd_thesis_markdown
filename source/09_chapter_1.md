# Introduction

## Background

### Augmented Reality
The definition of AR is that views of physical real-world environments are augmented with superimposed computer-generated images, hence enhancing users’ current perception of the reality.

AR is gaining popularity in these days. Many companies and organizations are developing innovative projects based on AR technology and it has been widely applied in the fields of gaming, education, healthcare, retail and architecture etc.

With vivid visual overlay added to the view of the real world, user experience for a task or product is greatly enhanced, bringing enormous positive impact to our daily lives as well as the industries.

We are able to predict the AR technology will continue growing rapidly in the coming future.

<!-- 
To include a reference, add the citation key shown in the references.bib file.
-->

### Projection-based AR
Projection-based AR is one of the categories of AR technology. It can be done by using a projector, a camera and a computer.

Computer-generated graphics are projected onto real-world surfaces using a projector. Interaction between human and the projected graphics are allowed by using a camera to recognize real-world objects and detect movements, and a computer to make responses.

Projection-based AR provides a future vision of without the use of screens. It tries to merge the virtual graphics onto the environment in the reality. “Projection Mapping” is a related technology which relies on the use of computer vision to 3D scan the complex scenes in the environment, thus turning any real-world object into a screen for image projection.

### Current Interactive Information Wall using Xbox Kinect
The Interactive Information Wall located in the Sir Run Run Shaw Building (RRS) on the 7/F at HKBU is currently using the projection-based AR technology to project information on the wall.

- A projector is mounted on the wall to project content on the wall.
- A Kinect sensor is used to provide interaction between human and the projected computer graphics. It senses user’s gestures and transmits data to a desktop application running on a computer.

## Objective
This project aims to enhance the Interactive Information Wall at HKBU by further utilizing the technology of Projection-based AR.

### Problems of the Current Interactive Information Wall
The current Interactive Information Wall has the following 2 major problems.

#### *System - Slow Response Time*
On the idle screen, user has to raise up the hand to summon the menu. After that, however, the system takes an unknown time, usually more than 3 minutes to respond.

#### *Kinect Sensor - Insensitive to User’s Gestures*
User has to move the palm in the air to control the cursor. However, the Kinect sensor is a bit insensitive to the palm’s movements, and sometimes a cursor lag will happen. User may encounter difficulties in controlling the cursor and moving it to the desired position on the projected screen.

There is another issue regarding the current design of the "click” event. User has to hold their hand in the air still for about 3 seconds to complete the “click” event. 

### Android Lantern - an Alternative to the Kinect Sensor and current Computer System

![Android Things Lantern - Hardware](https://lh3.googleusercontent.com/3iPwZB9gchkw2s2yITK9BLdrBTrHS38ter2QOt-WZn9i2k80Q0EirODlOin8osvCSSshPWzQnJbk=s300 "banner-lantern-internal") 

![Android Things Lantern - Projection](https://lh3.googleusercontent.com/QGS5ZU5DCoCsjDu2ckgtxKGkwG9JGiGfwmjPQiiLPmLtgOSs-taNnomEUzCb3MIMn-3n8QgX-meU=s300 "banner-lantern-projection")

Inspired by the [Android Things Lantern](https://github.com/nordprojects/lantern) [@nordproj-lantern] developed by Nord Projects, the current Kinect sensor and the system could be replaced by the **Android Lantern** which can transform any surface into a projected user interface in the hope of shortening the response time by developing an Android application to enhance user experience and allowing user interacting with the virtually projected graphics in a real-time manner by making use of Computer Vision and Machine Learning.
