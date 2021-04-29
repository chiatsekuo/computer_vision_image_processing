# Instance Search using CNN Transfer Learning & Color Histograms

## Introduction ##

The two instance search methods implemented in this project are color histogram and CNN. By cropping all query images and a subset of target images according to its corresponding bounding box coordinates, it is believed that the cropped images could help boost the instance search task by better focusing on the target instance. The followings are the implementation of the two methods applied on the search of the 10 example queries and the 20 testing queries from the given 5000 image dataset.

## Image cropping ##

All query and target images with bounding box coordinates provided are cropped first. Take two query images for instance, the below images on the left are the original ones while the cropped ones appear one the right hand side.

![cropped_imgs](/instance_search/cropped_imgs.PNG)

## Methods & Results ##

###	3.1 Color Histogram ###
This histogram matching algorithm utilizes the frequency of appearance of each color of the histogram within the image. If the image contains a specific set of colors, the frequency count of that color bin will be high. The histogram includes 256 bins. If more bins are used, a longer computational time would require. Each histogram is normalized to make the later distance comparison easier. Here, I used the intersection method (cv.HISTCMP_INTERSECT) to evaluate whether two histograms are close to each other in terms of color distribution.

#### 3.1.1 Result of example query & Top 10 retrieved images ####

The below images are the top 10 query results of each example query using color histogram comparison. Each row from the left to right represents the closest image to the least closest image. The first row from the top to the bottom corresponds to the indexing of the query image.

We can observe that the color histogram calculating with RGB values does not perform well on instance search for some reasons. First, the color histogram might record the background color of the images which may not be important to describe the target object in the images. Take the first query image for example, the color histogram of that image has recorded a high frequency of blue colors which represent the sky. However, the target object is Eiffel tower. This may trick the very color histogram to search for images that have a high proportion of blues in them. And this may yield a low accuracy value eventually.

According to the metric function provided, the comparison of the result list and the ground truth list is as shown below:

Average Precision of Q1: 0.0048<br>
Average Precision of Q2: 0.0070<br>
Average Precision of Q3: 0.2738<br>
Average Precision of Q4: 0.0081<br>
Average Precision of Q5: 0.0051<br>
Average Precision of Q6: 0.0843<br>
Average Precision of Q7: 0.0034<br>
Average Precision of Q8: 0.0512<br>
Average Precision of Q9: 0.0137<br>
Average Precision of Q10: 0.0052<br>
Mean Average Precision: **0.045654**<br>
