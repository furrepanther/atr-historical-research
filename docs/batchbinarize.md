## Binarization of image files

### Why binarization can improve OCR results

Binarization in Optical Character Recognition (OCR) means converting a grayscale or color image into a black and white (binary) image. Ideally, this conversion applies a threshold value to clearly distinguish between foreground (text)
and background (paper and stains) to improve OCR accuracy. 

### Performing batch binarization with Kraken

There are several ways to binarize images. One option is to use the Kraken OCR tool, which also allows you to binarize an entire folder of images at once. The binarization package for Kraken was developed in 2014 and 2015 by Benjamin Kiessling
and Thomas M. Breuel, licensed under the Apache License, Version 2.0 (the "License"):

[https://github.com/mittagessen/kraken/blob/main/kraken/binarization.py](https://github.com/mittagessen/kraken/blob/main/kraken/binarization.py)

To use the package for batch binarization, you have to put all the image files into a single folder.
Then you can use the ```kraken.binarization.nlbin``` function (in Python) to convert the images. Running the Python code for Kraken in a virtual environment is highly recommended. To enter your virtual environment (on Linux and Mac), use
```source ~/kraken-env/bin/activate``` in the terminal, replacing ```kraken-env``` for your own environment name. Then you can run the Python code with the following command:

```python batch_binarize.py```

The Python file itself should contain this code:
```
# import packages for image processing

import os
from pathlib import Path
from PIL import Image
from kraken import binarization

# define paths for input and output files
input_dir = Path.home() / "/home/monikab/Documents/IMG_automobiles" # enter your own path to your image folder here
output_dir = input_dir / "IMG_binarized" # enter your own output path here
output_dir.mkdir(exist_ok=True)

# define permitted image formats
extensions = (".jpg", ".jpeg", ".png", ".tif", ".tiff")

# process all images in folder
for img_path in input_dir.glob("*"):
    if img_path.suffix.lower() in extensions: # only process files with permitted file extension
        try:
            print(f"[+] Binarizing {img_path.name}") # status info will be shown in terminal
            img = Image.open(img_path)
            bin_img = binarization.nlbin(img)
            out_path = output_dir / img_path.name
            bin_img.save(out_path)
        except Exception as e:
            print(f"[!] Failed to process {img_path.name}: {e}") # status info will be shown in terminal

print("\n Binarization complete. Files saved in:", output_dir) # status info will be shown in terminal
```
As you can see in the code, the new file will be saved to the output folder you define as ``òutput_dir```. You can then also use these images within other OCR software.
