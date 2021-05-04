# Machine learning-based prediction of centriole orientation in planarians.  
The aim of this project is to automatically determine the orientation of centrioles with respect to a worm's anteroposterior axis, allowing biologists to quantitatively relate ciliary defects to centriole polarity.

# Installation
1. Clone this repository to your local environment
```
git clone <HTTPS address>
```
Windows users will need a Ubuntu sub-system downloadable via this link: https://ubuntu.com/wsl 

2. Install all required packages using the requirements text file:
```
pip install -r /path/to/requirements.txt
``` 
# Usage
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
  1/ Extraction of the edges and midline of a worm:  
 So far (17 November 2020), this step need to be performed manually. First, the code for the segmentation needs to be improved. Second, it's quite easy to segment the external edges of the worm. But for _Schmidtea Mediterranea_ the edges do not fit with the Region of interest where the centrioles are present.  
 Tests have to be performed for _Macrostomum Lignano_.  
   
 => Manual extraction of the edge and midline:  
With ImageJ, open the 63/100x image and draw the midline with _segmented line_ tool. Then _smooth_ the drawn line with the _fit spline_ ImageJ command. Save the x,y coordinates in a txt file. The same must be done for the worm's edges.  
   
 => Automatic extraction of the edge/midline:  
 Not implemented yet.
   
  2/ Get centriole coordinates:  
       A method that will automatically find the coordinates of as many as possible centrioles in the planarian worm 

  2/ Get the angle of the centriole
     From the coordinate obtain in 1/, extract the image and compute an angle  
     
     To do so a CNN approach is used
     
     ADVANCEMENT : Training of the model is under progress. Since the dataset is not completeley normalized, i might have trouble to get valuable output


  3/ Get Edge and Midline characteristic:
      a/ Automatic detection of the edge and the midline
      
      ADVANCEMENT: For the moment, the detection will be performed manually
      
      
      b/ Characterisation of the edge and the midline
      Once Edge and Midline are characterized, reduce the number of segment to facilitate computation
      
      The code for this part is located in ./tools/Midline_Edge_Reformater.ipynb
      
      ADVANCEMENT: Code is suppose to be over, but test need to be performed
      
  4/ Get 'Real' centriole characteristic
     -> centriole angle 
     -> relative lateral position (0 = midline, <0 : on the right side of the worm, >0 : on the left side of the worm)
     -> relative longitudinal position (0 = tail, 1 = head)
     
     
     ADVANCEMENT: see note below.
     
     NOTE: I need to think: how i will get the data from the CNN to know how i will inject them in thi script
     
     So far, this is write in ./Centriole_Compensation.ipynb.
     In this notebook, 
     
     
     
  5/ Summarized the data for a worm
 





# TO DO LIST:  


-----------------------------------------------------------------

## POUR LE CNN
  
### Preparation d'un DataBase Loader  

Je peux peut-etre passser la function qui permet de récuperer les images depuis les json dans la class Dataset.

Je suis pas certain que ce soit opti.
  
  
  
### Validation  

Pas obligatoire de faire cette étape mais tester mon pipeline d'analyse entier
  
  
  
  
---------------------------------------------------------------------
  
  
# Pour l'isolation des centrioles  


## How to detect centrioles in images

Jochen Approach: 

##Step 1: Apply difference of Gaussian with sigma1 = 0.5 and sigma2 = 5

Sonka, 2007
https://en.wikipedia.org/wiki/Difference_of_Gaussians
https://en.wikipedia.org/wiki/Gaussian_blur

-> in ImageJ perform Gaussian blur (Process -> Filters -> Gaussian Blur) 
# on the original image with a radius of 0.5 (sigma1)
# on the original image with a radius of 5 (sigma2)

#Then subtract image created sigma1 with images created with sigma2 (Process -> Image Calculator...)



## Step 2: Perform an adaptive thresholding approach

Otsu, 1979
Otsu_treshold -> implementé dans ImageJ: 
https://imagej.nih.gov/ij/plugins/otsu-thresholding.html
download .JAR file and put it in the plugin folder of Fiji

To apply it
Plugins -> Filters -> Otsu Tresholding

Then I should perform filters to remove too big and too small objects

  
  
  
---------------------------------------------------------------------
  
## Pour la rectification de l'angle des centrioles  
   
   
   
--------------------

LECTURE 

https://scipy-lectures.org/advanced/image_processing/index.html#basic-image

https://scipy-lectures.org/packages/scikit-image/index.html

https://scikit-image.org/docs/0.13.x/api/skimage.morphology.html#skimage.morphology.remove_small_objects
   
   
   
   
https://www.earthdatascience.org/courses/intro-to-earth-data-science/open-reproducible-science/jupyter-python/jupyter-notebook-shortcuts/

https://pylightxl.readthedocs.io/en/latest/quickstart.html
