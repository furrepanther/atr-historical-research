# Challenges of image processing before OCR/HWR

## Identifying useful image processing operations pre-OCR

The first step for any form of computational text analysis is to create high-quality machine-readable text, but even the most advanced tools for handwriting or optical character recognition struggle with low-quality scans or scans of materials that are damaged, low in contrast, or visually complex. Automated image processing can improve the final results, but there also is a danger of over-engineering the process. 

For the BYODL workshop in February 2025, we prepared two scripts to test different image enhancement options with Python:

- Test-case 1: human-written script for basic noise reduction and contrast enhancement
- Test-case 2: script including GenAI recommendations for complex image processing

While both scripts make use of similar Python packages, the AI-recommended workflow manipulates the image more, including a conversion of the colour scan to grayscale. Both scripts for image manipulation are available in the **notebooks** folders of this repository to be run in Kaggle, Google Colab, or BinderHub. Please check the [coding environments](/docs/codingenvironments.html) information page for detailed explanations.

## Working with the OpenCV library for computer vision

The most important library in both scripts is the [OpenCV (Open Source Computer Vision)](https://docs.opencv.org/4.x/index.html) library is an open source computer vision and machine learning software library. To better understand its possibilities for image manipulation, it is important to read the documentation. Not all options may be useful for preparing document scans for OCR. 

GPT4-Vision recommended the following manipulations when asked to enhance a blurred scan of early modern German print:

- [denoising](https://docs.opencv.org/3.4/d1/d79/group__photo__denoise.html), more specifically ```fastNlMeansDenoising()```
- [CLAHE (Contrast Limited Adaptive Histogram Equalization)](https://docs.opencv.org/4.x/d5/daf/tutorial_py_histogram_equalization.html)

  The OpenCV documentation includes an example of a black-and-white photograph before and after CLAHE transformation and explains that changing the global contrast
  of the image is, "in many cases, not a good idea." Contrast Limited Adaptive Histogram Equalization can help when different parts of the image vary in contrast.

- [image filtering](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html), more specifically ```filter2D()```

It is important to identify what the specific visual features of the images to be processed are to then find the suitable workflow. Experiences shared by researchers working with comparable sources can help but may be difficult to find online.

## Blogs and tutorials recommending different workflows

Several academic, private, and commercial blogs also discuss the use of OpenCV but recommend divergent steps for OCR preparation. Here are some examples:

- [NextGenINVENT: 7 steps of image pre-processing to improve OCR using Python](https://nextgeninvent.com/blogs/7-steps-of-image-pre-processing-to-improve-ocr-using-python-2/)
- [Geeks for Geeks: OpenCV Tutorial in Python](https://www.geeksforgeeks.org/opencv-python-tutorial/)
- [Alexander Obregon: How to Process Images with Python Using OpenCV](https://medium.com/@AlexanderObregon/how-to-process-images-with-python-using-opencv-6894010d78e4)
- [Richmond Alake: OpenCV Tutorial: Unlock the Power of Visual Data Processing](https://www.datacamp.com/tutorial/opencv-tutorial)

More general image processing advice is also given in Mageshwaran R.'s [Survey on Image Preprocessing Techniques to Improve OCR Accuracy](https://medium.com/technovators/survey-on-image-preprocessing-techniques-to-improve-ocr-accuracy-616ddb931b76) and in other publications collected in the [pre-OCR image processing folder](https://www.zotero.org/groups/5646174/atr_history/collections/4W36A6A4) of our Zotero group library.

## In-built image processing in OCR4all

OCR4all offers some in-built image processing. For early modern colour scans with page backgrounds in tones of yellow and brown, the following settings are recommended:

### Skew correction

**Skew angle estimation (degrees)**  defines the maximum rotation range (± value) of your pages. Avoid unnecessarily high values, as they may distort segmentation.

- Recommended default for flatbed scans: `2`
- If minor visible tilt: `3`
- Only go beyond `5` for strongly skewed camera images
- Increase slightly only if fine tilt remains uncorrected

### Parallel processing

**Number of parallel threads** for processing are preset to 20. To check if this reflects the number of CPU threads available on your system,
you can run ```nproc```. You may want to reduce this number slightly if your system becomes slow or memory usage is high.

### Thresholding and background handling

**Threshold** determines lightness and controls binarisation sensitivity. This is one of the most important steps.

- Start with: `0.6`
- Typical range: `0.55–0.65`

Higher → cleaner white background, risk of losing thin strokes  
Lower → more noise, better preservation of fine ink

**Zoom for page background estimation**

This setting improves adaptation to uneven paper tone.

- Recommended: `0.6–0.8`
- Start with: `0.7`

**Ignore border for threshold estimation**

This option can be useful if edges of your paper are darker.

- Recommended: `0.15`
- Increase to `0.2` if margins are shadowed

#### Text mask estimation

This sets the scale for estimating mask over text regions, separating text areas from the background.

- Keep default: `1.0`
- Adjust only if marginalia or edge text disappear

#### Noise filtering

The **percentage for filters** controls the aggressiveness of noise removal. Reduce slightly if fine serifs or diacritics vanish.

- Recommended: `75–85`
- Start with: `80`

**Range for filters**

- Keep default: `20`
- Adjust only if small speckles remain or small characters are removed

#### Black / white estimation

This is critical for yellowed paper. To define a **percentile for black estimation**, start with `7`. Increase to `8–10` if ink appears faded
The **percentile for white estimation** should first be set to `93`. Settings between 92 and 95 work well for most early modern papers. This setting
helps normalise yellow/brown background toward white.

#### Grayscale processing

**Forcing grayscale processing** is recommended for colour scans. It ensures proper thresholding instead of treating the image as a strict binary.

#### Error checking

**Disable error checking on inputs** is set as the default. What is the advantage of turning it on?

#### Summary of default configuration for most early modern colour scans

For most early modern colour scans, start with the following settings:

```
Skew: 2
Threads: 20 (if system supports 20 logical cores)
Threshold: 0.6
Zoom background: 0.7
Ignore border: 0.15
Filter percentage: 80
Filter range: 20
Black percentile: 7
White percentile: 93
Force grayscale: enabled
```

Do not over-optimise for a perfectly white background. Preserving thin strokes, ligatures, long s (ſ), and marginalia is more important than eliminating all background tone.
Always test on a small sample before batch processing.

## Could (Gen)AI help in making an informed decision?

Rather than letting (Gen)AI decide on a suitable workflow based on sample images, it might be a more efficient approach to describe problematic aspects of the scanned sources first and then ask an AI chatbot to suggest the best approaches for each of them. This may reveal that not all images collected for a project can be processed in the same way and that images may have to be processed in batches. Grouping the images should then depend on visual aspects and not on their content. Projects with very diverse sources will most likely not be able to work with a one-fits-all image manipulation algorithm. 
