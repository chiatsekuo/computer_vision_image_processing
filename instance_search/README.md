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

![Top 10 retrieved images by histogram](/instance_search/hist_query.png)

### 3.2 CNN ###

For the Convolutional Neural Network approach, I performed a transfer learning technique on ResNet. Benefiting from the pre-trained network’s weights, the image matching time could be shortened exponentially than training from scratch. By extracting features of all images including the query images and the target images from the Image folder, the similarities between the query image and each target image can be retrieved by calculating the distance between them. Here, I simply used Euclidean distance as the distance metric.

##### 3.2.1	Result of example query & Top 10 retrieved images ####

![Top 10 retrieved images by cnn](/instance_search/cnn_query.png)

(Note: The retrieved images of each query image are presented in the same row. From top to bottom: first query image to the last one.)

The average precision and the mean average precision of the instance search on 10 example query images are as shown below: 

Average Precision of Q1: 0.6899<br>
Average Precision of Q2: 0.4248<br>
Average Precision of Q3: 0.4228<br>
Average Precision of Q4: 0.6346<br>
Average Precision of Q5: 0.9774<br>
Average Precision of Q6: 0.4681<br>
Average Precision of Q7: 0.1824<br>
Average Precision of Q8: 0.1251<br>
Average Precision of Q9: 0.8453<br>
Average Precision of Q10: 0.6915<br>
Mean Average Precision: **0.546182**<br>

## Discussion & Conclusion ##

From the color histogram algorithm, I found that it tends to take the background scenes of each image into consideration as matching points or shared similar color distributions. If all target images were cropped out of the original images, it would be easier for algorithms to compare the shared features. Another discovery related to this problem is that even though the query image has similar color distribution with the retrieved target image, the object in both images are unrelated. In this situation, the color distribution may not work very well. Instead, a broader view of the images like the styles of edges should be considered to let the algorithm have a holistic view on the shape of both the query images and the target images. 

Therefore, I tried to implement convolutional neural networks to solve this issue. Since it not only detects the color distribution and edges, but also other important hidden features that the human eye may not discern. I also took advantage of transfer learning to extract image features. It not only saves time but also could bring a higher chance of success in instance search considering the fact that it has been trained on millions of real-world images with thousands of classes. From the results, we can see that the CNN solution yields way higher results than the color histogram. Though the mean accuracy may not be considered high, some cases (Q5 & Q9) have shown high accuracy in searching similar images.

I believe the color histogram model can be improved by increasing the color bins of the color histogram method and trying different distance metrics. As for the CNN transfer learning, different pre-trained models and deeper networks should be further tested to evaluate the potential of instance search among images.
