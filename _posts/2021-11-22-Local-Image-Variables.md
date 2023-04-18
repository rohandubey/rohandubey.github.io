---
layout: post
title: Unveiling the Power of Local Image Variables and Key Points
---

> As an AI enthusiast, I am always fascinated by how machines can recognize objects and patterns in images. One critical aspect of image recognition is identifying key points or landmarks in an image. These key points can be detected by analyzing local image variables, which describe a small region of an image. In this blog, I'll introduce three popular techniques that utilize local image variables: SIFT, SURF, and ORB.

Image recognition and object detection are critical fields in computer vision, with numerous applications in fields like self-driving cars, robotics, and security systems. To achieve accurate object detection, image recognition algorithms use various local image variables and key points. Three popular techniques that use local image variables to identify key points are SIFT, SURF, and ORB.

<definition>
***Local image variables** refer to image features or attributes that describe a small region of an image. These variables are used to identify key points or landmarks in the image, which can be used for object detection, tracking, or recognition. Some common local image variables are:
</definition>
* **Edges**: Edges refer to the boundaries between two regions in an image with different intensities. They are often used to detect corners, which are key points in the image.
* **Corners**: Corners are points in the image where there is a significant change in intensity in more than one direction. Corners are often used as landmarks because they are stable under transformations like rotation and scale.
* **Blobs**: Blobs refer to regions in the image where the intensity values are higher than the surrounding pixels. They are often used to detect objects in the image.
## SIFT (Scale-Invariant Feature Transform)

SIFT is a popular computer vision algorithm that identifies key points in an image by analyzing the local image variables. SIFT is known for its robustness to scale, rotation, and illumination changes, making it suitable for various types of images. Here's a brief overview of how SIFT works:

* **Scale-space extrema detection**: SIFT analyzes the image at different scales to identify the extrema, or points of maximum and minimum intensity.
* **Keypoint localization**: SIFT then filters out low contrast points and edge responses and localizes the keypoints based on the scale and orientation.
* **Orientation assignment**: SIFT assigns a dominant orientation to each keypoint based on the local image gradient directions.
* **Descriptor generation**: SIFT generates a descriptor for each keypoint based on the local image gradient magnitudes and directions within a specific region.

SIFT has been proven effective in numerous applications like object recognition, image stitching, and image retrieval.
## SURF (Speeded-Up Robust Features)
SURF is a modification of SIFT that is designed to be faster and more efficient. It achieves this by using a fast Hessian-based detection of the extrema, which reduces the computation time significantly. The steps involved in SURF are similar to SIFT:

* **Scale-space extrema detection**: SURF uses a fast Hessian-based detector to identify the extrema.
* **Keypoint localization**: SURF localizes the keypoints based on the scale and orientation.
* **Orientation assignment**: SURF assigns a dominant orientation to each keypoint based on the Haar wavelet response.
* **Descriptor generation**: SURF generates a descriptor for each keypoint based on the Haar wavelet responses in a specific region.

SURF is faster than SIFT but may not perform as well in some scenarios.

# ORB (Oriented FAST and Rotated BRIEF)
ORB is another modification of SIFT that is designed to be faster and more efficient. ORB achieves this by using a fast feature detection algorithm called FAST and a binary descriptor called BRIEF. Here's how ORB works:

* **Keypoint detection**: ORB uses the FAST algorithm to detect keypoints.
* **Orientation assignment**: ORB assigns a dominant orientation to each keypoint.
* **Descriptor generation**: ORB generates a binary descriptor for each keypoint based on a pattern of intensity comparisons between pairs of pixels in a specific region around the keypoint.

ORB is faster and more efficient than both SIFT and SURF, making it a popular choice for real-time applications like robotics and augmented reality.

## Custom Descriptors
While SIFT, SURF, and ORB are popular techniques for local feature detection and description, they may not always be the best choice for every scenario. Sometimes, we may need to create our own custom descriptors based on the specific requirements of our project.

As an example, I have created two custom descriptor scripts that can be used for finding the closest similar-intensity local features and the farthest points with similar intensity. The first script is useful for finding local features that are similar in intensity, which may be useful for detecting objects that have similar textures. The second script is useful for finding local features that are dissimilar in intensity, which may be useful for detecting boundaries between different objects in the image.

### <blue>Script 1: Closest Similar-Intensity Local Features</blue>
```python
import cv2
# Load the image
image = cv2.imread('image.jpg')
# Convert the image to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
# Create a SIFT object
sift = cv2.xfeatures2d.SIFT_create()
# Detect keypoints and compute descriptors
keypoints, descriptors = sift.detectAndCompute(gray, None)
# Create a flann matcher object
matcher = cv2.FlannBasedMatcher()
# Match the descriptors to find similar features
matches = matcher.knnMatch(descriptors, descriptors, k=2)
# Create a list of good matches
good_matches = []
for m, n in matches:
    if m.distance < 0.7 * n.distance:
        good_matches.append(m)
# Draw the matches
result = cv2.drawMatches(image, keypoints, image, keypoints, good_matches, None)
# Show the result
cv2.imshow('Result', result)
cv2.waitKey(0)
```
Below is an interactive demo in JavaScript for the code above, but with a twist. Instead of using SIFT, I have created a custom feature extractor that uses intensity as a measure to extract the closest three points of interest based on the point clicked. The <green>selected point is marked in green</green> and <red>the three closest points are marked in red</red>. Enjoy!
<iframe src="https://preview.p5js.org/rohny00/embed/UczBsAA5C"></iframe>

### <blue>Script 2: Farthest Similar-Intensity Local Features</blue>
```python
import cv2
# Load the image
image = cv2.imread('image.jpg')
# Convert the image to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
# Create a SIFT object
sift = cv2.xfeatures2d.SIFT_create()
# Detect keypoints and compute descriptors
keypoints, descriptors = sift.detectAndCompute(gray, None)
# Compute the distances between all descriptors
distances = cv2.norm(descriptors[:, None] - descriptors, axis=2)
# Set the diagonal values to a high value to exclude them from the minimum
np.fill_diagonal(distances, 999999)
# Find the indices of the farthest points with similar intensity
indices = np.argmin(distances, axis=1)
# Create a list of keypoints for the farthest points with similar intensity
farthest_points = []
for i in range(len(keypoints)):
    if distances[i][indices[i]] < threshold:
        farthest_points.append(keypoints[i])
# Draw the farthest points with similar intensity
result = cv2.drawKeypoints(image, farthest_points, None, color=(0, 255, 0), flags=0)
# Show the result
cv2.imshow('Result', result)
cv2.waitKey(0)
```
Below is an interactive demo in JavaScript for the code above, but with a twist. Instead of using SIFT, I have created a custom feature extractor that uses intensity as a measure to extract the farthest two points of interest based on the point clicked. The <green>selected point is marked in green</green> and <red>the two farthest points are marked in red</red>. Enjoy!
<iframe src="https://preview.p5js.org/rohny00/embed/_gPWnkzAi"></iframe>

## Conclusion

In conclusion, local image descriptors play an essential role in computer vision tasks such as object recognition, image retrieval, and matching. SIFT, SURF, and ORB are some of the most widely used feature extractors for local image descriptors due to their robustness and efficiency in identifying distinctive local features.

However, there may be situations where a custom feature extractor based on specific needs or requirements may be more suitable. As shown in the two custom feature extractor scripts, a feature extractor that uses intensity as a measure can be created to extract either the closest or farthest points of interest based on a clicked point.

Overall, understanding the characteristics and capabilities of different local image descriptors can be invaluable in developing computer vision systems that are both efficient and effective in a variety of applications.




<definition>
An auto-direction (or <def>eigendirection</def>) is a direction that does not change when we apply a given linear transformation.
Vectors laying on such direction will (at most) change their magnitude by the corresponding auto-value (<def>eigenvalue</def>) scalar constant.
</definition>

In the interactive demo below, choose two auto-directions by clicking on two (possibly consecutive) <red>red dots</red>.
Drag around the <green>chosen eigenvectors</green> to generate arbitrary linear transformations, mapping <blue>blue points</blue> to <red>red points</red>.
Notice how the eigenvectors will solely move along their auto-direction, effectively changing only their magnitude.

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "{{ page.url | absolute_url }}";
this.page.identifier = "{{ page.url | absolute_url }}";
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://reedn.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
	
> I am still studying about the local image feature, if you have any suggestion  or comment please let me know!
