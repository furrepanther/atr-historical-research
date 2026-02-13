# OCR4all - an open source OCR alternative

## What is OCR4all

[OCR4all](https://www.ocr4all.org/) offers "Optical Character Recognition (and more) for everyone". It is entirely free and open-source, running most smoothly on Linux machines and Linux-based servers.
Installations on Windows and Mac with Docker are also possible, but this may be a bit more challenging. The advantages of OCR4all are that it is flexibly applicable, fully transparent,
and integrates layout (with Larex) as well as options for text annotation. For installation instructions, refer to the [official OCR4all documentation](https://www.ocr4all.org/).  

## Creating and using an institutional instance of OCR4all

In 2024, Arnould Wils (FASoS Maastricht) set up an institutional instance of OCR4all for researchers at the Maastricht Faculty of Arts and Social Science. 
This instance ran on the university's Data Science Research Infrastructure (based on Kubernetes) and could be accessed by institution members via VPN. If you do not have the option to run DSRI locally, you can ask your ICT service for help and find out if a similar institutional instance is possible at your workplace. Please also note that using an institutional instance will most likely require an additional OpenShift workflow when your research institute uses Kubernetes clusters.

## Running OCR4all locally (ideally on Linux)

When running OCR4all locally, you work in **two environments**:

### 1. Local Ubuntu Terminal

The terminal is for infrastructure and file management.

- Starting or restarting the Docker container
- Creating project folders (optional)
- Copying image files into the project directory
- Checking container status

### 2. OCR4all Web Interface (Browser)

The browser interface is for scholarly work and OCR processing.

- Creating projects (recommended)
- Running preprocessing
- Performing segmentation
- Running recognition
- Correcting transcripts
- Exporting results

### Accessing OCR4all

If everything is set up properly, you are able to restart OCR4all by using:

```
sudo docker start ocr4all
```

If the container is running, open the following URL **in Google Chrome or Chromium**:

[http://localhost:1476/ocr4all/](http://localhost:1476/ocr4all/)

Other browsers may cause rendering or functionality issues. If the interface does not load, restart the container **from your local Ubuntu terminal** (not inside the browser):

To verify that OCR4all is running, you can  use the following terminal input:

```
docker ps
```

You should see `ocr4all` listed.

### Where OCR4all stores data

All OCR4all data is stored on your machine in:

```
~/ocr4all/data/
```

Each project corresponds to one directory. This directory persists independently of the container.  
If you remove the container but keep this folder, your data still remains safe.

### Creating a new project

You can create projects either via the GUI (recommended) or via the terminal.

#### Option A – Browser (recommended)

1. Open:
   [http://localhost:1476/ocr4all/](http://localhost:1476/ocr4all/)
2. Click **Create Project**
3. Choose a meaningful name

#### Naming conventions

- Lowercase letters only
- No spaces
- No special characters
- Descriptive and specific

Examples:

```
mob_letters_1705
field_notes_limburg_2015
```

Avoid vague names such as:

```
test
ocr_project_1
```

OCR4all automatically creates:

```
~/ocr4all/data/<project-name>/
```

If the `input` directory is missing, create it in the terminal:

```
mkdir -p ~/ocr4all/data/<project-name>/input
```

---

#### Option B – Terminal

Alternatively, create the directory manually:

```
mkdir -p ~/ocr4all/data/MyProject/input
```

After refreshing the browser, the project should appear.

#### Uploading image files in the terminal

File upload does **not** happen in the browser. You must copy files into the project’s `input` directory using:

```
cp /path/to/your/images/*.tif ~/ocr4all/data/MyProject/input/
```

Or use your file manager and drag files into:

```
~/ocr4all/data/MyProject/input/
```

Permitted file formats include TIFF, PNG, or JPG. This image files are stored directly in the `/input/` folder.
You must not create nested subfolders and cannot provide ZIP archives, which is why you need to pay special attention
to logical and consistent file naming.

```
page_001.tif
page_002.tif
```

After copying files, refresh the browser. Pages should now appear under **Overview**.

#### Processing the images in the browser interface

All processing happens in the OCR4all web interface. The feature ["Overview"](https://www.ocr4all.org/guide/user-guide/project-start-and-overview) gives you "a tabular presentation of the project’s ongoing progress". Here, each row represents an individual page with a unique identifier. The columns document the workflow progress:

> Once a particular step has been executed, it will appear as completed (green check mark) in that work stage’s specific column. (...) With the button "Export GT" (top right) all data created in the course of the project can be exported and packed as a zip folder within 'data'.

#### Pre-processing

According to the OCR4all documentation, the best results are achieve when original images are converted into "straightened binarized or greyscale images". OCR4all also offers in-built noise removal, but it is, of course, possible to manipulate images before uploading them to OCR4all. Details on pre-processing operations can be found in the [workflow](https://www.ocr4all.org/guide/user-guide/workflow) section of the OCR4all user guide.

## Segmentation with LAREX

In automated text recognition, segmentation is necessary to add "structural information about the layout regions (type and position) as well as reading order".
In the OCR4all interface, you can perform semi-automated layout analysis with [LAREX](https://github.com/OCR4all/LAREX). LAREX is an open-source tool specifically developed for early printed books. According to the Github documentation, "it uses a rule based connected components approach which is very fast, easily comprehensible for the user and allows an intuitive manual correction if necessary." Segmentation assigns:

- layout regions
- reading order
- structural categories

If the automated layout recognition with LAREX fails, you can try and segment your scans manually. All these options can be selected in the collapsing menu on the left-hand side of your screen. Manual correction is often needed for:

- marginalia
- multi-column layouts
- early modern print
- handwritten material

### Recognition (OCR / HTR)

In the web interface, you can finally move on to run a transcription model once your layout recognition is complete. Under the menu item **Recognition** in the right sidebar, you will find your files for which all pre-processing steps have been successfully completed. Here you can select those scans for which you want to create an automated transcript using one OCR model. Ideally, the selected image files should be similar in style, e.g. hand-written by the same person or printed by the same publisher. Go to **line recognition models** (under **available**) to find models or model packages. You will be able to choose from different historial and modern font types. The OCR4all developers "expressly advise the use of a model package, where five models simultaneously work and interact with each other! This is much preferable to using only one model at a time." Finding suitable models for your use case is made easier via a search function, where you can look for basic keywords like *antiqua*  or *fraktur*.

While you are encouraged to combine several models, you can only use models from the **same model family / training group** simultaneously. Otherwise OCR4all will give you an error. Models trained with different preprocessing pipelines are incompatible. Mixing unrelated models (e.g. historical Fraktur + 19th-century Fraktur + handwriting) will either fail technically or degrade results. When working on pre-1800 German print, for instance, select several models from the **same historical Fraktur group** (e.g. `deep3_fraktur-hist/*`). Avoid mixing with `fraktur19/*` or `htr-*` models unless the material requires it. Adding too many models does not automatically improve accuracy; compatibility matters more than quantity. After running the recognition process, the final results will be displayed under the menu item **ground truth production**.

Workflow summary:

1. Go to **Recognition**
2. Select processed pages
3. Choose a **model package**
4. View results under **Ground Truth Production**

#### Challenges in text recognition

Even with carefully selected models, decorative headlines, ornamental initials, woodcut typography or stylised characters will still be difficult to transcribe. The reasons are that training corpora rarely include ornate typographic display forms and that those vary considerably. Also, the position of decorative capitals often deviates strongly from baseline letterforms. Line segmentation may fragment ornamental shapes. For research purposes, such elements may require manual correction.

#### Export results from OCR4all

OCR4all does not provide direct plain text export. The only official export format is:

> **Ground Truth (GT) export**

This produces a ZIP archive containing:

- Processed page images
- PAGE-XML files (layout + recognition)
- Associated metadata

There is no built-in `.txt` export option, which is why you will need additional software.

#### Reading plain text from PAGE-XML files

PAGE-XML is a structured layout format that matches text to images, not a human-readable text format.

Important characteristics:

- Each text line is stored as a `<TextLine>` element.
- Recognition results are stored inside `<TextEquiv>` elements.
- Multiple `<TextEquiv>` elements may exist per line (alternative hypotheses).
- Each `<TextEquiv>` contains a `<Unicode>` element with text.

If all `<Unicode>` elements are extracted blindly, the result may contain an illegible mix of recognition hypotheses including isolated character-level fallbacks. This can lead to output where words appear broken up into individual letters.

To reproduce OCR4all’s console output, based on the highest transcription confidence, your extraction must iterate over `<TextLine>` elements and decide which of them should be preferred. Preference should normally be given to `<TextEquiv>` with `index="1"`.

#### XML parsing with Python

For structured extraction from PAGE-XML:

- **lxml** (Python package) is recommended.
- It supports XPath, namespace handling, and structured traversal.
- It is lightweight and well suited for research workflows.

#### Early modern characters in plain text output from PAGE XML

PAGE-XML preserves Unicode faithfully. As a result, the text you will be able to extract is not normalised to modern conventions but may contain long s (ſ)
and Ʒ as well as historical ligatures. Of course, the extracted plain text will also mirror early modern orthography unless you explicitly normalise it. No automatic spelling modernisation is applied.

### Spelling normalisation (optional)

If spelling normalisation is required after extraction, possible Python tools include:

- `regex` (advanced pattern-based normalisation)
- `unicodedata` (Unicode normalisation forms)
- `ftfy` (Unicode cleanup)
- Custom rule-based scripts (often necessary for early modern German)
- NLP libraries such as:
  - `spaCy` (custom pipelines)
  - `CLTK` (for historical languages; limited for early modern German)

For early modern German, normalisation is usually rule-based and project-specific rather than fully automated.

#### Model training (browser)

Of course, it is also possible to improve the OCR4all performance with model training based on corrected transcriptions. The training process and post-correction options are explained in detail at the bottom of the [workflow description in the OCR4all user guide](https://www.ocr4all.org/guide/user-guide/workflow). Training is computationally demanding and may take time on a laptop.

#### Summary of OCR4all functionalities

| Task | Terminal | Browser GUI |
|------|----------|-------------|
| Start container | ✓ | |
| Restart container | ✓ | |
| Create project | optional | ✓ (recommended) |
| Upload images | ✓ | |
| Preprocess images | | ✓ |
| Segmentation | | ✓ |
| Recognition | | ✓ |
| Correction | | ✓ |
| Export results | | ✓ |

## Suggested readings

Dennerlein, K., Rupnig, M., & Kastenhofer, N. (2024). Guidelines zur Volltexdigitalisierung von Dramen des 17 bis 19. Jahrhunderts mit OCR4all. [https://doi.org/10.5281/zenodo.12805233](https://doi.org/10.5281/zenodo.12805233)

Graf, K. (2023). OCR4all is better than Google. [https://doi.org/10.58079/CI04](https://doi.org/10.58079/CI04)

Langhanki, F., Wehner, M., Roeder, T., & Reul, C. (2023, June 30). Ocr4all — Open-source OCR and HTR across the centuries. [https://doi.org/10.5281/zenodo.8108008](https://doi.org/10.5281/zenodo.8108008)

Reul, C. (2021, November 12). Erschließung gedruckter und handschriftlicher historischer Quellen mit OCR4all [Billet]. Digital History Berlin. [https://doi.org/10.58079/nl2w](https://doi.org/10.58079/nl2w)

Schumacher, M. (2024). Toolbeitrag: OCR4all, forTEXT, Texdigitalisierung und Edition, 1(3). [https://doi.org/10.48694/fortext.3743](https://doi.org/10.48694/fortext.3743)

Winkler, A. (2021). OCR4all/calamari models for historical prints (Paris/Rome/Florence, 1582-1591). [https://doi.org/10.5281/zenodo.4608791](https://doi.org/10.5281/zenodo.4608791)

Please let us know if you come across other interesting papers covering OCR4all use cases that should be added here! Making an issue here on Github is the easiest way to suggest updates.
