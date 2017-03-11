# object-detection-with-svm-and-opencv

## Feature Selection and tuning

skimage hog function is used to extract the HOG features in cell 3 of the notebook (Vehicle-Detection-SVM.ipynb). Apart from HOG features color histogram and raw color features are also used

HOG features for all the 3 channels in HSV color space are extracted. These were selected after trial and error. Refer cell 2 for the colorspaces tested. Apart from these I used orientation as 9 with 8 pixels per cell and 2 cells per block which gave best results after testing a few combinations. 

Refer cell 8 in the notebook for the parameters used

## Classifier selection and tuning

First, different classfiers like SVC, decision tree were tested and svc was chosen because it gave better results with default configurations. Refer cell 5,10,11.

Played around with different parameters like kernel('linear', 'rbf'), C, gamma, probability etc. linear kernel was chosen because it was fastest without sizeable loss in performance.

Initially only the HOG features were used which gave good performance but also had a lot of false positives. Using the histogram and color features helped reduce the false positives. Threasholding further improved the false possitive situation especially in the videos.

## Finding the cars in the image

A sliding window approach has been implemented, where overlapping tiles in each test image are classified as vehicle or non-vehicle. Some justification has been given for the particular implementation chosen.

To find the cars sliding window technique is used in the cell 7 of the notebook. Ititially all the processing was done for each window but as noted in the class the hog features were extracted once to improve performance.

Also the processing is only done for the road part of the image by cropping out the unwanted parts before processing

## Training a Classifier

Cell 9 of the notebook sets the training and testing data. Here we read the images from the disk and extract color feature, histogram features and HOG features and bag them all in cell using the wrapper function in cell 4 of the notebook.

After this the features are scaled using sklearn standardscaler and split in test and training set in a 80-20 ratio.

The Classifier is then training with this data in cell 10 and finally its tested on the test data in cell 11.

## Video Processing

The detections from the video frames are added to a queue and the mean of the last 10 frames is used to get the last detections. The mean is thresholded to get the good detections. This remove the false positives and enhances the windows with car.