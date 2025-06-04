## OCR challenges when working with less common languages

### Quality assessment of OCR-text provide by Delpher

In a test with MA student Rienke Vos (Maastricht University), who had collected Dutch 19th and early 20th century newspaper articles for her thesis in Digital Cultures, we compared different approaches to automatically transcribing Dutch print.
The OCR text for the collected newspaper articles on Delpher was too error-prone for a mere OCR-post-correction, which is why we decided to redo the transcription from scratch.

### Reading Dutch newspapers with TRANSKRIBUS

At first, we ran a test in TRANSKRIBUS but found that the only available community model for Dutch print retrieved mixed to poor results as well. Although the test print was relatively clean, consisted in a single column, and used fairly standard fonts for a newspaper from the 1920s and 1930s, the model struggled with basic line segmentation, word segmentation, and character recognition. "Wetenschappelijke gegevens”, for instance, were misread as “wetonschap gev". 
We did not pay for one of the so-called "super models", however, and felt that our input material was too diverse to train the model on our own data, but decided to pre-process the image files and try again.
For prints in German and English, for instance, TRANSKRIBUS can often deliver good results without additional model training, but OCR quality is far less reliable for languages like Dutch.

### Dutch print OCR with Kraken

In parallel, we let a general-purpose [Kraken](https://kraken.re/main/index.html) model transcribe the same raw pages. Kraken has several [OCR and HTR models available on Zenodo](https://zenodo.org/communities/ocr_models/records?q=&l=list&p=1&s=10&sort=newest), but there isn’t a high-quality pretrained historical Dutch model yet. So we decided to start with McCATMuS, a generic transcription model for handwritten, printed and typewritten documents from the 16th century to the 21st century. Running Kraken in the Linux command line, we performed three actions. First, we binarised the input image, then we analysed the layout, and then we applied the actual OCR model:

```
kraken -i /home/monikab/kraken-env/img1.jpg /home/monikab/kraken-env/img1.txt binarize segment -bl ocr -m McCATMuS_nfd_nofix_V1.mlmodel
```

To run the operation via a Python file, the following packages are needed:

```
# import packages
from kraken import binarization, pageseg, serialization, rpred
from PIL import Image
import os

```
The advantage of using Kraken is that it is more transparent and allows more user control over segmentation and model training (via [ketos](https://kraken.re/2.0.0/ketos.html)). The first test with McCATMuS, however, failed:

```
ERROR    
Failed processing                     
kraken.py:433
                             /home/monikab/kraken-env/img1.jpg:                 
                             asdict() should be called on                       
                             dataclass instances  
```


                            





