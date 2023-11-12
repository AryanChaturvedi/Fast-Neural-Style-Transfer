# Fast-Neural-Style-Transfer
- Neural Style Transfer Using Perceptual loss and Instance Normalization.
- Modified Implimentation of Paper *"Perceptual Losses for Real-Time Style Transfer and Super-Resolution"* with **Instance Normalistion and Variational Regularization**.
- Implimenttion of NST paper *"A Neural Algorithm of Artistic Style"* for Comaparision with Fast Perceptual method
- Fast NST can learn single style and generalise it to any new image while NST trains a model to only transfer style from One image to other image.
- Fast NST is seamlessly fast for inference on other images.
## Brief Overview
### Fast NST using Perceptual Loss
Training a Fast style transfer model requires two networks:
- A pre-trained feature extractor: Perceptual Net (VGG19 trained on IMAGENET)
- And a transfer network : Transfer Net
- Below is VGG19 Architecture used as Feature Extractor.
- ![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/f007fd0a-4d81-4059-8915-cf66a901b361)

Fast NST employs a pre-trained Convolutional Neural Network with added loss functions to transfer style from one image to another and synthesize a newly generated image with the features we want to add.

Style transfer works by activating the neurons in a particular way, such that the output image and the content image should match particularly in the content, whereas the style image and the desired output image should match in texture, and capture the same style characteristics in the activation maps.

These two objectives are combined in a single loss formula, where we can control how much we care about style reconstruction and content reconstruction.

Here are the required inputs to the model for image style transfer:

- A Content Image(Yc): an image to which we want to transfer style to
- A Style Image(Ys):  the style we want to transfer to the content image
- An Generated Input Image (Y) – the final blend of content and style image
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/c73796f7-4887-4b31-ac41-3aa6078ea808)
### Perceptual Loss Net
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/3172eea2-fbcb-45c6-a3d3-0abcf2be33e9)
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/615de9f9-48fd-4e30-8e86-815ed24b7382)
- Feature Map (Generated Image Y) = Output of Relu Layer
- Content Loss Function is MSE loss and is Implimented in Training Function 
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/d92c5373-41a8-4c25-9d27-89c35ad16d20)
###  Transform Net
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/fc8b06b1-e4f0-4458-9f12-7f0480da29c5)
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/9b59d710-63af-4c15-9baf-b5e444ccdf66)
- Original Paper Used Batch Normalisation which captures global features of whole batch(not suitable for style transfer).
- I have used Instance Normalisation
- Instance normalization operates on a single sample.
-  IN doesn’t depend on the batch size, so its implementation is kept the same for both training and testing.
- The problem instance normalization tries to address is that the network should be agnostic to the contrast of the original image.
### Style Reconstruction Loss
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/cdf9ff68-6a26-45cd-9ee9-439f0e371fb9)
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/75120282-a8d2-46a8-9875-435922f11784)
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/afbc4d9e-81c1-4e7b-839b-c9076585537f)
- Style loss is weighted sum of MSE loss on Gram matrix of style image and generated image for all relu layers

### Total Variation Regularization
![image](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/35c01c81-8c59-459e-a125-520f35a30e08)
- Calculates the sum of the absolute differences between adjacent pixels along the last dimension (horizontal dimension) of the image.
- Repeat above process for verticle dimension and add both to get variational loss



## Sample Style Transfer Results
##### All models are trained for less than 5000 iterations due to time constrain.
- Original Steve job Image and Vincent van Gogh's Portrait
  ![Steve Original](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/479df1c8-7b48-4087-b7cd-9cd99609a42d)
- Style Transfer using NST
  ![Steve Style](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/cbde9805-9d67-4900-9852-e31fc6bccb99)
- Original Content and Style Images
![Lion Original](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/e98b44bb-5dc4-44c0-b707-449dc93695fa)
- NST generated Image![Lion Style1](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/fa4d4180-e1cf-46f4-b4d2-27014b26a543)
- Stlye Image: Vincent van Gogh's Self-Portrait
- ![Vincent_van_Gogh_-_Self-Portrait](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/ce10c5a8-6c26-4646-a50b-067af8da1777)
 - **Fast NST Based Style Transfer to Multiple images**
![Mars_Vin_Van_5000](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/e22bfb62-9c64-48eb-bba2-3a6a132de185)
![Golden_gate_Vin_Van_5000](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/6cd25021-2a10-4ed4-abe3-2ef4dc10708f)
![Snow_Mountain_Vin_Van_5000](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/f96d86b5-b2e7-44fe-aaae-b88ea87ad45a)
![Vin_Van_5000](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/95098675-b248-4bb6-8f3c-2fd77125d6e5)
- **Samples From Coco dataset while training**
- ![2400](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/fae5bbc6-ab60-4494-b7b6-49f2345233e2)
- ![2600](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/7b2f79b6-340c-4efb-af20-b68ea94d8409)

- **Original NST Paper Based Style Transfer from Style to Content Image**
![NST](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/b733e42e-5d2f-4f2d-b3c1-965e8ddfab7f)
- Stlye Image: Vincent van Gogh's Bridge
- ![download](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/55bbf4e3-8e1b-4015-a9aa-db64d4251108)
- **Fast NST Based Style Transfer to Multiple images**
  ![VV_Bridge_Gate](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/52cabcc1-2417-4299-867f-3012c6365c5b)
![Vin_Van_Bridge_Mars](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/1b84cd78-1102-4958-8d89-025bf6bea548)
- **Sample From Coco Dataset while training**
- -![3800](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/c4206842-2854-4b7a-856c-b74c1c9e40c3)
- **Original NST Paper Based Style Transfer from Style to Content Image**
![VV_Bridge_NST2](https://github.com/AryanChaturvedi/Fast-Neural-Style-Transfer/assets/77160352/7507b732-c87b-4658-a646-210d26974077)



  




