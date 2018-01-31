# **Traffic Sign Recognition** 

[//]: # (Image References)

[image1]: ./writeup/visualization.png "Visualization"
[image2]: ./writeup/b_n.png "Before Equalization"
[image3]: ./writeup/a_n.png "After Equalization"


## Data Set Summary & Exploration

### 1. Provide a basic summary of the data set.

* The size of training set is **34799**
* The size of the validation set is **4410**
* The size of test set is **12630**
* The shape of a traffic sign image is **32x32x3**
* The number of unique classes/labels in the data set is **43**

### 2. Include an exploratory visualization of the dataset.

The distribution of the training images with respect to the 43 traffic signs classes

![alt text][image1]

## Model Design & Architecture

#### 1. Image Preprocessing

At the begining I decided to normalize the data using `Z-score` but that alone didn't achieve good results on the validation set,
so I tried to convert the image to grayscale along with normalization it achieved better score but not the score we wanted to have,
I carried some manual error analysis and found out that the images the classifier wasn't doing good on were because they were very dark
and they would typically have even a bad human score so I searched and found out about `Histogram Equalization` and that alone made the score have a great leap from `0.75-0.8` validation score to `0.95` in the first few epochs.

![Before Histogram Equalization][image2]
![After Histogram Equalization][image3]

Data Augmentation could have been used too since as we see in the previous graph the difference in the amount of image for each class
vary alot but a good score was achieved without it so I didn't carry it out.

#### 2. Final model architecture

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x32 	|
| RELU					|												|
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x32 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 16x16x32 				|
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x64 	|
| RELU					|												|
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x64 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 8x8x64 				|
| Dropout		| keep_prob = 0.5        									|
| Fully connected		| Outputs = 1024        									|
| RELU					|												|
| Batch Normalization		|        									|
| Dropout		| keep_prob = 0.5        									|
| Fully connected		| Outputs = 1024        									|
| RELU					|												|
| Batch Normalization		|        									|
| Dropout		| keep_prob = 0.5        									|
| Fully connected		| Outputs = 43        									|
| Softmax				|         									| 


#### 3. Model Training

| Parameter         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Optimizer		| Adam        									|
| Batch Size		| 128        									|
| Epochs		| 15        									|
| Learning Rate		| 0.001        									|

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.999
* validation set accuracy of 0.982
* test set accuracy of 0.967

I started with the famous `LeNet` model but It was giving good training results but bad validation results, I started then to change in the model bit by bit by first adding `dropout` regularization to reduce overfitting that improved the model but no quite what I wanted so after that I started by increasing the number of filters and adding more convolution layers until I got the current model that achieved the current score. 

## Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image2]

The images weren't that hard to identify since they were clear images and the model was trained on less clear images `harder distribution ` so it didn't find difficulty in identifying the images

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set.

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| No Entry      		| No Entry   									| 
| Road work     			| U-turn 										|
| Right-of-way at the 	next intersection					| 	Right-of-way at the 	next intersection										|
| Children crossing	      		| Children crossing					 				|
| Speed limit (60km/h)			| Speed limit (60km/h)      							|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. The result is good since as we disscussed before the test images were easier than the images the model was trained on and it achieved a great score of `0.96` on it.
