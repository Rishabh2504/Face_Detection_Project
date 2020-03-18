# Face Detection Using OpenCV
First of all we need to install OpenCV. We can install it using pip:<br />
                                              ```
                                                        pip install opencv-python
                                              ```<br />
We need to download the trained classifier XML file (haarcascade_frontalface_default.xml), which is available in OpenCv’s GitHub repository and save it to working directory.<br />
Cascade classifiers are trained on a few hundred sample images of image that contain the object we want to detect, and other images that do not contain those images.<br />
There is an algorithm, called Viola–Jones object detection framework, that includes all the steps required for live face detection:<br />
•	Haar Feature Selection, features derived from Haar wavelets<br />
•	Create integral image<br />
•	Adaboost Training<br />
•	Cascading Classifiers<br />

There are some common features that we find on most common human faces:<br />
•	a dark eye region compared to upper-cheeks<br />
•	a bright nose bridge region compared to the eyes<br />
•	some specific location of eyes, mouth, nose…<br />

The rectangle feature value is simply computed by summing the pixels in the black area and subtracting the pixels in the white area.
Then, we apply this rectangle as a convolutional kernel, over our whole image. In order to be exhaustive, we should apply all possible dimensions and positions of each kernel. A simple 24*24 images would typically result in over 160000 features, each made of a sum/subtraction of pixels values. It would computationally be impossible for live face detection. So, we speed up this process by:<br />
•	once the good region has been identified by a rectangle, it is useless to run the window over a completely different region of the image. This can be achieved by Adaboost.<br />
•	compute the rectangle features using the integral image principle, which is way faster.<br />

Given a set of labelled training images (positive or negative), Adaboost is used to:<br />
-	select a small set of features
-	and train the classifier

<b>Cascade Classifier:</b><br />
In an image, most of the image is a non-face region. Giving equal importance to each region of the image makes no sense.<br />
The key idea is to reject sub-windows that do not contain faces while identifying regions that do.
Since the task is to identify properly the face, we want to minimize the false negative rate, i.e. the sub-windows that contain a face and have not been identified as such.<br />
A series of classifiers are applied to every sub-window. These classifiers are simple decision trees:<br />
-	if the first classifier is positive, we move on to the second
-	if the second classifier is positive, we move on to the third
-	…

Any negative result at some point leads to a rejection of the sub-window as potentially containing a face. The initial classifier eliminates most negative examples at a low computational cost, and the following classifiers eliminate additional negative examples but require more computational effort.
## In the Code:-
First of all we need to import OpenCV and numpy. 
Next,we need to load the Viola-Jones classifier.<br/>
The faceCascade object has a method detectMultiScale(), which detects objects of different sizes in the input image and the detected objects are returned as a list of rectangles.<br>
Parameters of detectMultiScale():<br />
<ol>
  <li> image - 	Matrix of the type CV_8U containing an image where objects are detected.</li>
  <li> scaleFactor - Parameter specifying how much the image size is reduced at each image scale.</li>
  <li> minNeighbors	Parameter specifying how many neighbors each candidate rectangle should have to retain it.</li>
  <li> flags	- Parameter with the same meaning for an old cascade as in the function cvHaarDetectObjects. It is not used for a new cascade.</li>
</ol>  
The variable faces now contains all the detected faces in the image.To visualize the detections, you need to iterate over all detections and draw rectangles over the detected faces.<br />
OpenCV’s rectangle() draws rectangles over images.<br />
imwrite() Saves an image to a specified file.<br />


