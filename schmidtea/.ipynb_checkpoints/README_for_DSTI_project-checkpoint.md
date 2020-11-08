## Project of Cyril Basquin DSTI student in cohort A19

This DSTI project is a feasibility project with the objective to estimate if it will be 'easy' to develop a tool that will automatically detect a specific structure from already acquired images and estimate its orientation. It is the continuation of my Post-doctoral fellow in Biology [1,2].  

Codes or accessible on GitHub: https://github.com/Cyril-Basquin/Schmidtea_CNN  
 
Note: Due to their size and for confidentiality, Dataset and Weights of the model are note store on GitHub.  

### I/ Context of the problem
Schmidtea mediterranea is a flat worm (planarian) which glide on substrate thanks to the coordinated beating of thousands of structures, called cilia, that decorate his ventral epidermis. The length of this animal range from ~200µm to 3cm with a 1:3 ratio (width:length). Cilia are anchored to the worm through a structure called basal body (BB). On each basal body 2 main structures located in opposition (the rootlet and the basal foot are required to orient the beating of the cilia. With a density of ~1 BB/µm², the smaller worms are decorated with a least 15 000 BBs. So far, to study the orientation of the BBs, two strategies were proposed. The first one detects and orients BBs automatically [2]. The second one (that I led), is a manual annotation thousands of basal bodies to get their orientation [1]. The first method gave a good idea of the orientation of the structures at the scale of the animal, but due to the not so good quality of the orientation, a lot of information are lost locally. In opposition, the second method is very precise but heavily time consuming.
Since I already annotate more than 500 000 basal bodies, the idea of this project is to feed a Convolution Neural Network with this dataset to get the orientation of the BBs with a great accuracy. If I succeed the next steps will to develop a tool to detect automatically the basal bodies on images. Here I will focus on the training of a neural network to predict BB orientation.  

### II/ Source of the Data.
For more details on the acquisition of the full experimental protocol [1]. Basically, small worms (~1mm) were immunolabelled with anti-rootletin antibody which allows to observe the localisation of the BBs and their orientation. The ventral epidermis of the worm was then fully are partially acquired with a fluorescent microscope. After a pre-treatment, I obtain images of size up to 150 million pixels.
Images were partially are completely annotated resulting in the labelling of +500 000 BBs. During this project, I work only on a small fraction of this Dataset (~50 000 BBs)


### III/ Results  
I split my code into 3 parts (accessible on GitHub)  
- Extraction of annotated structure.  
- Training of the CNN.  
- Validation of the model  

#### A)	Extraction of annotated BBs.
My previous work led me to annotate thousands of BBs. Those annotations were stored in dozens of excel files (at least one per worm). Each Excel file correspond to the acquisition of one worm. I iterated through each file to get the x-y coordinates + orientation of each annotated BB. Then thanks to their coordinate a images of size 32*32 (this size was chosen in respect to the size of the structure that we want to orient) was extracted from the original. Those images were then used to train the model.  
  
Either due to problems in my code or more likely in the initial Dataset, some extracted images are either completely black (they were removed of the dataset) or not centered on a BB. Those images were kept in the dataset but they will affect negatively the training of the model. In the future, further investigation will be needed to fix this issue.  
  
#### B)	Training of the CNN  
After extraction and cleaning of the Dataset, a CNN coded in PyTorch was trained. The backbone of the CNN is AlexNet, so I called my Network SchmidteaNet. To fit the AlexNet backbone with my (small: 32*32) images, Kernel Size, padding and stride of the convolutional layer and Poolling layer were adapted and Batch Normalization after convolution and fully connected layers.
Concerning the loss function, I choose for this report to use only a classification (36 classes corresponding to a 10° interval) approach by using the Cross Entropy Loss function. Two reason led me to this choice: the first on is the precision of my own annotation which are not perfect. Considering a precision of +/- 5° seems quite fair (from my own experience). The second reason is that I didn't found a loss function for circular problem. ‘Classical’ loss function for regressive problem (such as Mean Square Error) are inappropriate. My values are angle on a 360° circle. As a consequence a angle of 0.1° is almost the same that an orientation of 359.9° which is not reflected by MSE. 
Note: an appropriate loss function should probably related to cosine and sinus, as maybe the Circular Standard Deviation. If you have some ideas, please let me know.
Other parameters of the CNN can be found in the code. My model was train for ~12h (300 epochs) and reach an accuracy of 90% on the test set.  
  
#### C)	Validation of the model  
The aim of this project is to know if the prediction of a CNN can be better that the method propose in [2]. To validate my model, I choose to use it in 'real' condition, meaning that I didn't test its accuracy on BB randomly pick in a dataset, but on partially annotated worm. The orientation of the BBs on the ventral epidermis of planarian is highly regulated [1, 2]. Consequently, they are locally oriented in the same direction. A metric use to compare the angle dispersion is the Circular Standard Deviation (CSD). During my manual annotation I observed a local CSD of 19° [1]. In contrast, the local CSD obtain with the annotation method develop by Vu et al. was 47° (unpublish data).   
On my Validation worm, I obtain a prediction accuracy 73,3%. Among the ‘mis-annotated’ BBs, 17.7% are in the neighbour class (error < +/-20°) and 5.3% in the class after (error < +/- 20°). Then I look at the CSD. The manual annotation led to a Circular standard error of 13.8°. On the same Dataset, the prediction (after transformation of the classes in angle) give a CSD of 14.7°. 
Note: Due to technical aspects that I will not describe here, CSD calculation was not implemented in the python codes.  
  
### IV/ Conclusion & Perspective
This work illustrate how CNN can be powerful to perform classification task. With only a small proportion of the available Dataset, the trained model completely outperform the accuracy of [2] and the execution speed of [1]. During this project I choose a classification approach with 36 classes. In order to augment the precision of the prediction to fit more closely with reality, a higher number of classes can be tested (360?). A switch to a regressive method could also provide good results. Another possible improvement is to use a deeper neural network. With ‘only’ 8 layers (5 convolutions, 3 fully connected) and 36 million of parameters, the neural network used in this project is not that deep.  


### V/ Bibliography  
The papers are available at: https://github.com/Cyril-Basquin/Bibliography  
[1] Basquin C, Ershov D, Gaudin N, Vu HT, Louis B, Papon JF, Orfila AM, Mansour S Rink JC and Azimzadeh J. Emergence of a Bilaterally Symmetric Pattern from chiral components in the Planarian Epidermis. Dev Cell 2019 51, 516-525.  
[2] Vu H, Mansour S, Kücken M, Blasse C, Basquin C, Azimzadeh J, Myers EW, Brusch L and Rink JC. Dynamic Polarization of the Multiciliated Planarian Epidermis between Body Plan Landmarks. Dev cell 2019 51, 526-542.  
