
**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./writeup-images/non-vehicle-and-vehicle.png
[image2]: ./writeup-images/hog-examples.png
[image3]: ./writeup-images/accuracy.png
[image4]: ./writeup-images/find-car.png
[image5]: ./writeup-images/find-car-2.png

[image6]: ./examples/sliding_window.jpg
[image7]: ./examples/bboxes_and_heat.png
[image8]: ./examples/labels_map.png
[image9]: ./examples/output_bboxes.png



[video1]: ./project_video.mp4




### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in IPython notebook [here](https://github.com/BingbingLai/carnd-project-5/blob/master/object_detection_project.ipynb) @ `In [228]` :

- `get_hog_features` and,
-  compute binned color features: `bin_spatial`
-  compute color histogram features: `color_hist`

- started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]


- explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). 

the following image shows using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.
tried various combinations of parameters:

- colorspace: tried the following and 'YUV' and 'YCrCb' stand out
	- 'HSV'
	- 'LUV'
	- 'HLS'
	- 'YUV'
	- 'YCrCb'
	- 'RGB'
	
- 'orient' = 9 perform best  

- experimented hog channel on 'ALL', 0, 1, 2, found the 'ALL' got the higher accuracy. 



#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

check code @ `In [230]` in this [notebook](https://github.com/BingbingLai/carnd-project-5/blob/master/object_detection_project.ipynb):

- used StandardScaler and transform function to normalize pre-defined features
- used train_test_split function to randomize the training data and testing data
- fed the training data to SVM, then prediction accuracy is 0.97
- ![alt text][image3]




### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

- check code from the same notebook @ `In [232]`
- used windows scale 64 and 64 and overlap 0.5
- also use another sliding window techinique `find_car` @ `In [270]`

![alt text][image5]


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
![alt text][image5]

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:



![alt text][image7]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

- had hard time implementing it:
- each training takes >5 minutes, slow feedback loop
- wasted tons of hours debugging stupid issues
- shadowing was hard to detect
- requires a lot of parameter tuning

