# Machine learning-based prediction of centriole orientation in planarians.  
'Planarians' is a pipeline that allows researchers to quantitatively relate ciliary defects to centriole polarity via automatic determination of centriole orientation.

# Installation
1. Clone this repository to your local environment
```
git clone <HTTPS address>
```
Windows users will need a Ubuntu sub-system downloadable via this link: https://ubuntu.com/wsl or simply install Anaconda to access Jupyter.

2. Install all required packages using the requirements text file:
```
pip install -r /path/to/requirements.txt
``` 

# Usage / How it works
**Experimental requirements**    
Immunofluorescence images of anti-rootletin at 63/100x objective (100x for best results)  

**Basic principle**  
The code extracts the edges and the midline of a worm (using the DIC 10x image), then reformats them into an appropriate format.   
Centrioles are recognized (Using a mix of Difference of Gaussian and an equivalent of Find Maxima function from ImageJ) and Extracted from the 63/100x images.  
The angle of each centriole is predicted using a Neural Network (CNN adapted from VGG), so far only classification mode is trained (72 classes).    
Those angles are then "compensated" depending on the local orientation of the worm.  
Relative distance of the centriole on the anteroposterior and Medio-Lateral axes is also computed.    
Results are summarized in a customizable graph.  

**Differents step of the code**  
  1/ Extraction of the edges and midline of the worm:  
 So far (6 May 2021), this step needs to be performed manually with ImageJ. First, the code for the segmentation needs to be improved. Second, it's quite easy to segment the external edges of the worm. But for _Schmidtea Mediterranea_ the edges do not fit with the Region of interest where the centrioles are present.  
 Tests have to be performed for _Macrostomum Lignano_.  
   
 => Manual extraction of the edge and midline:  
With ImageJ, open the 63/100x image and draw the midline with _segmented line_ tool. Then _smooth_ the drawn line with the _fit spline_ ImageJ command (Edit -> selection). Save the x,y coordinates in a txt file. The same must be done for the worm's edges.  
   
 => Automatic extraction of the edge/midline:  
 Not implemented yet.
   
  2/ Get centriole coordinates:  
       A method that will automatically find the coordinates of as many as possible centrioles in the planarian worm 

  3/ Get the angle of the centriole
     From the coordinates obtained in 1/, extract the image and compute an angle  
     
     To do so a CNN approach is used
     
     ADVANCEMENT : Training of the model is under progress. Since the dataset is not completeley normalized, i might have trouble to get valuable output


  4/ Get Edge and Midline characteristics:
      a/ Automatic detection of the edge and the midline
      
      ADVANCEMENT: For the moment, the detection will be performed manually
      
      
      b/ Characterisation of the edge and the midline
      Once Edge and Midline are characterized, reduce the number of segment to facilitate computation
      
      The code for this part is located in ./tools/Midline_Edge_Reformater.ipynb
      
      ADVANCEMENT: Code is supposed to be finished, but tests need to be performed, especially with Macrostomum.
      
  5/ Get 'Real' centriole characteristics
     -> centriole angle 
     -> relative lateral position (0 = midline, <0 : on the right side of the worm, >0 : on the left side of the worm)
     -> relative longitudinal position (0 = tail, 1 = head)
     
     
     ADVANCEMENT: see note below.
     
     NOTE: I need to think: how will I get the data from the CNN to know how I will inject them in thi script
     
     So far, this is written in ./Centriole_Compensation.ipynb. 
     
     
     
  6/ Summarize the worm's data: 
  - CSV file as a list of lists, each containing data for individual reoriented centrioles (described in Main_v3, end of the script)
  - 5 'GRAPH' plots: worm is segmented into 5 anteroposterior segments. For each segment we segment the average angle (moving average) of the centrioles according to its location. The x axis shows its medio-lateral location (0 = midline) while the y axis represents the angle. Each point is a centriole. The dark area represents the circular standard deviation (CSTD).

**Before running Main_v3.ipynb**
- For manual midline and edge selection: After pasting the coordinates on the .xlsm file, go to the Graph sheet and ensure that the plotted worm looks normal. If not, redo the manual segmentation.
- Ensure that you have placed your files to be analysed in the folder to_analyse/. In the abscence of this folder, simply create it and paste your files inside.
- Ensure the that the weights of the neural network exist in: weights/VGG_schmidtea_weight_classification.pth. If not, you can find it on the Schmidtea drive.

# TO DO LIST:  


-----------------------------------------------------------------

## CNN

The angle predictions are good with Schmidtea, a worm that was used to train the model. However the predictions are completely off with _Macrostomum_. Perhaps the model needs to be trained with new manual _Macrostomum_ data.

### DataBase Loader preparation 

I could pass a function to get the images from json in the Dataset class.

I am not certain it's optimal.
  
  
  
### Validation  
  
  
  
--------------------------------------------------------------------
  
  
# Centriole isolation 


## How to detect centrioles in images

Jochen Approach: 

##Step 1: Apply difference of Gaussian with sigma1 = 0.5 and sigma2 = 5

Sonka, 2007
https://en.wikipedia.org/wiki/Difference_of_Gaussians
https://en.wikipedia.org/wiki/Gaussian_blur

-> In ImageJ perform Gaussian blur (Process -> Filters -> Gaussian Blur) 
     - on the original image with a radius of 0.5 (sigma1)
     - on the original image with a radius of 5 (sigma2)

-> Then subtract image created with sigma1 with image created with sigma2 (Process -> Image Calculator...)



## Step 2: Perform an adaptive thresholding approach

Otsu, 1979
Otsu_treshold -> implemented in ImageJ: 
https://imagej.nih.gov/ij/plugins/otsu-thresholding.html
download .JAR file and put it in the plugin folder of Fiji

To apply it
Plugins -> Filters -> Otsu Tresholding

Then I should perform filters to remove objects that are too big and too small.

  
  
  
---------------------------------------------------------------------
  
## Centriole reorientation  
   
   
   
--------------------

LECTURE 

https://scipy-lectures.org/advanced/image_processing/index.html#basic-image

https://scipy-lectures.org/packages/scikit-image/index.html

https://scikit-image.org/docs/0.13.x/api/skimage.morphology.html#skimage.morphology.remove_small_objects
   
   

   
https://www.earthdatascience.org/courses/intro-to-earth-data-science/open-reproducible-science/jupyter-python/jupyter-notebook-shortcuts/

https://pylightxl.readthedocs.io/en/latest/quickstart.html
