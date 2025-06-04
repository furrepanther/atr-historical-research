## OCR challenges when working with less common languages

### Dutch newspaper OCR

In a test with MA student Rienke Vos (Maastricht University), who had collected Dutch 19th and early 20th century newspaper articles for her thesis in Digital Cultures, we compared different approaches to automatically transcribing Dutch print.
The OCR text for the collected newspaper articles on Delpher was too error-prone for a mere OCR-post-correction, which is why we decided to redo the transcription from scratch.
At first, we ran a test in TRANSKRIBUS but found that the only available community model for Dutch print retrieved mixed to poor results as well.

Although the test print was relatively clearn, consisted in a single column, and used fairly standard fonts for a newspaper from the 1920s and 1930s, the model struggled with basic line segmentation, word segmentation,
and character recognition. "wetenschappelijke gegevens”, for instance, were misread as “wetonschap gev". We did not pay for one of the so-called "supermodels" and also felt that our input
material was too diverse to train the model on our own data, but decided to pre-process the image files and try again.

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


                            



TRANSKRIBUS, especially in its browser-based
light version, has an intuitive user interface and is very beginner-friendly, but also much more of a black box. Although the project started as an EU-funded research project,
the tool is now under Read Coop, a for-profit business. Therefore, TRANSKRIBUS has introduced a subscription model and gives users who pay priority access to computational resources
and better models. Also, they are marketing the tool as an AI-powered, universal platform for text recognition, which raises high user expectations. In reality, community models 
often underperform on less common data types, such as multilingual materials.
Unfortunately, there is also little transparency about the quality of the community models, their training data, or performance measures. As mentioned before, we cannot say anything about their "supermodels" because we never paid to use them, but the overall promises of "AI" seem exaggerated when the models offered are still trained for rather narrow use cases. 
For prints in German and English, for instance, TRANSKRIBUS can often deliver good results without additional model training, but OCR quality is far less reliable for languages like Dutch.

