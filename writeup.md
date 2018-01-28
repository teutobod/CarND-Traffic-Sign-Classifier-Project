# **Traffic Sign Recognition** 


[//]: # (Image References)

[image1]: ./examples/keep_left.png
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"

[image4]: ./traffic-signs-data/web/stop_sign.jpg "Traffic Sign 1"
[image5]: ./traffic-signs-data/web/speed_limit_60.jpg  "Traffic Sign 2"
[image6]: ./traffic-signs-data/web/s_curve.jpg  "Traffic Sign 3"
[image7]: ./traffic-signs-data/web/priority_road.jpg   "Traffic Sign 4"
[image8]: ./traffic-signs-data/web/bumpy_road.jpg  "Traffic Sign 5"

### Data Set Summary & Exploration

#### 1. Summary of the data set

I used the numpy to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799 images.
* The size of the validation set is 4410 images.
* The size of test set is 12630 images.
* The shape of a traffic sign image is 32x32x3 pixels.
* The number of unique classes/labels in the data set is 43.

#### 2. Exploratory visualization of the dataset

Here is an example for one image in the data set:

![alt text][image1]

9, Keep left

### Design and Test of the model

#### 1. Preprocessing the image data

I decided to normalize the image data because the error surface of the CNN will
faster approach to global minima during the training phase.


#### 2. The final CNN architecture

My final model consisted of the following layers:


| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5    	| 1x1 stride, valid padding, outputs 28x28x18	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x18				    |
| Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x48   |	
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x48			        |
| Flattning				| outputs 1200                     				|
| Fully Connected      	| outputs 360			                        |
| RELU					|												|
| Fully Connected      	| outputs 252			                        |
| RELU					|												|
| Fully Connected      	| outputs 43			                        |
|                       |                                               |

#### 3. Model training
To train the model, I used the following parameter set:

optimizer = Adam optimizer
This optimizer was used in the LeNet lab and worked quite well for me.

training_rate = 0.001
This value was recommend as a good starting point and worked quite well for me.

Batch size = 128
This value was recommend as a good starting point and worked quite well for me.

Epoches = 20
Because I decided to increase the size of the CNN layers during the experimental training phase (see next section), I had to increase the
number of epoches too.


#### 4. Approach to finding the right model

My final model results were:
* validation set accuracy of 94.4 %  
* test set accuracy of 93.7 %

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
* What were some problems with the initial architecture?
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

I choose the LeNet architecture from the previous lab as starting point, because I already had a working implementation.
As it turned out, this model (without doing any change) already achieved a accuracy over 80%. That's why I decided to proceed with the LeNet architecture.
 
To get a higher accuracy I first tried to tune the batch size, epoche and training rate parameters, but the accuracy didn't improve much.
Next I enhanced the classification capability of the LeNet by extending the size of its layers. I increased the number of neurons in each layer 
(except the first and the last one) by a factor of three, which finally led to a higher classification accuracy (~90%).
Then I decided to extend the training phase because I assumed that a lager CNN will need more training cycles. Thus I doubled the epoches to 20.
After this step the accuracy of the validation set raised to 94.4%. The test set accuracy was 93.7%.



### Test on new images

#### 1. Test set

Here are the five German traffic signs that I found on the web. I used them to validate my trained model.

![alt text][image4] &ensp; ![alt text][image5] &ensp; ![alt text][image6] &ensp; ![alt text][image7] &ensp; ![alt text][image8]

I decided to choose traffic signs that have different shapes to evaluate if shape has any effects on the predicting results.

The first sign (stop) should be easy for the model to classify because stop signs are the only signs in the training set that have octagonal shape.

The second sign (speed limit 60) might be more challenging for the model because shape and color of different speed limit sign are the same. The
model needs to recognize the digits on the sign to classify it correctly.

The third sign (double curve) might be harder to classify correctly because there are multiple warning signs in the training set that only differ
by the black symbol in the center of the sign. At the same time the two signs dangerous curve left and dangerous curve right look very much the same.

The fourth sign (priority road) should be easy because shape and color are very unique.

The difficulty of classify the fifth sign (bumpy road) should be almost similar to the third one. The symbol in the sign center is quite filigree
this could be a problem for the prediction model.


#### 2. Classification results

Here are the results of the classification (most likely predictions):

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Stop        		    | Stop  									    | 
| Speed limit (60km/h)	| Speed limit (60km/h)							|
| Double curve			| Double curve									|
| Priority road	      	| Priority road				 				    |
| Bumpy road			| Bumpy road      							    |


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. 
This compares favorably to the accuracy on the test set, which only has a accuracy of 93.7%.

#### 3. Prediction certainties

In the table below you can see the top three soft max probabilities and its related classes for each
og the five traffic sign images above.

The code for making predictions on my final model is located in the 12th cell of the Ipython notebook.

| Top three probabilities       	     |     Prediction	        					                  | 
|:--------------------------:|:--------------------------------------------------------------------------:| 
| 9.99e-01 5.9e-05  4.6-06   | Stop sign, Speed limit 40km/h, Speed limit 20km/h   						  |
| 9.24-01  7.57e-02 5.08e-12 | Speed limit 60km/h, Speed limit 50km/h, Speed limit 80km/h  	              |
| 7.90e-01 1.65e-01 4.44e-02 | Double curve, Right-of-way at next intersection, Road narrows on the right |
| 1.00e+00 1.0e-13  4.4e-17	 | Priority road, Right-of-way at next intersection, No passing				  |
| 1.00e+00 9.0e-16  1.4e-16	 | Bumpy road, Bicycles crossing, Speed limit 20km/h                          |


For the first, fourth and fifth image the model is very sure (probability 1.0).

Image two has also a quite high probability value (0.924).

The third image has a lower probability (0.79). The model is still relatively sure about the classification outcome.
The second and third probabilities just have values of 0.16 and 0.04.
