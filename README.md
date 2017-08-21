# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


This project goal is to write a software pipeline to detect vehicles in a video. Also this document will describe in details the algorithms and technics used in the pipeline.

The Project
---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./images/image1.jpg
[image2]: ./images/image2.png
[image3]: ./images/image3.png
[image4]: ./images/image4.png

[video1]: ./out.mp4

## Pipeline
The pipeline consists of two independent parts: lanes detection part and vehicles detection part.
This document describes only vehicles detection part. Another part was described in previous project.

### 1. The software uses Linear SVM classifier. 
The archives with images are located at *train_data* directory and should be unpacked there.
Interesting thing is that the classifier requires not only *vehicle* images to train but also *non-vehicle*. There are almost 9000 images of both *vehicle* and *non-vehicle* type:
```
Cars:  8792 , non-cars:  8968
```

### 2. Classifier features
The classifier can't process images directly. It requires *features vector* at input what is derived from the image.
There are different schemes for retrieving features vector from the image:
1. Spatial Binning of Color
![alt text][image1]

Actually it is simple scale down to desired size. The smaller is size the wider is the range of matching samples.

2. Histograms of Color
Histogram of selected channels in selected color space.
![alt text][image2]

The length of this features vector is determined by bins count.

3. Histogram of Oriented Gradient (HOG) features
Good method that can describe the shape of an object.
![alt text][image3]

The key parameters are: orientations, pixels per cell, cells per block

4. Color space
Here is are some color spaces
![alt text][image4]

I selected the following key parameters for features extraction:
```
color_space = 'HSV' # Can be RGB, HSV, LUV, HLS, YUV, YCrCb
orient = 9  # HOG orientations
pix_per_cell = 8 # HOG pixels per cell
cell_per_block = 2 # HOG cells per block
hog_channel = 2 # Can be 0, 1, 2, or "ALL"
spatial_size = (24, 24) # Spatial binning dimensions
hist_bins = 16    # Number of histogram bins
spatial_feat = True # Spatial features on or off
hist_feat = True # Histogram features on or off
hog_feat = True # HOG features on or off
```
