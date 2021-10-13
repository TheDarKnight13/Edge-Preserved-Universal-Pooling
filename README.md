# Edge-Preserved-Universal-Pooling

With the introduction of anti-aliased convolutional neural networks (CNN), there has been some resurgence in relooking the way pooling is done in CNNs. The fundamental building block of the anti-aliased CNN has been the application of Gaussian smoothing before the pooling operation to reduce the distortion due to aliasing thereby making CNNs shift invariant. Wavelet based approaches have also been proposed as a possibility of additional noise removal capability and gave interesting results for even segmentation tasks. However, all the approaches proposed completely remove the high frequency components under the assumption that they are noise. However, by removing high frequency components, the edges in the feature maps are also lost. In this work, an exhaustive analysis of the edge preserving pooling options for classification, segmentation and autoencoders are presented. Two novel pooling approaches are presented such as Laplacian-Gaussian Concatenation with Attention (LGCA) pooling and Wavelet based approximate-detailed coefficient concatenation with attention (WADCA) pooling. The results suggest that the proposed pooling approaches outperform the conventional pooling as well as blur pooling for classification, segmentation and autoencoders. In terms of average binary classification accuracy (cats vs dogs), the proposed LGCA approach outperforms the normal pooling and blur pooling by 4% and 2%, 3% and 4%, 3% and 0.5% for MobileNetv2, DenseNet121 and ResNet50 respectively. On the other hand, the proposed WADCA approach outperforms the normal pooling and blur pooling by 5% and 3%, 2% and 3%, 2 and 0.17% for MobileNetv2, DenseNet121 and ResNet50 respectively. It is also observed from results that edge preserving pooling doesn’t have any significance in segmentation tasks possibly due to high resolution to low resolution translation, whereas for convolutional auto encoders high resolution reconstruction has been observed for the LGCA pooling. 

# Laplacian-Gaussian Concatenation with Attention

# Wavelet based approximate-detailed coefficient concatenation with attention
