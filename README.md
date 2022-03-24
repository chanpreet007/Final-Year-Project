# Identify and localize COVID-19 abnormalities on chest radiographs

# 1). Abstract
The Society for Imaging Informatics in Medicine (SIM)’s collected dataset con- taining Chest X rays and corresponding labels with bounding box information could help identify and localize COVID-19 abnormalities on chest radiographs. In particular, we’ll categorize the radiographs as negative for pneumonia or typ- ical, indeterminate, or atypical for CO ̈VID-19. You and your model will work with imaging data and annotations from a group of radiologists.
# 2). Introduction
Five times more deadly than the flu, COVID-19 causes significant morbidity and mortality. Like other pneumonias, pulmonary infection with COVID-19 results in inflammation and fluid in the lungs. COVID-19 looks very similar to other viral and bacterial pneumonias on chest radiographs, which makes it difficult to diagnose. our computer vision model to detect and localize COVID-19 would help doctors provide a quick and confident diagnosis. As a result, patients could get the right treatment before the most severe effects of the virus take hold.
# 3). Problem Statement
Currently, COVID-19 can be diagnosed via polymerase chain reaction to detect genetic material from the virus or chest radiograph. However, it can take a few hours and sometimes days before the molecular test results are back. By contrast, chest radiographs can be obtained in minutes. While guidelines exist to help radiologists differentiate COVID-19 from other types of infection, their assessments vary. In addition, non-radiologists could be supported with better localization of the disease, such as with a visual bounding box. we’ll identify and localize COVID-19 abnormalities on chest radiographs. In particular, we’ll categorize the radiographs as negative for pneumonia or typical, indeterminate, or atypical for COVID-19. This will also enable doctors to see the extent of the disease and help them make decisions regarding treatment. Depending upon severity, affected patients may need hofpitalisation, admission into an intensive care unit, or supportive therapies like mechanical ventilation. As a result of better diagnosis, more patients will quickly receive the best care for their condition, which could mitigate the most severe effects of the virus.
# 4). Methodology
![tp-whiteboard-2-2](https://user-images.githubusercontent.com/43450694/160013321-03919a73-4cbf-4ede-ad50-7bec71906c98.jpg)
Study Level Models All of study models were trained with variants of Effi- cientnet models.Some models were trained on multiple stages which includes finetuning with a reduced learning rate or increasing the image size.Baseline Architectures used in our final study level solution:
# 4.1 Efficientnet v2m
Pretrained imagenet weights were used. 512 x 512(5- fold) 640x640(fold 0) 1024x1024(fold 0) image size. PCAM pooling + SAM attention map used in stage2 Loss fnc: BCE4-class + [0.75* lovaszloss+0.25∗BCE]Segmentationloss
# 4.2 Efficientnet v2l
Pretrained imagenet weights were used from timm models. 512 x 512(5 folds) 640x640 (fold 1) image size PCAM pooling + SAM attention map used in training and finetune stage. Loss fnc: BCE4-class + [0.75* lovaszloss + 0.25 ∗ BCE]Segmentationloss
# 4.3 Efficientnet B5
Pretrained imagenet weights were used from timm models. 640 x 640 image size average pooling + sCSE attention map along with multi-head attention was used in the training and finetune stage. Loss fnc: BCE4-class + [0.75* lovaszloss + 0.25 ∗ BCE]Segmentationloss
# 4.4 Efficientnet B7
Pretrained imagenet weights were used from timm models. 640 x 640 image size average pooling + sCSE attention map along with multi-head attention was used in the training and finetune stage.

# 4.5 Image Level Solution (None)
Image Level Solution Binary Classification : Very Similar to study level models, binary model was trained with Efficientnet B6. Again our model was trained with imagenet weights without any pretraining on external data.All none pre- dictions were the same as the “Negative For Pneumonia” Class which was in study predictions. So, our final binary predictions were a weighted average of Efficientnet binary predictions and study-based Efficientnet- v2m(5 fold) Nega- tive for Pneumonia predictions whose training was explained above in the study level solution.

## 4.6 Image Level Solution (opacity)
opacity is the actual location of the infection i.e the bounding boxes
# 4.7 Efficientnet - D5
: It was trained on just training data. The image size used was 512 x 512. It was trained in two stages, in the second stage it was finetuned with a lower learning rate. The exponential moving average(EMA) was also used in this model’s training.
# 4.8 Efficientnet - D3:
It was trained on the training data + public test data pseudo labels generated from an ensemble of decent scoring object detection models. Image size used was the default for efficientdet D3 which is 896 x 896. It was also trained in two stages as efficientdet D5. The exponential moving average(EMA) was also used in this model’s training.
# 4.9 Yolo - v5l6:
It was trained on the training data + public test data pseudo labels. The image size used was 640 x 640 for training. Some images without Bounding Boxes were also included in training data( 20
# 4.10 Yolo - v5x:
It was trained on the training data + public test data pseudo labels. Image size used was the default for Yolo-v5x which is 640 x 640. Some images without Bounding Boxes were also included in training data( 20 %)
# 4.11 RetinaNet:
Thebackboneusedforretinanetwasresnext10164x4d.Imageusedwas(1333,800)
# 5). Result
Ensembling the study level and Image level model will result in the map (Mean Average Precision) of 0.617 where as the state of art in the domain is 0.635



