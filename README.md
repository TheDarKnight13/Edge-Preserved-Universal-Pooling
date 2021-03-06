# Edge-Preserved-Universal-Pooling

This work relooks the way pooling is done in CNNs. An exhaustive analysis of the edge preserving pooling options for classification, segmentation and autoencoders has been done. Two novel pooling approaches are presented, namely the  Laplacian-Gaussian Concatenation with Attention (LGCA) pooling and Wavelet based approximate-detailed coefficient concatenation with attention (WADCA) pooling. The results suggest that the proposed pooling approaches outperform the conventional pooling as well as blur pooling for classification, segmentation and autoencoders. In terms of average binary classification accuracy (cats vs dogs), the proposed LGCA approach outperforms the normal pooling and blur pooling by 4% and 2%, 3% and 4%, 3% and 0.5% for MobileNetv2, DenseNet121 and ResNet50 respectively. On the other hand, the proposed WADCA approach outperforms the normal pooling and blur pooling by 5% and 3%, 2% and 3%, 2 and 0.17% for MobileNetv2, DenseNet121 and ResNet50 respectively. It is also observed from results that edge preserving pooling doesn’t have any significance in segmentation tasks possibly due to high resolution to low resolution translation, whereas for convolutional auto encoders high resolution reconstruction has been observed for the LGCA pooling. 

# Laplacian-Gaussian Concatenation with Attention (LGCA)
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Picture1.png' width=500><br>

In the LGCA pooling approach, Gaussian and Laplacian filtering operations are performed on the input feature maps and are concatenated and passed through an attention network. Since the channel dimension is doubled during the concatenation operation, the output from the attention framework is passed through a convolution layer to reduce to the original dimension as the feature map. The attention network is used to remove the redundancy among channels and to bring the focus of the overall network to the most relevant channels in the concatenated output. The architecture for the LGCA approach is as shown above.

**Implementation for Replacing Traditional Max Pooling**
``` python
model.maxpool = ConMax(inc, device, size)
```
where, <br />
inc is the number of input channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
size is the Gaussian Kernel size

**Implementation for Replacing Traditional Average Pooling**
``` python
model.avgpool = ConAvg(inc, device, size)
```
where, <br />
inc is the number of input channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
size is the Gaussian Kernel size

**Implementation for Replacing Traditional Strided Convolution if its followed by ReLU**
<br />
This 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride,padding)
model.relu = nn.ReLU()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride-1,paddingn)
model.relu = ConConv(outc,device,size)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
filter is the filter size <br />
size is the Gaussian Kernel size <br />
paddingn is equal to 1 if padding is 1, else equal to padding -1

**Implementation for Replacing Traditional Strided Convolution if its followed by Batch Normalization** 
<br /> 
This 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride,padding)
model.bn = nn.BatchNorm2d()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride-1,paddingn)
model.relu = ConConvb(outc,device,size)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
filter is the filter size <br />
size is the Gaussian Kernel size <br />
paddingn is equal to 1 if padding is 1, else equal to padding -1

**Implementation for Replacing 1x1 Strided Convolution if its followed by Batch Normalization** 
<br /> 
This 
``` python
model.conv = nn.Conv2d(inc,outc,1,stride)
model.bn = nn.BatchNorm2d()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,1,stride-1)
model.relu = ConConv1(outc,device)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />

# Wavelet based approximate-detailed coefficient concatenation with attention (WADCA)
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Picture2.png' width=500><br>

In the WADCA approach, 2D DWT  based on Haar wavelet has been employed for the decomposition of the low and high frequency components. The high frequency components are zeroed and 2D-IDWT is employed with concatenating the approximate and detailed coefficient with zeros. Then, like in the case of LGCA, the feature maps are passed through an attention network followed by a convolution layer. The architecture for the WADCA approach is as shown above.

**Implementation for Replacing Traditional Max Pooling**
``` python
model.maxpool = HaarMax(inc, device)
```
where, <br />
inc is the number of input channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />

**Implementation for Replacing Traditional Average Pooling**
``` python
model.avgpool = HaarAvg(inc, device)
```
where, <br />
inc is the number of input channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />

**Implementation for Replacing Traditional Strided Convolution if its followed by ReLU**
<br />
This 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride,padding)
model.relu = nn.ReLU()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride-1,paddingn)
model.relu = HaarConv(outc,device)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
filter is the filter size <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
paddingn is equal to 1 if padding is 1, else equal to padding -1

**Implementation for Replacing Traditional Strided Convolution if its followed by Batch Normalization** 
<br /> 
This 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride,padding)
model.bn = nn.BatchNorm2d()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,filter,stride-1,paddingn)
model.relu = HaarConvb(outc,device)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
filter is the filter size <br />
device is the object representing the device on which a torch.Tensor is allocated <br />
paddingn is equal to 1 if padding is 1, else equal to padding -1

**Implementation for Replacing 1x1 Strided Convolution if its followed by Batch Normalization** 
<br /> 
This 
``` python
model.conv = nn.Conv2d(inc,outc,1,stride)
model.bn = nn.BatchNorm2d()
```
can be replaced by 
``` python
model.conv = nn.Conv2d(inc,outc,1,stride-1)
model.relu = HaarConv1(outc,device)
```
where, <br />
inc is the number of input channels <br />
outc is the number of output channels <br />
device is the object representing the device on which a torch.Tensor is allocated <br />

# Demonstration of Densenet121 with different pooling layers using GradCAM
Original Image <br />
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Pictures/Org2.png' width=200><br>

With Traditional Pooling  <br />
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Pictures/Norm2.png' width=200><br>

With LGCA  <br />
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Pictures/Concat2.png' width=200><br>

With WADCA  <br />
<img src='https://github.com/TheDarKnight13/Edge-Preserved-Universal-Pooling/blob/main/Pictures/Haar2.png' width=200><br>


