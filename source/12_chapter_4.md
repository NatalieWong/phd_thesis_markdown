# Performance Results

## Comparison of Pre-trained Object Detection Models' Performance
There are many choices of pre-trained model in the [Tensorflow detection model zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md) but not all models can be converted into TensorFlow Lite format which can be used for object detection on mobile devices. It is because only a small set of TensorFlow operations are compatible with those of TensorFlow Lite. Operations of TensorFlow Lite target at floating-point (float32) and quantized (uint8, int8) inference. At present, only the MobileNet models can be converted into TensorFlow Lite format and the quantized MobileNet models can be run on Coral USB Accelerator, an on-board Edge TPU coprocessor for faster machine learning inference on mobile devices.

The following two scatter diagrams show the differences among the TensorFlow models in terms of size, accuracy and latency. MobileNet models which are optimized to run on mobile devices has a relatively small size and low latency, however, the tradeoff is the decreased accuracy of inference. Moreover, the quantized models have an up to 4 times reduction on the size of their original models as the float weights are turned into 8-bit only, allowing the models to run faster on mobile devices.

![TensorFlow models - comparisons](https://lh3.googleusercontent.com/yBxte6onUHFr8lIcYNmJoMaGDWie9mKQFdIrGJr8UT0ThV17wAuZRFLwz7-9XitXWvw8OXjDhhq0=s550 "TensorFlow models - comparisons")

## Performance of Inference using different Hand Detection Models
I have successfully trained several hand detection models with different number of training steps based on Quantized SSD MobileNet V1 and V2 using EgoHands dataset prepared by Indiana University as well as the hand dataset prepared by myself using the Auto CSV and TFRecord files Generator.

***TensorBoard --- a visualization tool for object detection model training***

TensorBoard is an web interface visualizing the metrics for a model training and evaluation. After each training, I have accessed to the TensorBoard to learn more about the learning process of the trained model. The average precision (mAP) and the total loss are the metrics that I was looking at to see if a model was successfully trained. The mAP shows the model's accuracy of prediction and the total loss indicates the errors made by the model during the evaluation.

***Testing Hand Detection Models on Samsung Note 3 smartphone***

I have installed the official TensorFlow Object Detection Demo application for Android on the smartphone and used it to try out the freshly trained hand detection model first before adding the model to this project’s Android application.

The models' performances of inference are compared and the summaries are as follows.

### Re-trained from Quantized SSD MobileNet V1
5 hand detection models were trained based on the EgoHands dataset.

The model with 500 training steps was trained on Macbook Pro using one CPU core. The model with 1,500 training steps was trained on Macbook Pro using a quad-core processor. The rest of the models were trained on a desktop computer using one CPU.

The Python script used for the training and evaluation process is `object_detection/model_main.py` and it was executed using Python 2.7.

#### Total Training Time of the models
| 500 steps | 1,500 steps | 15,000 steps | 30,000 steps | 50,000 steps |
|:--:|:--:|:--:|:--:|:--:|
| 61 minutes | 1 hour 20 minutes | 11 hours (667 minutes) | 19 hours (1135 minutes) | 31.7 hours (1900 minutes) |

#### TensorBoard Records for Model Training and Evaluation
***Model trained with 1,500 steps***

![TensorBoard-py2-1500steps-DetectionBoxes_Precision](https://lh3.googleusercontent.com/mY6XtIKTaJBBpOpg4eWwm8O1rYIMKJSbTRkVQRPxU3hFtGC_8invfQ46lH2govwe9RQ3KeNwzuqD=s300 "TensorBoard-py2-1500steps-DetectionBoxes_Precision"){width=50%} ![TensorBoard-py2-1500steps-Loss](https://lh3.googleusercontent.com/GirLRgmtZctNeCP2NpEWCyMQrDu6B9vNLjxI4bcqZFGLn-xlr5gxA57aD6jttgDvn8ZYBxZqs_ZD=s300 "TensorBoard-py2-1500steps-Loss"){width=50%}

***Model trained with 30,000 steps***

![TensorBoard-30k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/XIuFArRQobKXILukD0iS40h7lghyaVSXm9SUtC1tt_zQa0RDrw_5ZctdWkmJjbGGu7-XabEGSTGJ=s300 "TensorBoard-30k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-30k-loss](https://lh3.googleusercontent.com/ggUpjPBXmh6AbNd_gtKx9aW2GAHW0v4pfMkO4YN8TjNSaKVEa6C4kS0I09Yi0J2CTzwp0ouArONf=s300 "TensorBoard-30k-loss"){width=50%}

***Model trained with 50,000 steps***

![TensorBoard-py2-50k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/m3O4y9yBSJAYxu4YX_gVHaUB3xUACzioKBnL7a4fYADzcB6H0OnvuVoKjEQn4CemOYK6GCC934NE=s300 "TensorBoard-py2-50k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-py2-50k-Loss](https://lh3.googleusercontent.com/F2FxcJ_xRNyHbS2s02dBeJT08_ydSm0KGE9M6dLhRa2vl0CyOh3oRji9wdOFUWboPK5INgTpYmds=s300 "TensorBoard-py2-50k-Loss"){width=50%}

The value of mAP climbed and the value of the total loss decreased gradually throughout the training processes, indicating that hand detection models were well trained.

#### Performance of inference on Samsung Note 3
The following chart shows the confidence of hand detection results (in %) corresponding to the models' total number of training steps. Higher confidence means that the model has a higher accuracy in the detection of a hand.

![samsung-note-3-v1-confidence](https://lh3.googleusercontent.com/HCq9EoNYWiDGpci4P5ByuX1dFC6fcOE3yofhZXBJ2k2v8assNqYE2SUfvsCQqemKgSuGJuZUYqBj=s600 "samsung-note-3-v1-confidence") \

The model with 500 training steps is able to detect a hand, however, the confidence of the detection results is low. The models with training steps greater than or equal to 1,500 have significant higher confidence. Smoother detection of a hand was achieved using the models with training steps greater than or equal to 15,000, however, there could be multiple detection results for a single.

#### Performance of inference on Raspberry Pi 3 B+ Model through Android application
Only the model with 50,000 steps was deployed on the Raspberry Pi 3 and the confidence of hand detection results is 71.09%. It could accurately locate a hand across the images but the detection is less smooth, i.e. bumpy detection with around 1 second delay.

#### Problems of using `object_detection/model_main.py` for model training
I attempted to train a model with 100,000 as the target number of total training steps using the desktop computer with one CPU. The training process ran smoothly until it was killed at step 74,555. The training process using a pre-trained quantized SSD MobileNet V1 model and the Python script `model_main.py` cannot reach 100,000 training steps on the desktop computer.

### Re-trained from Quantized SSD MobileNet V2
Two hand detection models were trained based on the EgoHands dataset and one model was trained based on my own dataset.

All the 3 models were trained using one CPU in the desktop computer. The Python scripts used for the training and evaluation process is `object_detection/legacy/train.py` and `object_detection/legacy/eval.py`. They were all executed using Python 2.7.

#### Total Training Time of the models
Two models trained based on EgoHands dataset:

| 50,000 steps | 100,000 steps |
|:--:|:--:|
| 37.1 hours (2228 minutes) | 99 hours (5967 minutes) |

The model trained based on my own dataset:

| 50,000 steps | 
|:--:|
| 49.5 hours (2969 minutes) |

#### TensorBoard Records for Model Training and Evaluation
***Model trained based on EgoHands dataset with 100,000 steps***

![TensorBoard-v2-100k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/j3xkeDb8CCjjXpOIz_tuDUvzoIt1iZb-CiZRTNcp9w_xOuyvTU4tmF0BqoQUtDFGCLi78XF0awI1=s300 "TensorBoard-v2-100k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-v2-100k-Losses](https://lh3.googleusercontent.com/oFu5vcXGnllQMMf5DdD3_9QUTvFPsAqjvlDRvO7Qsr4Ud0XBr3V85DvTp0YKQk-g3p2P7RcfFjOs=s300 "TensorBoard-v2-100k-Losses"){width=50%}

***Model trained based on my own dataset with 50,000 steps***

![TensorBoard-mydataset-v2-50k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/lvAtkjAL0PjtYvIuP1qnjzlJukQzxYqvPeXytE_tiXP96_cU639eCr7Q0XI29ZdDKt-N0Fklnr8m=s300 "TensorBoard-mydataset-v2-50k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-mydataset-v2-50k-Losses](https://lh3.googleusercontent.com/Phtcs-p35q2cM8zXq2AN8pLp2qk9GhvSIGDGBhWkmmCbeRluSI3SrEdW11hTU49rGzWjNyx6VAyr=s300 "TensorBoard-mydataset-v2-50k-Losses"){width=50%}

In general, the mAP value rose and the value of the total loss fell throughout the training processes, indicating that hand detection models were successfully trained.

For the model trained with 100,000 steps based on EgoHands dataset, the mAP value surged to 0.6 at step 20,000 and started to level off at step 50,000. On the other hand, for the model trained with 50,000 steps based on my own dataset, the mAP value only climbed to 0.3 at step 20,000 and reached 0.65 at the end of the training process. It seems that the model trained with 100,000 steps based on EgoHands dataset is better than the one trained with 50,000 steps based on my own dataset.

#### Performance of inference on Raspberry Pi 3 B+ Model through Android application
The following chart shows the confidence of hand detection results (in %) corresponding to the models' total number of training steps. The 2 models were trained based on the EgoHands dataset.

![rpi3-v2-confidence](https://lh3.googleusercontent.com/zeCXQhkup5DCvWRXA5Jet5VeLAUV2ngtU0RXUL30et6DLIg1LFBdFXkOAoGuGOIDwYIZb9P76vkz=s600 "rpi3-v2-confidence") \

Both of the models have a high accuracy of hand detection. The positions of a hand across the images could be located very accurately with with less than 1 second delay, almost a real-time detection.

For the model trained based on my own dataset, the total training steps was 50,000 and the average confidence of hand detection is only 59.37%, with a maximum confidence up to 93.35%. This model is able to locate a hand across the images, however, the detected position is sometimes inaccurate. Moreover, the detection is unsmooth, i.e. very bumpy detection with a minimum of 2 seconds delay. 

#### Performance of inference on Samsung Note 3
The confidence of hand detection for the 2 models trained based on the EgoHands dataset is 99.61%. Both of them could achieve smooth hand detection and only one detection result was shown for a single hand.

The average confidence of hand detection for the model trained based on my own dataset is 82.03%, with a maximum confidence up to 90.63%. Only open hand, i.e. the back of the hand facing upward with all fingers pointing upward, could be easily detected as most of the images in my dataset contains images of this kind of gesture. Nonetheless, the detection is less smooth, i.e. bumpy detection.

#### Observation of the Confidence Level among the Detection Results
There was a wide range of confidence among the detection results, from 12.5% or less, up to 99.6%.

### Problem for All the Models
Other objects which have similar color to skin could be misclassified as hand.

### Discovery regarding the use of Python Scripts for Object Detection Model Training
For the hand detection model training process based on the EgoHands dataset, I had started the model training using quantized SSD MobileNet V2 using `object_detection/model_main.py` executed by Python 2.7, however, the training process killed abruptly at step 222 as the model evaluation process could not be done. Additionally, I had used the `object_detection/legacy/train.py` and `object_detection/legacy/eval.py` to re-train the quantized SSD MobileNet V1 model for hand detection. Yet, I could not even start the training process.

The combination of a pre-trained model and the Python script(s) used for the training process is summarized as follows.

| `object_detection/` | Quantized SSD MobileNet V1 | Quantized SSD MobileNet V2 |
|:--:|:--:|:--:|
| `model_main.py` | Yes (works well with 50,000 as the target no. of training steps but unable to reach 100,000 training steps on a desktop computer with one CPU) | No |
| `legacy/train.py` and `legacy/eval.py` | No | Yes |


### Conclusion of the Hand Detection Models' Performance
Among all the hand detection models that I have trained so far, the best hand detection model is trained using the EgoHands dataset prepared by Indiana University and the quantized SSD MobileNet V2 pre-trained model. The following graphs compare the hand detection confidence among the best 2 hand detection models re-trained from quantized SSD MobileNet V1/V2 with 50,000 and 100,000 steps respectively using EgoHands dataset as well as the model re-trained from quantized SSD MobileNet V2 with 50,000 steps using my own dataset.

***Comparison of hand detection confidence running on the smartphone***

![samsung-note-3-v1-v2-mydataset-confidence](https://lh3.googleusercontent.com/I-useYio3d5II3mWNLnz15jvkBzaOM9mw_08xfr8G8nwchul9xCClNBtjs2hJruDwTasF6WQsIM7=s680 "samsung-note-3-v1-v2-mydataset-confidence") \

***Comparison of hand detection confidence running in the Android application on Raspberry Pi 3 B+ Model***

![enter image description here](https://lh3.googleusercontent.com/gDET1rpTGtdHTSZ5tH6yhLG6c7_a8aPPi3WHo5r8AnhYMrqmL3rNTBsR-6rEFz2GOYe3y2Mxz9dR=s680 "rpi3-v1-v2-mydataset-confidence") \

To conclude, a good hand detection model for mobile application, which is able to locate a hand accurately and with minimal delay or real-time hand detection, can only be generated by using a quality dataset, and re-training a pre-trained object detection model with higher accuracy and low latency like quantized SSD MobileNet V2.

#### Criteria of a Quality Hand Dataset
A quality hand dataset should have the following traits.

For the images,
- hands can be clearly seen, and
- size (or dimension) of all the images should be consistent with the images captured by the camera for inference. It is better to be in HD format already without resizing.

For annotation of hands in the images,
- all the hands which can be clearly seen in each image should be annotated to maximize the data being used to train a hand detection model.

#### Evaluation on the Quality of the Datasets
Even though my dataset has images collected from the videos of people playing the piano, typing words on the keyboard and folding Origami models have more gestures which suit my purpose, the Egohands dataset still outperforms my own dataset. The Egohands dataset has more than 4,401 images (around 83% of total images) for model training. Annotations for all the hands that appear in each image are done. In contrast, for my own dataset, I only have 1,537 images (80% of the total images collected from 7 videos) for model training. Only one hand can be detected and annotated for each image.

## Using Android OS instead of Android Things
At the very beginning of this project, I had followed user guide of the [Android Things Lantern's demo project](https://github.com/nordprojects/lantern) so I had installed the Android Things on the Raspberry Pi. Android Things is an operating system specialized for smart devices to execute Android applications on them.

### Problems of using Android Things on Raspberry Pi 3
When I first deployed a hand detection model re-trained from SSD MobileNet V1 model in both of the Android applications running on Samsung smartphone and Raspberry Pi 3, I found that the hand detection process runs smoothly, achieving real-time inference with the best hand detection result's highest confidence of 84% on the smartphone, however, there is a high latency, with around 2 to 3 seconds delay, for the hand detection process running on the Raspberry Pi 3 and the highest confidence of the best hand detection result is only 72%.

This problem may be associated with the Android Things. The Android Things only supports 32 bits processing but Android OS supports 64 bits processing. As the processor of Raspberry Pi 3 supports 64 bit processing, I have installed the Android OS on the Raspberry Pi 3 in a bid to make fully use of the computational power of the Raspberry Pi 3 so that the hand detection process can be enhanced.
