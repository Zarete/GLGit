
DEADLINES :
- [x] Lundi : script benchmark + Stats
- [ ] Mardi : Texte
- [ ] Mercredi : Images + Relecture
- [ ] Jeudi: Dernière relecture
- [ ] Vendredi : REMISE DU RAPPORT AVANT 18H

TODO :
- [x] Faire script complet pour tester toutes les opérations et tailles d'images
- [x] Script R pour statistiques (ajouter des trucs ou rendre plus beau)
- [ ] Images pour chaque partie
- [ ] Références
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

The development of technologies allowing images captures has permited to scientists to use these new tools in many domains such as astronomy, geology, biology etc (*Add reasons why they use this tools*). The main problem to deal with images is that in most of cases they can not be used in their raw format to make analysis. Actually there are additional steps required which are called image processing. After treatment the images can reveal all the things that scientist could need.
There are many possibilities to process an image according to the final result wanted. In this report we will focus on 2D Linear Filters. Filters are used to convert an input image into an output, based on the convolution product principle (Figure 1).

![convolution_equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/09e0e66216b29a0ccd3d60a6e0f9aba3ce6fb4b2)
<span style = "font-size:14px"> **Figure 1**. Convolution product formula. This mathematical formula can be seen as the combination of two matrix in image processing.</span>


Basically, a convolution product is the result of two matrix combinations. Convolution operation needs an image and a convolution mask also called kernel. An image is actually a bidimensional matrix in whic each case correspond to a specific pixel value (Figure 2A). Its dimension has a value of *width\*height*. This matrix will be multiplied by the convolution mask which is a matrix *j\k* with *j* and *k* odds values (Figure 2B).
The result of the convolution product gives a new value to the central pixel of the kernel based on the weighted average of the surrounding pixels(Figure 2C). It leads to a new image with modified pixels values.

Through this report we will discuss three filters using this mathematical principle in imageJ software :
* Convolve
* Gaussian blur
* Mean filter

## 1. Material and Methods

### 1.1. ImageJ
To perform the experiments explained later in this report ImageJ(Reference) has been used. It is an open source software regrouping a large community and allowing images manipulations and analyses. It is widely used across the world by many researcher in all kind of domains. One thing that make the popularity of ImageJ is the possibility to easily develop plugins to automatize image processing and analyses and its well documented API (Application Programming Interface). Thus we decided to use it as the main goal of the project is to develop a web version of ImageJ.

### 1.2. Convolve
As said previously, convolution uses a kernel and an image. The problem with convolution is that the kernel can have some of its components out of the image (Figure 3A). There are some possibilities to avoid that problem (Figure 3B) :
* adding a black stripe around the image with a size of *(n-1)/2*
* starting the processing at position *(X((n-1)/2),Y((n-1)/2))*
* not processing these pixels
* extending the image by mirroring the edges

As convolution represents a major part of image processing, several algoritmhs have been developed to speed up the treatment.

The easiest way is to simply go through an image and make the convolution product between the kernel and the area it covers. This is the main way to perform convolution and it is widely used other image processing. This method is relevant for specific images types.

A second way to realize convolution is the case where the kernel used is separable. This means that the kernel can be expressed as the outer product of two vectors (Figure), one row vector and one column vector. It has been demonstrated that the product of these two vectors is equivalent to the two dimension convolution of the vectors. This is based on the associative property of convolution. The main advantage of this method is that it is faster than the "original" method. But there is a disadvantage coming with it as the kernel have to be separable. In other ways this type of convolution is associated with specific kernels.

Another way to perform convolution is to go through a step of FFT (Fast Fourier Transform). This is based on the property of convolution saying that a convolution product can be apply both in the spatial and the frequency domains. Thus the method used is to load an image and apply an FFT. Then the convolution is performed and an inverse FFT is realized to go back to the spatial domain. This method has shown more acurracy and needs less computation time for certains types of images. It is actually the process used in ImageJ. This method is slightly dependant of the implementation of FFT into a software.

By looking for plugins implementing the differents methods described above, we have been able to locate just one plugin different from the one used by default in ImageJ. This plugin is called **```Real_Convolver.java```** and implement the simple convolution into the spatial domain. It allows the use of convolution only for 32 bits images, so the totality of the benchmarking has been realized on this type of image.

### 1.3. Gaussian Blur

The Gaussian Blur also known as Gaussian Smoothing operator is a 2-D convolution operator that is used to blur images and remove detail and noise. In this sense it is similar to the mean filter, but differ by using a different kernel. Thus, using a Gaussian Blur filter consist in realising a convolution on a picture with a gaussian function.

**Gaussian Blur 2-D Equation**
![Gaussian Blur 1-D Equation](https://homepages.inf.ed.ac.uk/rbf/HIPR2/eqns/eqngaus2.gif "Gaussian Blur 2-D Equation")



Again, to speed up images treatment, algorithm have been developed. Here we are going to compare the ImageJ Gaussian Blur filter default function to a plugin, Accurate Guassian Blur. This last has been implemented for high accuracy treatement espcially for 32-bit images. Also, these methods encounter the same trouble as the convolution with edges of images when they are processed.

![Gaussian Blur 1-D Shape](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/gauss1.gif "Gaussian Blur 1-D Shape")

![Gaussian Blur 2-D Shape](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/gauss2.gif "Gaussian Blur 2-D Shape")

**Gaussian Blur 1-D Equation**
![Gaussian Blur 1-D Equation](https://homepages.inf.ed.ac.uk/rbf/HIPR2/eqns/eqngaus1.gif "Gaussian Blur 1-D Equation")




<!-- ![alt text](/path/ "Title") commande for adding local picture -->

### 1.4. Mean filter
Among the linear filters, the most common is the mean filter beacause it's easy to implement and also reliable. As it has been said before (see section 1.2 Gaussian blur), it's a smoothing filter which has the purpose of smoothing an image by blurring and removing detail and noise. Doing so, a convolution mask is used, it can be with various shape (square, rectangular or circular) and in vast majority the weights in the kernel are uniforms (meaning the initial values in the kernel are the same), but those can also be triangular (i.e.  inversely proportionnal to distance from the input sample). [^MAT2007]

For example if S*xy* represents the set of coordinates in a rectangular sub image window of size *m × n* centered at point *(x,y)*, the arithmetic mean filtering process computes the average value of the corrupted image *g(x,y)* in the area defined by S*xy*. The value of the restored image *f* at any point *(x,y)* is simply the arithmetic mean computed using the pixels in the region defined by S*xy*. In other words :

*Put figure here*
![equation aritmethic mean filter](/home/tristan/Bureau/m2/s3/bioinf_st/bioinf_phase_1/formule_mean.png "Equation")


This operation can be implemented using a convolution mask in which all coefficients have value *1/(mn)*. A mean filter simply smoothes local variations in an image. Noise is reduced as a result of blurring. The main problem of this filter is that noisy pixels—including anomalous spikes—are weighted the same as all the other pixels in the kernel. [^GAJ2011]
Because it's uses a convolution kernel, we find the same issue as describe in section *1.2 Convolve*, thus the same solutions can be applied here.

During our research, we didn't found plugin of the mean filter whose proceed differently from the original plugin. That's why the benchmarking were realised only for the mean filter already implemented in *ImageJ*.


### 1.5. Benchmark

Benchmarking were realized based on time of processing. A JavaScript script was implemented to automatize the process (Supplementary Data 1). First a warmup phase consisting in running 20 times the chosen operation is performed. After the script switch to the testing phase consisting in 20 runs of the image processing selected. This process is used on the same RGB image with a size of *1920\*1200*. This image was converted into 32 bits, 16 bits and 8 bits greyscale images and the benchmark was performed on each of these pictures. To get more data on other sizes of image, the initial picture was also resized two times. This lead to three images sizes : *1920\*1200*, *960\*600* and *512\*320*. The results are displayed in the imageJ log console and saved into a csv file.
In this case, time processing of Convolve, Mean and Gaussian blur were calculated. An R script allows to get a boxplot of the results showing the different quartils and the mean of data for the warmup and the testing phase.  The process has been repeated for the alternatives plugins found for the different filters.

Benchmarking in JavaScript is really complicated since the results are very variable and that they depend a lot on the state of the computer used for the test (Référence Article Taveau). Thus the results obtained by our method have to be taken as informative.

## 2. Results

### 2.1. Convolve
<!-- TODO : Add image showing the difference of results in spatial domain for both plugins -->
We compared two plugins, one realizing the "original" way to make a convolution and another one getting an extra step of FFT to get the result.
The first thing important to note is that the output images from one method and the other are the same one (Figure).

First, the time of processing for the different images size were computed using R (Figure). This shows a diminution of processing time the smaller the image gets.

<center><img src='./Size/size.svg' width=600></center>

In a second time a comparison between **```Real_Convolver.java```** and the convolution plugin implemented into ImageJ was performed (Figure). We can see that the plugin using the step of FFT, the ImageJ's one, takes less time to process an image no matter his size.

<center><img src='./Convolve_vs_RConvolve/vs.svg'></center>

### 2.2. Gaussian Blur
### 2.3. Mean filter


## 3. Discussion
The several benchmarks performed allowed us to observe certain behaviors for the differents plugins tested.

For the convolution part, we have not seen any difference in the output of the process. We explain this by a the associative property of convolution saying that a convolution product is the same in the spatial domain and in the frequency domain (FFT). Moreover the algorithm using an FFT before the convolution (ImageJ) seems to have better performances than the ```Real_Convolver.java``` which apply simply the convolution process through the spatial domain. As expected we saw a decrease of processing time the less the image is big. That can easily be explained by the fact the decrease in pixels needing thus less calculus for tiny images<sup>[1]</sup>.

## Conclusion
Through this work, we were able to study the differents process used for 2D Filters. The final goal is to implement the best algorithm into a "web ImageJ like" for each filter studied.

For the further steps, we decided to use the convolution algorithm found in ImageJ source code. We will also implement the recognition of separable kernels to increase a bit more the processing by creating two vectors that will be used to make the convolution.

## References
[^MAT2007]:MATT HALL. Smooth operator: Smoothing seismic interpretations and attributes. The Leading Edge. 2007 Jan;26(1):16–20.
[^GAJ2011]: Gajanand Gupta. Algorithm for Image Processing Using Improved Median Filter and Comparison of Mean, Median and Improved Median Filter. International Journal of Soft Computing and Engineering (IJSCE) ISSN. 2011 Nov;Volume-1(Issue-5):2231–307.
[1] lol
</span>
