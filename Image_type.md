# Digitization 
- Gray scale quantization: Discretization of amplitude values
- Spatial sampling: Discretizing a continouos function in terms of coordinate value  
- Uniform sampling: Sampling on a lattice
- Non-uniform sampling: Adaptive, denser a lot of details
Microscopu images 12/16 bit images; mostly 16 ==> 4096 or 65536 different levels of  gray.  

HUman can see only 32 levels of gray

# Image coding and compression 

## Redundancy 
Infomration and data are different things.
**Data** is how you encode/express your **information**.  
Redundant data does not provide additional information.  

- Coding redundancy: Some gray levels are more common.  
- Interpixel reduncancy: The same gray level covering a large area.
- Psycho-visual redundancy: 32 levels to be suitable for human eye (only for observation)  
## Compression
- Reversible (lossless): No loss of information. (2-10 times): 
    - TIFF, OME-TIFF; With annotaiton information, with 16 bits/pixel and multible channels. can use different compression methods. (COmpression: Huffman, LZW)
    - GIF; One channel and 256 colors. (Compression: LZW)  
    - PNG; 16bits/pixel 4 channels with RGB + transparency (Compression Huffman, LZW)

- Non reversible (lossy): loss of information (10-30 times): JPEG (.jpg, .jpe, .jpeg), JPEG2000 (.jp2, .jpx)  
- Vector based: Shapes of objects are defined by lines....
    - PS
    - EPS
    - PDF
    - SVG 

Bio-format is a good tool for reading/writing image data.  

References:  
[NBIS Spatial omics course](https://uppsala.instructure.com/courses/58516/pages/schedule) 
