# Analysing ISS data
## Preprocessing  
**Aim:** preparing raw image that can be used for decoding the identity of each spot.  
**Approach:**  Removing (10%) overlap between imaging FOV (Field of View) by image stitching and retiling of stiched images.  For cycle alignment and stiching **ASHLAR** is used.  
**Tools:** ASHLAR.  
## Deconvolution (deblurring)  
**Aim:** Post-processing step to improve the quality of the picture to be sharper and more in focus. 
**Approach:** The collecetd image from the sample is distotred or convoluted or it may appear blurry/ out of focus. The reason behind getting blurry images is due to the fact that lights spreads out or becomes diffracted when passes from the slide/sample through the optical path of the microscope.  
By knowing the imaging system, we can model light diffraction. 
There are different algorithms:

| Algorithm | Description | Pros | Cons |Examples|
|-----------|-------------|------|----|----------|  
|Inverse filters| back-calculate the original (deconvoluted) image|simple, fast, least computationally expensive| sensitive to noise, introduces artefacts|Wiener deconvolution, Tikhonov filtering, Linear least squares, Naive inverse filtering|
|Iterative| estimating the original image by blurring image using the point spread function. Then comparing the blurred estimate to real image.| Better performance than Inverse filters| slow and computationally expensive| Richardson-Lucy, Jansson-Van Cittert, Non-linear Tikohono filtering, Landweber| 
|Blind deconvolution|Without known PSF <sup>*</sup>, PSF is calculated from the input image.| Simpler| Not common|
|Deep learning based methods|Using Convolutional Neural Network (CNN) which is based on blind deconvolution|more accurate estimation of PSF|It may suffer from the same limitation as blind deconvolution|

<sup>*</sup> Point spread function; PSF is a mathematical model of how a point source of light/object is imaged in the real world.

**Tools:**  
- ImageJ plugins such as DeconvolutionLab2, Iterative Deconvolve paired with Diffraction PSF3D.  
- Huvygens software (commercial)
- AutoQuant (commercial)
- Imaris (Commercial)
- Velocity (commercial)
- Flowdec; this library contains TensorFlow implementation of image and signal deconvolution algorithms. Richardson-Lucy (which is an iterative method) has been implemented. This is GPU based which is faster. 
- deconwolf
- skimage  

## CARE: content-aware image restoration  (instead of deconvultion) 
**Aim:**  Post-processing step to improve the quality of the picture to be sharper and more in focus. 
CARE similar to deconvolution is commonly used for image restoration but it is faster.  This method relies on machine learning techniques to learn from large datasets and generalize the restoration process. In short the steps are:
- Identifying distortions (blur, noise, or artefacts)    
- Estimating undistotred version of original image by advanced algorithms (machine learning techniques)  
- Restorating the original image by denoising, deblurring, inpainting or super-resolution  

## Decoding
**Aim:** Decode identity of each spot.  
**Approach:** 
- Prepare data in SpaceTX format   
- Create reference images by projecting signal images   
- Aligment step to ensure level alignment based on reference image from each round or nuclei stain (optional)    
- Top hat filteirng  (optional)  
- Normalize images  (channel intensities)  
- Localize spots from all cycles and channels  
- Record the intensity of each spot and each channel and cycle    

**Tools:** starfish 

## Cell segmentation  
**Aim:**  To label individual instances of cells.  
**Approach:**   
- Staining information of cell landmarks:  
    - Nucleus (DAPI)    
    - Cell body (Nissl, Poly-A, max projection); Might have limited power when the cells are very close or overlapping.    
    - Cell membrane (E-cadherin & &beta;-catenin)  
- Computational methods  
    - Binning the data    
    - Watershed segmentation  
    - Deep learning (DeepCell, U-net, cellPose, starDist)  
    - Random Forest (Ilastik)  
    - Read-based  

**Check the following videos:**
https://www.youtube.com/watch?v=3yk9sBja7YI
https://www.youtube.com/watch?v=F6sfm1i8OfY
https://www.youtube.com/watch?v=fVeW9a6wItM
https://www.youtube.com/watch?v=un7QvhXZ_G4

**Tools:** 
- spage2vec; Graph Neural Network based (using molecule distance)
- SSAM;  Using gene expression as likelihood of cell location by treating "local maxima" as analogues of single cell which in a way by-pass the segmentation problem. It is like single pixel vs single cell.  Steps: 1) calculate spatial mRNA density 2) identify cell-type signature 3) generate cell-type map (by pearson correlation pixel vs cell gene expression)  4) construct tissue domain map. It is important to choose an optimal bandwith to keep the topology or shape or location of the cells. Low badnwidth shows the molecules but the cell become hetergenous while high bandwidth will show more dense but masking the right location of cells.  
- Baysor; Graph-based method using a probabilistic model.  It uses KNN. It creates spatial neighbour graph
- SSAM-lite; it is similar to SSAM but as a web server.  
- plankton.py; It is a framework to analyze spatial relationships between molecules in single molecular spatial omics data.  It takes decoded spot (starfish output), for QC uses _scanpy_, and return _anndata_ object for downstream analysis (squidpy for example neighborhood enrichment analysise). Similar to _anndata_ it uses _sdata_.  It performs radial distance-based modellin of local environment (similar to SSAM). It calculates local-neighborhood models for each of the moleculas then it is fed to UMAP.   

Augmented cell segmentation by using scRNASeq data such as:  
    - Baysor: works with DAPI prior  
    - pciSeq: Poisson point process + negative binomial (from Mats Nilsson group)
    - Sparcle: multivariate Gaussian distribution   
    - JSTA: joint segmentation and typing by using deep learning on top of watershed segmentation  


In brief: One can analyse the data without segmentation. 

*Note: DAPI stains nuclei while most mRNA is in the cytoplasm*  

References:  
NBIS Spatial course  
[Elen Bochard weblog](https://blog.biodock.ai/image-deconvolution/)  
