#**Behavioral Cloning** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/center.png "center normal image"
[image2]: ./examples/left.png "left normal image"
[image3]: ./examples/right.png "right normal image"
[image4]: ./examples/center_flip.png "center flip image"
[image5]: ./examples/left_flip.png "left flip image"
[image6]: ./examples/right_flip.png "right flip image"
[image7]: ./examples/center_crop.png "center crop image"
[image8]: ./examples/left_crop.png "left crop image"
[image9]: ./examples/right_crop.png "right crop image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.ipynb containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md for summarizing the results
* run1.mp4 a video recording of driving autonomously for two laps

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.ipynb file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy

####1. Solution Design Approach

I firstly tried LeNet as training model, with data I collected by driving one lap. It did not work well and the car drove off the track.

Then I tried with NVIDIA model, it was better as the car drove smoothly, however, the car still drove off the track, especially at the turn right afte the bridge, which has red lane on the left, and sand on the right. 

With advice from the course, I started by creating more data. And it worked much better!

With advice from reviewer, I noticed that the images used for training was read by opencv(cv2) and they were in BGR format. In drive.py, images were reading using PIL and they were in RGB format. To solve the issue, I transformed images to RGB format and re-trained the model.

The driving seemed good, however, it touched the lane on the same spot again. So I decided to collect more data, specifically for that turn spot.

With turn data I collected, I re-trained the model, and finally, it worked great!

####2. Final Model Architecture

My model is based on NVIDIA's model:
1. Normalize layer
2. Crop layer to get rid of useless information like sky
3. One convolutional layer with 5x5 filter size, depth 24, 2x2 strides
4. One convolutional layer with 5x5 filter size, depth 36, 2x2 strides
5. One convolutional layer with 5x5 filter size, depth 48, 2x2 strides
6. One convolutional layer with 3x3 filter size, depth 64, 1x1 strides
7. One convolutional layer with 3x3 filter size, depth 64, 1x1 strides
8. Flatten layer with input 1x33x64, output 2112
9. Dense layer with input 2112, output 100
10. Dropout layer with rate 0.5
11. Dense layer with input 100, output 50
12. Dropout layer with rate 0.5
13. Dense layer with input 50, output 10
14. Dense lyaer with input 10, output 1 

####2. Attempts to reduce overfitting in the model

To avoid overfitting, I added two dropout layers between dense layers. Also, I trained model with 2 epochs. 

####3. Model parameter tuning

The model used an adam optimizer, so the learning rate was default value. To avoid overfitting, I used 2 npochs to avoid overfitting.

####4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. 

I collected three data set: 
1. center lane driving
2. smooth driving around curve, and counter-clockwise
3. making turns on the spot when the car is likely to touch the right lane

I combined my collected data with data provided by Udacity. Then augumented data by flipping image & measurement. Also, I cropped image by removing useless information like sky!

Please find data samples in the examples folder.
