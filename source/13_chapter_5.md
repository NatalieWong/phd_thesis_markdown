# Discussions

## Limitations
### Dependency on Color Features for Hand Detection Model
Reviewing the problems of the hand detection models that I have trained, I discovered that other objects which have similar color to skin would be misclassified as hand. This shows that the hand detection models rely heavily on color features to detect a hand.

For the image annotation of hands in EgoHands dataset, apart from labeling the whole part of a hand, any part of a hand, like a finger, which could be seen manually in an image was also annotated. (The green non-filled rectangle is the bounding box of the hand.)

![EgoHands-hand-part-annotation](https://lh3.googleusercontent.com/r-bOD4_DHyJmOqtWTWdLmOVOLumN4GdreApXzB5TWMKLFLlg_-tzbhMpVaouIwPLq4HLPgvmaPIg=s500 "EgoHands-hand-part-annotation")
![EgoHands-hand-part-annotation](https://lh3.googleusercontent.com/2VYbJQGtnsj0GkEsFMw0OT58iYT2LS1hCOktiYJJrtS57bcWDmyQtb-XwQB9SIdyVOZG9YqFXxjV=s500 "EgoHands-hand-part-annotation-2")
![EgoHands-hand-part-annotation](https://lh3.googleusercontent.com/5Lvyalyv-sYKO4wafRQap8r5sHdKAw0ZSUBbWChzVUW-mz4wziq44gjB5tzLOpxkX_tbVFjJ3QYh=s500 "EgoHands-hand-part-annotation-3")
![EgoHands-hand-part-annotation](https://lh3.googleusercontent.com/M_hWzJVv7qTuujzOnUo7ceMETGe2PZx4S1BhYDPBvRn_Jlvs7_6yJB_kuLqtwvqTkZ-sPXXFZtIi=s500 "EgoHands-hand-part-annotation-4")

Therefore, I assume that this is the reason why the models rely heavily on color features in the detection.

Though, the detection results of the objects which are misclassified as hand usually have a lower confidence than that of a real hand so the possible solution is to filter the detection results by setting a minimum confidence level.

#### Weaker Computational Power of Raspberry Pi
By comparing the hand detection results in the Android applications running on the smartphone and Raspberry Pi, I found that the performance of hand detection on smartphone always outperform the one on the Raspberry Pi when they are using the same model for inference.

There is a tradeoff between the accuracy of the hand detection result and the detection speed for the Android application running on Raspberry Pi. When I am using the hand detection model re-trained from the pre-trained SSD MobileNet V2 model, in the demo project running on the smartphone, I have set the minimum confidence level of the hand detection result to `0.6f` in the hope of minimizing the misclassified results and this solution did work, however, in this project's Android application, I can only set the minimum confidence level to `0.1f` in order to achieve real-time hand detection.

The implementation for hand detection process are the same as I have just reused the `Classifier.java` and `TFLiteObjectDetectionAPIModel.java` in the demo application for this project's Android application. At this point, I am able to conclude that the computational power of Raspberry Pi is weaker than that of the smartphones and this affects the hand detection performance.

### Black Background Color for Android application UI to Smoothen Hand Detection
Previously, the background color of the UI is white and this raises a color overlaying problem.

The hand detection model relies on color during the inference in a certain extend. That means if an object has a similar color to the skin, the model may wrongly predict it as a hand. Conversely, if a hand has a color which is not similar to that of skin, it may not be detected. The latter is the situation I am facing when the the background color of the UI is white.

As white light beams produced by the projector are projected on the hand, it makes a hand whiter, having a color which is dissimilar to that of skin. This reduces the accuracy of hand detection on images captured from camera module. A hand cannot keep being detected across the frames in the preview view and thus reduces the user experience on navigation in the application.

By changing the background color of the UI to black, white light beams can be eliminated. The skin color of a hand being captured by the camera module is retained, allowing a hand to be detected smoothly across the frames in the preview view.

### Single Hand Detection in Android application
At present, only the best hand result is selected among the detection results and one hand position indicator, i.e. a red dot, is drawn on the UI based on the location of the best result.

### Latency for Hand Detection on Raspberry Pi
Although I have switched to use Android OS on Raspberry Pi and deployed the best hand detection model re-trained form SSD MobileNet V2 model in the Android application running on the Raspberry Pi, the one-second delay of the hand detection process still persists while I am running the Android application. I doubt that this is another problem raised due to the weaker computational power of the Raspberry Pi.

## Further Developments
### Implementation
#### Using libGDX API to improve the speed for the conversion of image format from YUV to RGB
Currently the image format captured from the preview view is in YUV format, however, the TensorFlow Object Detection API can only support inference on images in RGB format. The conversion of image format from YUV to RGB is now done by `convertYUV420SPToARGB8888()` which can only be executed on the CPU.

The idea of the enhancement is to load the Y and UV channels of an image into GL textures, then draw the textures onto a Mesh by using a custom shader [suggested by Ayberk Özgür](https://stackoverflow.com/questions/22456884/how-to-render-androids-yuv-nv21-camera-image-on-the-background-in-libgdx-with-o)  which performs color space conversion to RGB format.

Two classes - `ShaderProgram` and `Mesh` provided by libGDX API will be used to create GL textures and perform the rendering of images in RGB format. Since the shader can be run on the GPU of the Raspberry Pi, the image format conversion speed is enhanced.

```kotlin
shader = ShaderProgram(
	"attribute vec4 a_position;                         \n" +
	"attribute vec2 a_texCoord;                         \n" +
	...
	
	"void main(){                                       \n" +
    "   gl_Position = a_position;                       \n" +
    "   v_texCoord = a_texCoord;                        \n" +
    "}                                                  \n",
	...
	
    "void main (void){                                  \n" +
    "   float r, g, b, y, u, v;                         \n" +
    
    "   y = texture2D(y_texture, v_texCoord).r;         \n" +
    "   u = texture2D(uv_texture, v_texCoord).a - 0.5;  \n" +
    "   v = texture2D(uv_texture, v_texCoord).r - 0.5;  \n" +
    
    "   r = y + 1.13983 * v;                            \n" +
    "   g = y - 0.39465 * u - 0.58060 * v;              \n" +
    "   b = y + 2.03211 * u;                            \n" +
    
    "   gl_FragColor = vec4(r, g, b, 1.0);              \n" +
    "}                                                  \n"
)
```
```kotlin
mesh = Mesh(true, 4, 6, // static mesh, 4 vertices, 6 indices
    VertexAttribute(VertexAttributes.Usage.Position, 2,
					"a_position"),
    VertexAttribute(VertexAttributes.Usage.TextureCoordinates, 2,
				    "a_texCoord"))
mesh?.let {
	it.setVertices(vertices)
	it.setIndices(indices)
}
```
The image format conversion utilizing the libGDX should be continuously running at the background along with the hand detection routines in the MainActicity which is a class extended from the AppCompatActivity. Unfortunately, the current AndroidApplication class provided by the libGDX is not fully compatible with AppCompatActivity, thus my MainActivity class cannot be extended from the AndroidApplication.

A possible solution is to revise the implementation of the AndroidApplication class making it compatible with AppCompatActivity, then compile the whole libGDX project as a `.jar` file and add it to my Android application project.

#### Coral USB Accelerator for Hand Detection Model Inferencing
Coral USB Accelerator is an on-board Edge TPU co-processor designed to significantly accelerate the model inferencing in a power efficient manner on small and low-power devices like smartphones and Raspberry Pi.

The co-processor is able to perform 4 trillion operations per second. The following are the official performance benchmarks produced by C++ benchmark tests [@coral-tpu].

| Model architecture | Desktop CPU* | Desktop CPU*  + USB Accelerator (USB 3.0) |
|:--:|:--:|:--:|
| SSD MobileNet V1 | 109 | 6.5 |
| SSD MobileNet V2 | 106 | 7.2 |
> Remarks:
	- Time is measured in milliseconds per inference.
	- The models have an input tensor size of 224 x 224
	- *Desktop CPU: Single Intel® Xeon® Gold 6154 Processor @ 3.00GHz

I would like to use the Accelerator in order to improve the speed for the hand detection model inferencing on Raspberry Pi 3. Nevertheless, the current API of the Accelerator only supports running a model inferencing using Python script. I have to write a program acting as an adapter so that I am able to use the Accelerator in Android application.

#### Re-train a hand detection model which supports gesture recognition
The current hand detection can only detect the position of a hand in each image captured from the preview view and does not support recognition of any gestures.

In fact, scrolling is one of the essential action for navigation in the pages where there are lots of sub-items. In the news page and department staff information page, there are many news articles and academic staff respectively. Now there are only 3 items shown on each page. To achieve a better navigation experience, fragment transition could take place when a user waves the hand towards left or right, allowing different items to be shown on the page.

Further research into the TensorFlow [Gesture Classification Web App](https://github.com/tensorflow/examples/tree/master/lite/examples/gesture_classification/web), a demo project showing how to generate TensorFlow Lite models for gesture recognition, can be done so as to enhance the interactive features provided by the Lantern Information Wall.

#### Improvement on the Presentation of UI Components
There is lots of text in the news page and department staff information page particularly. I think this poses a negative effect towards user experience as this violates the simplicity of a UI.

Re-design on the presentation of the text is therefore needed to reduce the text shown in a page without affecting the content that is essential for the users to understand its meaning.
