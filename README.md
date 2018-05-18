## Writeup

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/chess_orginal.jpeg "chess original"
[image2]: ./output_images/chess_undistort.jpeg "chess undistort"
[image3]: ./output_images/road_orginal.jpeg "road original"
[image4]: ./output_images/road_undistort.jpeg "road undistort"
[image5]: ./output_images/thresholding.jpeg "threshold pipline"
[image6]: ./output_images/perspective_transform.jpeg "perspective transform"
[image7]: ./output_images/hls.jpeg "hls"
[image8]: ./output_images/final_combined.jpeg "final_combined"
[image9]: ./output_images/final_roa_applied.jpeg "final_roa_applied"
[image10]: ./output_images/polyfit.jpeg "polyfit"
[image11]: ./output_images/boundries_applied.jpeg "boundries applied"
[image12]: ./output_images/boundries_withtext.jpeg "boundries withtext"
[image13]: ./output_images/histogram.jpeg "histogram"

[video1]: ./project_output.mp4 "Video"
[Final_Submission_Advance_Lane_Finding_Video_Pipeline.ipynb]: ./examples/Final_Submission_Advance_Lane_Finding_Video_Pipeline.ipynb "Final Code"
[Advance_lane_finding_Submission_Test_Images_and_llustration.ipynb]: ./examples/Advance_lane_finding_Submission_Test_Images_and_llustration.ipynb "Test Code"

### Note : In the below discussion I will be using [Advance_lane_finding_Submission_Test_Images_and_llustration.ipynb](./examples/Advance_lane_finding_Submission_Test_Images_and_llustration.ipynb) code file for illustration purpose. 


### The final video pipeline code is in the file [Final_Submission_Advance Lane_Finding_Video_Pipeline.ipynb](./examples/Final_Submission_Advance_Lane_Finding_Video_Pipeline.ipynb)




### Camera Calibration and Undistortion

The code for this step is contained in the first code cell (In [109]) of the IPython notebook located in "./examples/Advance lane finding Submission- Test Images and illustration.ipynb.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

Before undistortion

![alt text][image1]

![alt text][image3]

After applying undistorting

![alt text][image2]

![alt text][image4]

### Color Transform, Gradients and Thresholding

In this step I applied all the gradients and thresholding techniques thought in the course such as sobelx, sobely, magnitude thresholding  and direction thresholding. Below is the visual illustration. Ref code cells 3,4 and 5( In [111]..In[113]) 

![alt text][image5]

I also applied HLS color transform 

![alt text][image7]

Finally I combined both the color transformed image and thresholding binary image to get the below result

![alt text][image8]

Lastly, region of interest is selected to proceed further

![alt text][image9]

### Perspective Transform 

I chose to hardcode the source and destination points in the following manner: Ref code cell 6 (In [114])

The below source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 280, 650      | 0, 700        | 
| 1080, 650     | 1200, 700     |
| 800, 490      | 1200, 100     |
| 500, 490      | 0, 100        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image6]


### Detect Lanes

I used peaks in histogram technique to the detect lanes. 

![alt text][image13]

And then used sliding window to identify the left and right lanes. A 2nd order polynomial is fitted with the points obtained.

![alt text][image10]

Ref code cell (In [129], [128], [116])

### Radius of Curvature and Vehicle Position

Roc and Vehicle position is calculated in the code cell In [117] and [121]

### Boundary Markings with Texts:

The obtained results where converted in to usuable format and the boundaries are plotted in the original image as shown below

![alt text][image11]

Finally the Radius of Curvature and Vehicle Position obtained in the previous step is displayed in the original image. See below

![alt text][image12]


### Pipeline (video)


Here's a [link to my video result](./project_output.mp4)


### Short Comings and Future Enhancements:

My project is will not perform well on the challenge videos as I have hardcoded region of interest and perspective transform. 
The below techniques can be employed to make it more robust. 


* Better technique for noise reduction while applying gradients
* Dynamic way to deduce the source and destination points for Perspective transform
* Sanity check
* Tracking and look a head filter


