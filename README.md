# Star-Image-Quality-Control
A Practical Application of Machine Learning Models in Image Classification


In astrophotography, astronomers take hundreds of
images of the same target (or “sub-exposures”), and through a
process called stacking, these images are combined together to
produce the final image, which will have more signals and less
noise. However, due to various external factors, some images can
suffer from bad quality. The quality of each image can be
measured by examining the roundness of the stars. A good image
will have stars that are round and pinpoint. A bad image will
have stars that are bloated, elongated, or large in size. The
process of examining each individual image and filtering out bad
images is a time consuming one. In this project, we will use the
Python programming language to preprocess the star images,
separate them into training and testing data, create and compare
different models and validate the accuracy of the model and
choose the best model.
Keyword: SVM, Hu Moments, Image Classification, KNN,
Random Forest, Machine Learning, Astronomical Images.

1. Dataset

Taking astronomical images of the night sky requires
extremely long exposure time in order to collect enough light
photons coming from nebulae and galaxies. Due to the
rotation of the earth around itself, astronomers have to use
special imaging equipment to counter this rotation and keep
the imaging frame perfectly still during the entire exposure
duration, which can last several minutes. Even the slightest
vibration coming from the ground or the winds, or the
imperfections in the equipment can ruin the quality of the
image. Unwanted movements of the imaging frame can often
be assessed via the shape of the stars (the “light trail” effect,
as often referred to as in photography). When that happens,
stars, which are point sources of light, appear large or
elongated in the image, or have a strange shape. A good
image should have stars that appear as small, round circles
with a smooth gradient, brightest at their core and dimmer
towards their edge. Below is an example:

![Screen Shot 2022-05-26 at 3 21 30 PM](https://user-images.githubusercontent.com/58579913/170589196-28701e6e-ebfb-49c7-a126-d0f761b31da3.png)

  The process of selecting good star images from bad ones can
  be a very futile exercise for humans, as the final product
  usually comprises hundreds or thousands of sub-exposures.
  Therefore, it would be beneficial to astronomers if a tool can
  be developed to help them finish this exercise with ease.

2. Data augmentation

The data size is relatively small, the number of good images
is 136, and the bad one is 95, because those images were
taken by one of our team members and manually labeled. So,
to increase the accuracy of the models, we implemented three
different data augmentations. For the shape of images is
square, we could rotate the images with three different
degrees, 90, 180 and 270 without information loss. In
addition, Gaussian and poisson noise were added to original
data. However, some augmented data with gaussian noise
made an error. Because, in some images, many white dots
were created, which leads to error during the feature
extraction method. Because our core features extraction
method is Hu moment and the contour of image is an
important factor, so when many white dots appeared on the
images, the contour became completely changed which made
an error. However, white dots with low brightness did not
affect the feature extraction due to the brightness threshold.
So, considering the error, the multipliers of gaussian and
poisson noises were adjusted to the level that does not cause
error.


![Screen Shot 2022-05-26 at 3 21 59 PM](https://user-images.githubusercontent.com/58579913/170589241-c8bdfd19-e4ca-48b5-b707-62fe5cbdfad2.png)

3.Feature Extraction

As mentioned in the introduction, in the context of our
particular problem where we need to separate the “bad” stars
from the “good” stars, the main feature that should be
considered is the shape of the star, regardless of its relative
position, size, or orientation. For that reason, Hu invariant
moments are believed to be the perfect tool for this.
However, the first thing we need to do is to separate the stars
from the sky background. Visually, we can clearly see which
part of the image belongs to the star, and which part of the
image belongs to the background, based on the brightness of
pixels. For the computer to do this, we need to employ the
threshold function provided in the cv2 library. The idea is
quite simple: choose a value as the threshold and compare
each pixel’s brightness with this threshold. If the pixel is
darker than the threshold, make it black; and if the pixel is
brighter than the threshold, make it white. After this
operation, we will get a black and white image where the
white area shows the general shape of the original star. Below
is an example:

![Screen Shot 2022-05-26 at 3 22 13 PM](https://user-images.githubusercontent.com/58579913/170589261-c1ad902f-e36b-40e1-ab35-8a3565e3037e.png)


![Screen Shot 2022-05-26 at 3 22 25 PM](https://user-images.githubusercontent.com/58579913/170589274-9cd6b9e6-ca0b-42f6-84de-7962c83b8d01.png)

The contour is simply an array of (x, y) coordinate pairs that
make up all the points along the edge of the shape.

Next, we need to calculate the moments of the contour. In
simple terms, a moment of an image is a function that
captures the weighted average of the intensities of the pixels
in an image. The method of weighting is chosen in such a way
that gives the moment some useful properties. More
information can be found here:
https://en.wikipedia.org/wiki/Image_moment. In the cv2
library, the cv2.moments() function provides an array of
moments of any given shape:

Finally, we calculate Hu moment invariants, which are
functions of all the moments up to third order of the image:

![Screen Shot 2022-05-26 at 3 22 53 PM](https://user-images.githubusercontent.com/58579913/170589306-4da2370f-34ab-4eff-b74a-2de668da8cd9.png)

4. EDA and visualization
5. Model selection and test models.
