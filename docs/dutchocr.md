## OCR challenges when working with less common languages

### Quality assessment of OCR-text provide by Delpher

In a test with MA student Rienke Vos (Maastricht University), who had collected Dutch 19th and early 20th century newspaper articles for her thesis in Digital Cultures, we compared different approaches to automatically transcribing Dutch print.
The OCR text for the collected newspaper articles on Delpher was too error-prone for a mere OCR-post-correction, which is why we decided to redo the transcription from scratch.

### Mixed results with TRANSKRIBUS community models

At first, we ran a test in [TRANSKRIBUS](https://www.transkribus.org/) but found that a general community model for print and Dutch models for HTR (trained on handwritten Dutch letters) retrieved mixed to poor results as well. Although the test print was relatively clean, consisted in a single column, and used fairly standard fonts for a newspaper from the 1920s and 1930s, the model struggled with basic line segmentation, word segmentation, and character recognition. "Wetenschappelijke gegevens”, for instance, were misread as “wetonschap gev". 
We did not pay for one of the so-called "super models", however, and felt that our input material was too diverse to train the model on our own data, but decided to pre-process the image files and try again. For prints in German and English, for instance, TRANSKRIBUS can often deliver good results without additional model training, but OCR quality is far less reliable for languages like Dutch.

### Low accuracy with pre-trained Kraken models

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
We, therefore, tried CATMuS Print Fondue Large for print text instead. Here, loading the package, binarizing the image and segmenting was successful, but the process took a while on an older Linux machine because we could not rely on CPU acceleration. We received the following warning:
```
Could not initialize NNPACK! Reason: Unsupported hardware.
```
NNPACK is a CPU acceleration library used by PyTorch to speed up deep learning models. Our hardware did not support NNPACK, so PyTorch resorted to a slower method. Kraken still completed the OCR process successfully and wrote the extracted text to a new file. However, the results were even worse than those we got from Transkribus because the automated segmentation in Kraken often only recognised fairly dark print as text while text in lighter shades was ignored, even after image binarization. To retrieve better results in Kraken, the images would need more advanced pre-processing, including contrast enhancement and noise reduction, but training an existing model for specific newspaper fonts may also be necessary.

### High accuracy with OCCAM's PERO model

Encouraged by Arnoud Wils, we also tested the OCR tool [OCCAM](https://ai4culture.eu/resources/tools/122) developed by Crosslang for AI4Culture. This tool was developed in Belgium and makes use of two models trained on Dutch / Flemish texts. We first uploaded test images to check the performance of their Loghi model, which was disappointing: only individual characters were transcribed and in some cases, we received blank output. Then we switched to the PERO model, which immediately gave us very good results. All lines in single-column images were correctly identified and the transcription was mostly correct. For double-column scans, we also got a decent start although the post-correction needed was more extensive. Dutch "ij" combinations and older spelling variants caused OCR-errors, but they could easily be corrected. OCCAM even offers in-built OCR correction a) manually, b) using two different spellchecking algorithms, and c) using an LLM. It is important to carefully consider which option serves your needs best.

### Post-processing OCR results outside of OCR software

Of course, it is also possible to download the raw OCR text from OCCAM and other OCR platforms to perform the spelling correction locally with packages like Python's spellchecker. The [by5-base-dutch-ocr-correction](https://huggingface.co/ml6team/byt5-base-dutch-ocr-correction) transformer model is also an option. It is a fine-tuned version of the [ByT5 model](https://huggingface.co/docs/transformers/en/model_doc/byt5), which is a variant of Google's T5 (Text-to-Text Transfer Transformer) model. The ByT5 model is unique because it operates at the byte level, which can be particularly useful for handling noisy text data, such as text obtained from OCR processes. For more details, consult the [official Google Research documentation on Github](https://github.com/google-research/byt5). Like any machine learning model, however, the ByT5 model may have biases present in its training data, and sensitive data may need to be anonymised before running the model. It's important to evaluate the model's outputs for any potential biases and consider the ethical and legal implications of using the model in your research.

### HTR for Dutch sources

For handwritten texts, Arnoud Wils suggests [Arkindex](https://doc.arkindex.org/), with which he has already performed tests and got impressive results.
 
### More information

Algun, S. (2018, December 6). Review for tesseract and kraken ocr for text recognition. Medium. https://medium.datadriveninvestor.com/review-for-tesseract-and-kraken-ocr-for-text-recognition-2e63c2adedd0

We build internet. (n.d.). Kraken, the unknown python ocr system. Retrieved June 4, 2025, from https://www.webuildinternet.com/2016/10/01/kraken-the-unknown-python-ocr-system/


                            





