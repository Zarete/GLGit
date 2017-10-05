DEADLINES :
- [x] Lundi : script benchmark + Stats
- [x] Mardi : Texte
- [ ] Mercredi : Texte + Images + Relecture
- [ ] Jeudi: Dernière relecture
- [ ] Vendredi : REMISE DU RAPPORT AVANT 18H

TODO :
- [x] Faire script complet pour tester toutes les opérations et tailles d'images
- [x] Script R pour statistiques (ajouter des trucs ou rendre plus beau)
- [ ] Images pour chaque partie
- [ ] Images pour chaque partie + Références
- [ ] Faire arborescence dossier pour Taveau
- [ ] Ajouter les scripts finaux en annexe

# <center>2D filters methods for image processing in bioinformatics</center>
<span style = "color:grey">**Formation**</span> <span style = "color:grey;float:right">**Cours 4TBI901U**
</span>
Master 2 Bioinformatique <span style = "float:right">Bioinformatique Structurale</span>
<br/>
<span style = "color:grey">**Auteurs**</span> <span style = "color:grey;float:right">**Responsable UE**</span>
Gary BOUCHENTOUF * <span style = "float:right">Jean-Christophe TAVEAU</span>
Tristan FRANCES
Thomas MAUCOURT
<br/>

## Introduction
<span style = "text-align:justify">
    
The development of technologies allowing images captures has permited to scientists to use new devices in many domains such as astronomy, geology, biology etc (*Add reasons why they use this tools*). The main problem to deal with images is that in most of cases, these cannot be used in their raw format to make analysis. Actually there are additional steps required which are called image processing. After treatment the images can reveal all the things that scientist could need.
There are many possibilities to process an image according to the final result wanted. In this report we will focus on 2D Linear Filters. Filters are used to convert an input image into an output, based on the convolution product principle (Figure 1). 

![convolution_equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/09e0e66216b29a0ccd3d60a6e0f9aba3ce6fb4b2)
<span style = "font-size:14px"> **Figure 1**. Convolution product formula. This mathematical formula can be seen as the combination of two matrix in image processing.</span>

Basically, a convolution product is the result of two matrix combinations. Convolution operation needs an image and a convolution mask also called kernel. An image is actually a bidimensional matrix in whic each case correspond to a specific pixel value (Figure 2A). Its dimension has a value of *width\*height*. This matrix will be combined with the convolution mask which is a matrix *j\k* with *j* and *k* odds values (Figure 2B). It is important to keep in mind that if the kernel is not symetrical, it needs a rotation of 180°.
The result of the convolution product gives a new value to the central pixel of the kernel based on the weighted average of the surrounding pixels(Figure 2C). It leads to a new image with modified pixels values.

Through this report we will discuss three filters using this mathematical principle in imageJ software :
* Convolve
* Gaussian blur
* Mean filter

## 1. Material and Methods

### 1.1. ImageJ
To perform the experiments explained later in this report ImageJ(Reference) has been used. It is an open source software regrouping a large community and allowing images manipulations and analyses. It is widely used across the world by many researcher in all kind of domains. One thing that make the popularity of ImageJ is the possibility to easily develop plugins to automatize image processing and analyses and its well documented API (Application Programming Interface). Thus we decided to use it as the main goal of the project is to develop a web version of ImageJ.  

### 1.2. Convolve
We previously define that convolution needed an image and a kernel to be performed. That raises a problem of coverage between the mask and the targeted picture. Indeed at the borders of the image the kernel can have some of its parts outside the picture. To solve this issue several possibilities can be applied :

* adding a stripe of pixels with a value equals to 0 (black) around the image
* extending the borders by mirroring the edges
* starting the processing at position *(X((kw-1)/2), Y((kh-1)/2))* with *kw* and *kh* respectively the kernel's width and height
* not processing the convolution for this pixels

As convolution is a well defined mathematical principle very used in many domains and especially in image processing, several methods have been developed to improve the computation. We can find five main implementation of convolution :
* basic approach
* convolution with separable kernels
* recursive filtering
* fast convolution

First the "basic approach". It is simply based on the definition of convolution by computing the inner product of the current sample of the image and the kernel. The value obtained is then stored into the central mask's pixel. For information, the algorithmic complexity of such method is *O(N²)*. This approach is still widely used due to its simplicity and the possibility to parallelize the processing.

An other way to perform convolution is allowed in the case of specific kernels called separable (Reference). A kernel is separable in 2-Dimensions when it can be decomposed into two vectors (a column vector and a row vector) (Figure). These kinds of kernels are found in Gaussian masks for example. Such kernels decrease the complexity because they allow to separate the basic convolution process into two simpler convolutions.

The third methods available to apply convolution is called recursive filtering. This approach is based on the dependence of the sample's (rank *n*) with the previous samples (rank *n-1*, *n-2*, ...). It needs a step to
define the recursive formula needed for the convolution product wanted and that can be very hard task. One property of convolution is that the Fourier transform convolution

The last approach we will discuss is the most popular and is actually the one implemented into ImageJ software. The fast convolution is based on the frequency domain unlike the previous we discussed that were used into the spatial domain. This method go through an extra step by using the Fast Fourier Transform (FFT) on the input signal before performing convolution. According to a mathematical principle a convolution into the frequency domain is equivalent to a product of Fourier Transforms. This means that a convolution into the spatial domain is equivalent to a multiplication into the frequency domain. Thus :
<center><b><i>kernel * sample = FFT<sup>-1</sup>(FFT(kernel) x FFT(sample))</i></b></center>
<br/>
By looking for plugins implementing the different methods described above, we have been able to locate just one plugin different from the one used by default in ImageJ. This plugin is called <b>```Real_Convolver.java```</b> (Reference) and implements the simple convolution into the spatial domain. It allows the use of convolution only for 32 bits images, so the totality of the benchmarking has been realized on this type of image.

### 1.3. Gaussian Blur

The Gaussian Blur also known as Gaussian Smoothing operator is convolution operator that is used to blur images and remove detail and noise. In this sense it is similar to the mean filter, but differ by using a different kernel. Thus, using a Gaussian Blur filter consist in realising a convolution on a picture with a gaussian function.

The filter set up the value of a pixel by 
**Gaussian Blur 2-D Equation**
![Gaussian Blur 2-D Equation](https://homepages.inf.ed.ac.uk/rbf/HIPR2/eqns/eqngaus2.gif "Gaussian Blur 2-D Equation")



Again, to speed up images treatment, algorithm have been developed. Here we are going to compare the ImageJ Gaussian Blur filter default function to JAVA  plugin, Accurate Guassian Blur. This last has been implemented for high accuracy treatement espcially for 32-bit images. Also, these methods encounter the same trouble as the convolution with edges of images when they are processed.

![Gaussian Blur 2-D Shape](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/gauss2.gif "Gaussian Blur 2-D Shape")





<!-- ![alt text](/path/ "Title") commande for adding local picture -->

### 1.4. Mean filter
Among the linear filters, the most common is the mean filter beacause it's easy to implement and also reliable. As it has been said before (see section 1.2 Gaussian blur), it's a smoothing filter which has the purpose of smoothing an image by blurring and removing detail and noise. Doing so, a convolution mask is used, it can be with various shape (square, rectangular or circular) and in vast majority the weights in the kernel are uniforms (meaning the initial values in the kernel are the same), but those can also be triangular (i.e.  inversely proportionnal to distance from the input sample). [^MAT2007]

For example if S*xy* represents the set of coordinates in a rectangular sub image window of size *m × n* centered at point *(x,y)*, the arithmetic mean filtering process computes the average value of the corrupted image *g(x,y)* in the area defined by S*xy*. The value of the restored image *f* at any point *(x,y)* is simply the arithmetic mean computed using the pixels in the region defined by S*xy*. In other words :

![equation aritmethic mean filter](./formule_mean.png)
</br> *Equation of the arithmetic mean filter* 



This operation can be implemented using a convolution mask in which all coefficients have value *1/(mn)*. A mean filter simply smoothes local variations in an image. Noise is reduced as a result of blurring. The main problem of this filter is that noisy pixels—including anomalous spikes—are weighted the same as all the other pixels in the kernel. [^GAJ2011] 
Because it's uses a convolution kernel, we find the same issue as describe in section *1.3 Convolve*, thus the same solutions can be applied here.

During our research, we didn't found plugin of the mean filter whose proceed differently from the original plugin. That's why the benchmarking were realised only with the mean filter already implemented in *ImageJ*. 


### 1.5. Benchmark

Benchmarking were realized based on time of processing. A JavaScript script was implemented to automatize the process (Supplementary Data 1). First a warmup phase consisting in running 20 times the chosen operation is performed. After the script switch to the testing phase consisting in 20 runs of the image processing selected. This process is used on the same RGB image with a size of *1920\*1200*. This image was converted into 32 bits, 16 bits and 8 bits greyscale images and the benchmark was performed on each of these pictures. To get more data on other sizes of image, the initial picture was also resized two times. This lead to three images sizes : *1920\*1200*, *960\*600* and *512\*320*. The results are displayed in the imageJ log console and saved into a csv file.
In this case, time processing of Convolve, Mean and Gaussian blur were calculated. An R script allows to get a boxplot of the results showing the different quartils and the mean of data for the warmup and the testing phase.  The process has been repeated for the alternatives plugins found for the different filters.

Benchmarking in JavaScript is really complicated since the results are very variable and that they depend a lot on the state of the computer used for the test (Référence Article Taveau). Moreover, implementing a good benchmark implies a deep understanding of the execution processes and the interpreter. Thus the results obtained by our method have to be taken as informative. 

## 2. Results

### 2.1. Convolve
We compared two plugins, one realizing the "original" way to make a convolution and another one getting an extra step of FFT to get the result.
The first thing important to note is that the output images from one method and the other are the same one (Figure).

<center><img src = "./img/RC_vs_C.png"></center>
<br/><span style = "font-size:14px"> **Figure X**. Convolution performed with ```Real_Convolver.java``` (on the left) and the ImageJ default convolution (on the right)</span><br/><br/>

First, the time of processing for the different images size were computed using R (Figure). This shows a diminution of processing time the smaller the image gets.

<center><img src='./Size/size.svg' width=600></center>
<br/><span style = "font-size:14px"> **Figure X**. Comparison of convolution processing time for 3 types of images. The reduction of pixels lead to a decrese in time processing for the ImageJ implementation of convolution and for the plugin ```Real_Convolver.java```.</span><br/><br/>

In a second time a comparison between **```Real_Convolver.java```** and the convolution plugin implemented into ImageJ was performed (Figure). We can see that the plugin using the step of FFT, the ImageJ's one, takes less time to process an image no matter his size.

<center><img src='./Convolve_vs_RConvolve/vs.svg'></center>
<br/><span style = "font-size:14px"> **Figure X**. Processing time for differents images sizes and comparison between the ImageJ convolution and the ```Real_Convolver.java```. From left to right we can see the diminution of time processing for both plugins but with a better performance for the FFT extra step processing.</span><br/>

### 2.2. Gaussian Blur
### 2.3. Mean filter
The results of this benchmark are represented in *FigureX*.
![figure results mean](./img_type/results_mean.png)

*FigureX : Representation of the benchmark for the mean filter of ImageJ*
*Legend : sizes  1 : 1920 x 1200; 2 : 900 x 600; 3: 512x320*</br>
We obtain 4 graphics (one per type of image) which contains 3 boxplot (one per size of image) comparing the difference of time processing. We can see there's no real differences for the time processing between the type of image. However it is not the case for the size that shows the bigger the image is, the longer the processing last.


## 3. Discussion
The several benchmarks performed allowed us to observe certain behaviors for the differents plugins tested.

For the convolution part, we have seen a difference in the output of the process. We can not explain this difference, but our hypothesis is that the FFT, makes some approximations and this modify original image at the end. Moreover the algorithm using an FFT before the convolution (ImageJ) seems to have better performances than the ```Real_Convolver.java```. As expected we saw a decrease of processing time the less the image is big. That can easily be explained by the fact that it needs less calculus for tiny image because there are less pixels to process.

Concerning the mean filter, the results of the benchmark don't allow us to say because of the lack of alternative plugin and the weak sturdiness of the benchmarking using *Javascript*. This can be explained by the fact the mean filter isn't a good noise removal, due to the fact it's simple algorthim, leading to the development of others filters more efficients. 

## Conclusion
Through this work, we were able to study the differents process used for 2D Filters. The final goal is to implement the best algorithm into a "web ImageJ like" for each filter studied.

For the further steps, we decided to use the convolution algorithm found in ImageJ source code. We will also implement the recognition of separable kernels to increase a bit more the processing by creating two vectors that will be used to make the convolution.

## References
[^MAT2007]:MATT HALL. Smooth operator: Smoothing seismic interpretations and attributes. The Leading Edge. 2007 Jan;26(1):16–20.
[^GAJ2011]: Gajanand Gupta. Algorithm for Image Processing Using Improved Median Filter and Comparison of Mean, Median and Improved Median Filter. International Journal of Soft Computing and Engineering (IJSCE) ISSN. 2011 Nov;Volume-1(Issue-5):2231–307.

</span>
