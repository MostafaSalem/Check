# A supervised framework with intensity subtraction and deformation field features for the detection of new T2-w lesions in multiple sclerosis

A new framework for detecting new T2-w lesions in multiple sclerosis is proposed. We train logistic regression classifier with subtraction and deformation features. In this repository, the framework is implemented as a docker image. An electronic paper of the method is available here: [https://www.sciencedirect.com/science/article/pii/S2213158217302954](https://www.sciencedirect.com/science/article/pii/S2213158217302954)

## Overview: 
In this study, we merge intensity- and deformation-based approaches in an automated multi-channel supervised logistic regression classification. In contrast with the previous supervised approaches, our model uses features not only from the baseline, follow-up, and subtraction images but also from the DF operators obtained from the non-rigid registration between timepoints scans. We evaluated the performance of the method using leave-one-out cross validation on 36 images presenting new T2-w lesions on the follow-up scan and also on 24 images without new lesions.
![pipeline](/imgs/pipeline.png)	

# Install:
The framework is implemented as a docker image. First, you have to install [Docker](https://www.docker.com/). You can follow this [ How To Install and Use Docker on Ubuntu 16.04
](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) tutorial to install it . One the docker is installed, run the following command to build the docker image [lr_model_img](https://www.sciencedirect.com/science/article/pii/S2213158217302954):

```
build_docker.sh
```
After building the docker image, run the python script Main.py which display an option menu with all the avialable options.

```python
python Main.py
```

You can preprocess the dataset, train, test, and evaluate the results as follows:  
1- Preprocess the dataset.  
2- Train a LR model using Leave-One-Out.  
3- Train a cascade LR model using Leave-One-Out.  
4- Train a LR model (The full model).  
5- Train a cascade LR model.  
6- Test a dataset using a LR model.  
7- Test a dataset using a Cascade LR model.  
8- Evaluate segmentation Masks to dataset GT.  
9- Evaluate segmentation Masks for No-NewLesions Cases (FPs).  
10- Exit:  
# How to use it:
## Preprocessing the dataset
For preprocessing, T1-w, T2-w, Pd-w, and FLAIR should be obtained for each patient. The preprocessing module expects the dataset to be stored as:
```
[dataset_folder]
          [patient_1_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image
          [patient_2_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image
	     
		 ....
		 
		 
          [patient_n_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image, new-lesion-Mask (in case od training)
```
**Note: you have to choise which image space  you want to register to (PD space or FLAIR space). Also, you will be asked how many modality you want to use (only in obtaining the deformation fields).** You can skip patient folders by preceding the folder name with **'-'**

## Training
You can use **Train a LR model using Leave-One-Out** to train the LR model using a leave one out scheme. The new lesions mask should be in the follow-up folder for each patient. Also, **Train a cascade LR model using Leave-One-Out**  will train two LR model using a leave one out scheme. The second cascaded model will be trained using the all positive cases + the FP from the first model. Finally, you can use **Train a LR model (The full model)** and **Train a cascade LR model** to train a full model with your dataset. These models can be used for testing other datasets. The training module expects the dataset to be stored as:
```
[training_folder]
          [patient_1_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image, new-lesionMask (in case of training)
          [patient_2_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image, new-lesion-Mask (in case of training)
	     
		 ....
		 
		 
          [patient_n_folder]
                    [baseline_folder]
                        t1_image, t2_image, pd_image, flair_image
                    [followup_folder]
                        t1_image, t2_image, pd_image, flair_image, new-lesion-Mask (in case od training)
```
**Note: you can skip patient folders by preceding the folder name with **'-'**

## Testing
The models created in the training step can be used for testing other datasets. You can use **Test a dataset using a LR model** and **Test a dataset using a Cascade LR model** to test using models created by **Train a LR model (The full model)** and **Train a cascade LR model** respectively.
**Note: you have to create a new folder for the testing results. The script will create a folder for each case inside the main result folder. Each patient folder will contain 3 files:  
1- newlesions_probability.nii.gz (the probability map).  
2- newlesions_labels.nii.gz(segmentation mask by thresholding the probability map by 0.5).  
3-newlesions_labels.s3.nii.gz(segmentation mask by thresholding the probability map by 0.5 and removing lesions less than 3 voxels).**   You can skip patient folders by preceding the folder name with **'-'**  

## Evaluation
You can use **Evaluate segmentation Masks to dataset GT** to evaluate the framework segmentation masks against the ground truth. Also, you can use **Evaluate segmentation Masks for No-NewLesions Cases (FPs)** to evaluate the framework segmentation masks for patient with no new T2 lesions (or healthy cases). The output of that case is how many FPs generated by the framwork.

# Citing this work:

Please cite this work as:

```
@article{SALEM2018,
title = "A supervised framework with intensity subtraction and deformation field features for the detection of new T2-w lesions in multiple sclerosis",
journal = "NeuroImage: Clinical",
volume = "17",
number = "Supplement C",
pages = "607 - 615",
year = "2018",
issn = "2213-1582",
doi = "https://doi.org/10.1016/j.nicl.2017.11.015",
url = "http://www.sciencedirect.com/science/article/pii/S2213158217302954",
author = "Mostafa Salem and Mariano Cabezas and Sergi Valverde and Deborah Pareto and Arnau Oliver and Joaquim Salvi and Àlex Rovira and Xavier Lladó",
keywords = "Brain, MRI, Multiple sclerosis, Automatic new lesion detection, Machine learning",
abstract = "Abstract Longitudinal magnetic resonance imaging (MRI) analysis has an important role in multiple sclerosis diagnosis and follow-up. The presence of new T2-w lesions on brain MRI scans is considered a prognostic and predictive biomarker for the disease. In this study, we propose a supervised approach for detecting new T2-w lesions using features from image intensities, subtraction values, and deformation fields (DF). One year apart multi-channel brain MRI scans were obtained for 60 patients, 36 of them with new T2-w lesions. Images from both temporal points were preprocessed and co-registered. Afterwards, they were registered using multi-resolution affine registration, allowing their subtraction. In particular, the DFs between both images were computed with the Demons non-rigid registration algorithm. Afterwards, a logistic regression model was trained with features from image intensities, subtraction values, and DF operators. We evaluated the performance of the model following a leave-one-out cross-validation scheme. In terms of detection, we obtained a mean Dice similarity coefficient of 0.77 with a true-positive rate of 74.30% and a false-positive detection rate of 11.86%. In terms of segmentation, we obtained a mean Dice similarity coefficient of 0.56. The performance of our model was significantly higher than state-of-the-art methods. The performance of the proposed method shows the benefits of using DF operators as features to train a supervised learning model. Compared to other methods, the proposed model decreases the number of false-positives while increasing the number of true-positives, which is relevant for clinical settings."
}
```
