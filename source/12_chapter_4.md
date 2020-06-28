# Performance Results

The hand detection models' performances of inference are compared and the summaries are as follows. The models were trained based on the EgoHands dataset and on a desktop computer using one CPU.

### Re-trained from Quantized SSD MobileNet V1
The Python script used for the model training and evaluation process is `object_detection/model_main.py` and was executed using Python 2.7. The total training time of the model with 50,000 steps is 1900 minutes (around 31.7 hours).

#### TensorBoard Records for Model Training and Evaluation
***Model trained with 50,000 steps***

![TensorBoard-py2-50k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/m3O4y9yBSJAYxu4YX_gVHaUB3xUACzioKBnL7a4fYADzcB6H0OnvuVoKjEQn4CemOYK6GCC934NE=s300 "TensorBoard-py2-50k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-py2-50k-Loss](https://lh3.googleusercontent.com/F2FxcJ_xRNyHbS2s02dBeJT08_ydSm0KGE9M6dLhRa2vl0CyOh3oRji9wdOFUWboPK5INgTpYmds=s300 "TensorBoard-py2-50k-Loss"){width=50%}

The value of mAP climbed and the value of the total loss decreased gradually throughout the training processes, indicating that hand detection model was well trained.

#### Performance of inference on Raspberry Pi 3 B+ Model through Android application
The confidence of hand detection results is 71.09%. It could accurately locate a hand across the images but the detection is less smooth, i.e. bumpy detection with around 1 second delay.

### Re-trained from Quantized SSD MobileNet V2
The Python scripts used for the model training and evaluation process are `object_detection/legacy/train.py` and `object_detection/legacy/eval.py` and were executed using Python 2.7. The total training time of the model with 100,000 steps is 5967 minutes (around 99 hours).

#### TensorBoard Records for Model Training and Evaluation
***Model trained based on EgoHands dataset with 100,000 steps***

![TensorBoard-v2-100k-DetectionBoxes_Precision](https://lh3.googleusercontent.com/j3xkeDb8CCjjXpOIz_tuDUvzoIt1iZb-CiZRTNcp9w_xOuyvTU4tmF0BqoQUtDFGCLi78XF0awI1=s300 "TensorBoard-v2-100k-DetectionBoxes_Precision"){width=50%} ![TensorBoard-v2-100k-Losses](https://lh3.googleusercontent.com/oFu5vcXGnllQMMf5DdD3_9QUTvFPsAqjvlDRvO7Qsr4Ud0XBr3V85DvTp0YKQk-g3p2P7RcfFjOs=s300 "TensorBoard-v2-100k-Losses"){width=50%}

#### Performance of inference on Raspberry Pi 3 B+ Model through Android application
The model has a high accuracy of hand detection. The confidence of hand detection results is 99.61%. The positions of a hand across the images could be located very accurately with with less than 1 second delay, almost a real-time detection.
