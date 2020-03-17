# Face Detection Using OpenCV
First of all we need to install OpenCV. We can install it using pip:
                                              pip install opencv-python
We need to download the trained classifier XML file (haarcascade_frontalface_default.xml), which is available in OpenCv’s GitHub repository and save it to working directory.
Cascade classifiers are trained on a few hundred sample images of image that contain the object we want to detect, and other images that do not contain those images.
There is an algorithm, called Viola–Jones object detection framework, that includes all the steps required for live face detection:
•	Haar Feature Selection, features derived from Haar wavelets
•	Create integral image
•	Adaboost Training
•	Cascading Classifiers

There are some common features that we find on most common human faces:
•	a dark eye region compared to upper-cheeks
•	a bright nose bridge region compared to the eyes
•	some specific location of eyes, mouth, nose…

The rectangle feature value is simply computed by summing the pixels in the black area and subtracting the pixels in the white area.
Then, we apply this rectangle as a convolutional kernel, over our whole image. In order to be exhaustive, we should apply all possible dimensions and positions of each kernel. A simple 24*24 images would typically result in over 160000 features, each made of a sum/subtraction of pixels values. It would computationally be impossible for live face detection. So, we speed up this process by:
•	once the good region has been identified by a rectangle, it is useless to run the window over a completely different region of the image. This can be achieved by Adaboost.
•	compute the rectangle features using the integral image principle, which is way faster.

Given a set of labelled training images (positive or negative), Adaboost is used to:
•	select a small set of features
•	and train the classifier

In an image, most of the image is a non-face region. Giving equal importance to each region of the image makes no sense.
The key idea is to reject sub-windows that do not contain faces while identifying regions that do.
Since the task is to identify properly the face, we want to minimize the false negative rate, i.e. the sub-windows that contain a face and have not been identified as such.
A series of classifiers are applied to every sub-window. These classifiers are simple decision trees:
•	if the first classifier is positive, we move on to the second
•	if the second classifier is positive, we move on to the third
•	…

Any negative result at some point leads to a rejection of the sub-window as potentially containing a face. The initial classifier eliminates most negative examples at a low computational cost, and the following classifiers eliminate additional negative examples but require more computational effort.


