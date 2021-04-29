# Instance Search using CNN Transfer Learning & Color Histograms

## Introduction ##

The two instance search methods implemented in this project are color histogram and CNN. By cropping all query images and a subset of target images according to its corresponding bounding box coordinates, it is believed that the cropped images could help boost the instance search task by better focusing on the target instance. The followings are the implementation of the two methods applied on the search of the 10 example queries and the 20 testing queries from the given 5000 image dataset.

## Image cropping ##

All query and target images with bounding box coordinates provided are cropped first. Take two query images for instance, the below images on the left are the original ones while the cropped ones appear one the right hand side.

