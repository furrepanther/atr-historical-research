## OCR model training

### Model training in Transkribus

As a beginner-friendly tool, Transkribus offers GUI-based OCR model training within their Lite platform. 
Upload your documents, manually transcribe at least 100–150 lines, and verify correct line segmentation. 
Once enough ground truth is created, go to "Models" → "Train a model", select your training set and optionally a base model to fine-tune. 
You can adjust language settings and metadata. Submit your job to the Transkribus server, which handles training (based on HTR+ or PyLaia).
Training may take hours to days depending on data size and the job priority. Once done, the model is available for use within your account.
You can run it on new documents, export results, or publish the model. Transkribus is easy to use but proprietary. While it offers many pre-trained models, especially for European languages, 
it is not entirely transparent and models cannot be used outside of the ecosystem.

### Model training in Kraken

Christine Roughan has created a guide on how to train and implement OCR models using Kraken. Her advice is to start with a pre-trained model and improve it rather than starting anew.
While Roughan's tutorial uses Arabic text as an example, the model training process is the same for other languages. To train a model in Kraken, prepare binarized line images with matching 
UTF-8 transcriptions (one .png + .txt per line). Store them in a folder like training-data/. Install ketos via ```pip install ketos`` in your Kraken environment. Then train with:

```ketos train -f training-data/*.png -o model.mlmodel --epochs 30```

An epoch is one complete iteration through your entire training dataset. If you train for 30 epochs, the model will have seen each training line 30 times.
This gradually improves the model performance. You can also use ```--resize``` and ```--threads``` to adjust image handling or speed. Optionally evaluate using ```ketos test```. 

Using kraken to train your own ocr models. (2019, November 5). The Digital Orientalist. https://digitalorientalist.com/2019/11/05/using-kraken-to-train-your-own-ocr-models/

### Model training in e-scriptorium

eScriptorium offers a web interface for layout analysis, transcription, and model training. After uploading images (TIFF, PNG), manually segment or correct auto-segmented lines.
Add or import ground truth transcriptions for each line. Once annotated, go to “Training”, select your training dataset, and choose a base model. Configure parameters like number of epochs
and batch size, then run training directly in the interface. eScriptorium tracks training progress and integrates models into your workspace automatically. 
You can test models on pages immediately. For advanced users, you can export data as PAGE XML and train externally. Model reuse, model sharing, and multilingual support are strong features.

says, S. (2023, September 26). Train your own ocr/htr models with kraken, part 1. The Digital Orientalist. https://digitalorientalist.com/2023/09/26/train-your-own-ocr-htr-models-with-kraken-part-1/

Train your own ocr/htr models with kraken, part 2. (2023, November 3). The Digital Orientalist. https://digitalorientalist.com/2023/11/03/11400/

### Model training in OCR4all

OCR4all is a GUI-based OCR toolkit built on Tesseract and LAREX, designed for historical printed text. It runs best on Linux or a Linux server (Kubernetes) but can also be installed on Windows and Mac using Docker.
Start by uploading page images, then use LAREX for semi-automatic layout analysis (zones, columns). Then, pair regions with ground truth transcriptions.
Once you have at least 100–200 line-region + text pairs, go to the ```Model Training``` tab and initiate training. OCR4all uses Tesseract's training pipeline with wrappers.
You can train from scratch or refine existing models (e.g. historical Fraktur). Once training is complete, apply your model to new scans within the interface.
OCR4all produces ALTO/XML and TXT output and supports quality evaluation (WER/CER).

### Further reading

Clausner, C., Antonacopoulos, A., & Pletschacher, S. (2020). Efficient and effective OCR engine training. International Journal on Document Analysis and Recognition (IJDAR), 23(1), 73–88. https://doi.org/10.1007/s10032-019-00347-8

Training a kraken model—Kraken documentation. (n.d.). Retrieved June 4, 2025, from https://kraken.re/2.0.0/training.html

OCR4all | Setup guide, user guide, developer documentation and more. (n.d.). Retrieved June 4, 2025, from https://www.ocr4all.org/

Train own mixed models · Issue #66 · OCR4all/OCR4all. (n.d.). GitHub. Retrieved June 4, 2025, from https://github.com/OCR4all/OCR4all/issues/66





