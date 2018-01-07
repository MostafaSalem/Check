# A supervised framework with intensity subtraction and deformation field features for the detection of new T2-w lesions in multiple sclerosis

A new framework for detecting new T2-w lesions in multiple sclerosis is proposed. We train logistic regression classifier with subtraction and deformation features. In this repository, the framework is implemented as a docker image. An electronic paper of the method is available here: [https://www.sciencedirect.com/science/article/pii/S2213158217302954](https://www.sciencedirect.com/science/article/pii/S2213158217302954)

## Overview: 
In this study, we merge intensity- and deformation-based approaches in an automated multi-channel supervised logistic regression classification. In contrast with the previous supervised approaches, our model uses features not only from the baseline, follow-up, and subtraction images but also from the DF operators obtained from the non-rigid registration between timepoints scans. We evaluated the performance of the method using leave-one-out cross validation on 36 images presenting new T2-w lesions on the follow-up scan and also on 24 images without new lesions.
![pipeline](/imgs/pipeline.png)	
