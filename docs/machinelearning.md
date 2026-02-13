## Why we need advanced machine learning for ATR

State of the art automated text recognition makes use of advanced machine learning. A mere rule-based approach for character recognition would not allow the transcription of a variety of
fonds and handwritings, especially because characters can even vary within the same text. This is why automated text recognition now relies on neural networks (NNs), among which convolutional neural networks are the most common. (Alvin, 2024)
Neural networks were first developed in the 1950s and have been increasingly used since the 1980s. 
First proposal for neural networks in 1944 bz Warren McCulloch and Walter Pitts.
In 1954, Belmont Farlez and Wesley Clark (MIT) created the first *artificial neural network*.

Neural networks can abstract from dirty input data.

## Deep learning as another step forward

Deep learning is a type of articifical intelligence that goes beyond machine learning by letting neural networs imitate the human mind so that the algorithms no longer rely on existing patterns. (Huynh & Corfe-Tan, 2019; Li, 2022) This is especially useful in handwriting recognition (HTR) where we typically see the greatest variations not just between texts, but also within a single text written by one author. (Hodel, 2021) Deep learning includes that the machine goes beyond mere text recognition but can link text to meaning and assess logic. The algorithms behind deep learning use so-called deep neural nets, which can also combine text recognition with translations into other languages. (Ahamed & Tahmid, 2025) Companies experimenting with such technology include [omni:us](https://omnius.com/blog/omnius-ai-models/). This company offers automation services for insurance providers worldwide.

>With this AI model, we also have the benefit of transcribing handwritten text into electronic text. Simultaneously, we utilize OCR (Optical Character Recognition) when extracting texts from documents, which has worked perfectly. However, OCR is limited to printed text – it is pivotal in electronically converting printed text, but when it comes to handwritten texts on documents, OCR fails to provide the same accurate results that HTR is capable of. This introduction of HTR has boosted our abilities in extracting information from documents, as well as, the performance of the other AI models that we currently use.

Challenges and error margins... One concern in the training of deep learning algorithms is the large amount of data needed. Some companies train on personal documents that include customer information etc.

## Reverse OCR

The program *My Text in Your Handwriting* (http://visual.cs.ucl.ac.uk/pubs/handwriting/) developed at University College London can imitate handwriting. 

>We present an algorithm that renders a desired input string in an author's handwriting. An annotated sample of the author's handwriting is required; the system is flexible enough that historical documents can usually be used with only a little extra effort. Experiments show that our glyph-centric approach, with learned parameters for spacing, line thickness, and pressure, produces novel images of handwriting that look hand-made to casual observers, even when printed on paper.

Darks sides... ethical concerns...

## References

Ahamed, M. R., & Tahmid, S. M. A. (2025). Deep learning-based intelligent language translation systems. Journal of Global Knowledge and Innovation (JGKI). https://doi.org/10.5281/zenodo.14635266

Alvin, Tan Pengshi. Optical Character Recognition (OCR) with CNN-LSTM Attention Seq2Seq | Towards AI. 8 Sept. 2024, https://towardsai.net/p/artificial-intelligence/optical-character-recognition-ocr-with-cnn-lstm-attention-seq2seq.

Hodel, T. (2021, January 27). Wie Paläographie und Deep Learning zusammen gedacht werden [Conference presentation slides]. Werkstattgespräche Digital Humanities, Universität Paderborn. https://doi.org/10.5281/zenodo.4475549

Huynh, R., & Corfe-Tan, W. (2019). Introduction to deep learning (Version 2019-07-30). University of Auckland. Retrieved February 13, 2026, from https://github.com/UoA-eResearch/deep-learning-tutorial-2019

Li, C. (2022). Special character recognition using deep learning methods (Master’s thesis, Auckland University of Technology). Tuwhera Open Research. https://hdl.handle.net/10292/15042

