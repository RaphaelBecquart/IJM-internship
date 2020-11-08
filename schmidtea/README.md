### The aim of this code is to analyse automatically the orientation of centriole inside planarians.  


Differents stepas are required:  
  1/ Get centriole coordinate:
      A method that will automatically find the coordinates of as many as possible centriole in a planariane

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
