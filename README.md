# Technical Report on Remote Sensing Image Segmentation 
### Experiments 
#### 1.1 Problem Statement 
Remote sensing image segmentation is crucial for various applications, such as land cover 
classification, environmental monitoring, and urban planning. Traditional segmentation models 
require dense pixel-wise annotations, which are expensive and time-consuming to obtain. To 
address this, we explore a sparse annotation approach where only a subset of pixels is labeled, 
leveraging a custom Partial Cross-Entropy Loss to improve segmentation performance. Thus, in 
this study, we implement Partial Cross-Entropy Loss and integrate it into a remote sensing 
segmentation network. The goal is to investigate how this loss function influences model 
performance compared to standard approaches. Additionally, we explore how hyperparameters 
such as batch size and learning rate impact segmentation performance.  
#### 1.2 Objective 
The primary goals of this study are: 
1. To implement U-Net with ResNet-34 encoder for remote sensing image segmentation. 
2. To incorporate Partial Cross-Entropy Loss to handle sparse annotations effectively. 
3. To conduct experiments by varying key hyperparameters such as learning rate and batch 
size. 
4. To analyze the impact of sparse labeling on model performance and evaluate its 
generalizability. 

### Methodology 

#### 2.1 Dataset  
We use the GID-15 dataset, a widely used remote sensing dataset containing high-resolution 
images with 15 land cover classes, including urban areas, vegetation, water bodies, and farmland. 
Since the dataset consists of large images, the following preprocessing steps were applied: 
1. Resizing: All images were resized to 512×512 pixels to ensure consistency. 
2. Data Augmentation: Applied horizontal flipping to enhance generalization. 
3. Normalization: Pixel values were scaled to [0,1] range for stable training. 
Since fully labeled segmentation masks are costly to obtain, we simulate a sparse annotation 
scenario by randomly selecting 500 labeled pixels per image. These labeled pixels are used for 
training, while the rest are ignored using Partial Cross-Entropy Loss. 

#### 2.2 Model Architecture 
U-Net with ResNet-34 Encoder 
We utilize Segmentation Models PyTorch (SMP) to implement a U-Net model with a ResNet-34 
encoder pre-trained on ImageNet. 
1. ResNet-34 extracts high-level spatial features from images. 
2. A series of upsampling and convolution layers reconstruct the segmentation map. 
3. Final Layer: A 1×1 convolution to output a segmentation mask of 15 classes 

### Partial Cross-Entropy Loss 
Cross-entropy loss is commonly used in segmentation tasks but assumes fully labeled data. 
However, in remote sensing applications, labels may be sparse or partially annotated. Hence, in 
our sparse annotation setup, most pixels remain unlabeled. Partial Cross-Entropy Loss (PCE) 
modifies the standard cross-entropy by computing loss only for known labels while ignoring 
unknown, uncertain pixels or in our case pixels that serve as just the background of the image 
(index 0). 
This ensures that the model does not learn from uncertain or missing labels, leading to better 
generalization.
