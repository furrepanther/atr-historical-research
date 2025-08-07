# OCR4all - an open source OCR alternative

## What is OCR4all

[OCR4all](https://www.ocr4all.org/) offers "Optical Character Recognition (and more) for everyone". It is entirely free and open-source, running most smoothly on Linux machines and Linux-based servers.
Installations on Windows and Mac with Docker are also possible, but this may be a bit more challenging. The advantages of OCR4all are that it is flexibly applicable, fully transparent,
and integrates layout (with Larex) as well as options for text annotation.

## Creating and using an institutional instance of OCR4all

In 2024, Arnould Wils (FASoS Maastricht) set up an institutional instance of OCR4all for researchers at the Maastricht Faculty of Arts and Social Science. 
This instance runs on the university's Data Science Research Infrastructure (based on Kubernetes) and can be accessed by institution members via VPN.

## Creating a project folder for new document scans via the command line

New project folders with data to transcribe have to be created via the command line (local command line or terminal after [installing the OpenShift client](https://dsri.maastrichtuniversity.nl/docs/openshift-install/)),
which is certainly very challenging for users without previous Linux experience. But learning it is well worth the trouble.
The following guide explains how to upload image data into the **OCR4all instance hosted on DSRI (Maastricht University)**. For your own institutional OCR4all instance,
you have to adjust the steps accordingly.

### Step 1: make sure you have to right OCR4all access URL

First, you need to log in to DSRI and find the right ocr4all project in your dropdown menu. If the project has a running pod, you should be able to see the
route URL:

```https://ocr4all-p$$$-ocr4all-p$$$.apps.dsri2.unimaas.nl```

Here, ```p$$$``` stands for the staff number of the person who created the project, which had to be anonymised. When you click on the route URL itself, you will get an error notification, so make sure to add ```/ocr4all/``` in the end:

```https://ocr4all-p$$$-ocr4all-p$$$.apps.dsri2.unimaas.nl/ocr4all/```

### Step 2: optionally view OCR4all demo projects

You should now be able to see the user interface with options to select a project. Here you will see a dropdown with several pre-installed projects.
Projects like ```Geography``` are **demo projects** created by OCR4all and included for testing purposes only. You cannot use these projects to load
your own data. Rather, you must create your own project before processing any files. 
Since OCR4all does not have built-in user accounts or access control, every user should create their own project to avoid overwriting each other’s data.

### Step 3: Workflow for making a new OCR project

Prepare a folder of image files on your local machine (in TIFF, PNG, or JPG format). Then you can follow this process:

a) **Find your access token**:

To use OCR4all on DSRI for your own data, you need to generate an individual access token first. You can find your personal token when logging in to the DSRI OpenShift web portal (as described above) and navigating to this page:

[https://oauth-openshift.apps.dsri2.unimaas.nl/oauth/token/display](https://oauth-openshift.apps.dsri2.unimaas.nl/oauth/token/display)

Do not share this token with anyone and store it in a safe place!

b) **Login to OpenShift** (`oc login`):

Open the OCR4all pod on DSRI, use the web terminal, and log in using the following information from the OpenShift web portal:

```
oc login --token=<your-token> --server=https://api.dsri2.unimaas.nl:6443
```
You are now authenticated and can interact with the DSRI platform via the command line.

c) **Switch to the right project**

To see which projects you have access to on DSRI (or a similar instance), you can run:

```oc get projects```

To ensure that you are working within the right DSRI project for OCR4all, run the command below:

```oc project ocr4all-p$$$```

Again, ```$$$``` needs to be replaced with the staff number of the person who created the OCR4all set up at your institution.

d) **Access the running OCR4all pod**

Now, you need to make sure that you are working within the running pod of the OCR4all project. To list the current running pods in your project, use the following command:

```oc get pods```

Identify the running OCR4all pod (it usually has a name like ocr4all-xxxxx-xxxxx), then connect to it via:

```oc rsh <pod-name>```

e) **Create a new OCR project directory via terminal**

Once inside the running pod, navigate to the OCR4all data directory, where you will create a space to store your own image files:

```cd /var/ocr4all/data/```

Then create a new folder for your project and its input images by running the following commands:

```
mkdir MyProject
mkdir MyProject/input
```

Replace ```MyProject``` with the desired name of your OCR4all project. Avoid using spaces or special characters.

Now you can exit the pod with ```exit```.

f) **Upload your input images**

Now copy your input image files from your local machine into the newly created input directory in the OCR4all pod.

Use the following command:

```oc cp /path/to/your/images/ <pod-name>:/var/ocr4all/data/MyProject/input/```

First replace /path/to/your/images/ with the actual path to your local image folder. Then replace <pod-name> with the OCR4all pod name.
Ensure the files are named in logically and in the required order (e.g. source_page_001.tif, source_page_002.tif). Ideally, you should use image files (TIFF, PNG, JPG) and ensure that all files are directly inside ```/input/``` (not nested in subfolders). You can use PDFs file, but single images are preferred. It is not possible to upload compressed ZIP folders. If you accidentally uploaded the wrong files, re-run ```oc cp``` with updated files. You may need to manually delete unwanted images inside the pod using ```oc rsh```.

g) **Switch to the web interface for image processing**

Once your data are uploaded, you can switch to the OCR4all web interface for all following steps. Open the OCR4all interface in your browser (see information above) and check if your newly created project is now listed next to the sample projects.
Select your project and begin the actual OCR worfklow from image preprocessing to layout analysis and automated transcription. These steps are described further down below and also in the official [OCR4all user guide](https://www.ocr4all.org/guide/user-guide/introduction).

## Creating a new project folder via the OCR4all GUI

Depending on your instance, it may also be possible to create new OCR projects via the application's graphic user interface (GUI). In the case of the Maastricht University instance, you can open OCR4all in your browser via [https://ocr4all-p$$$-ocr4all-p$$$.apps.dsri2.unimaas.nl/ocr4all/]().
Then you should select ```Create Project```. Choose a unique and descriptive name for your project. A project name like ```test``` is much too vague,
and something like ```ocr_project_1``` does not allow you any immediate insight into what the project is about,
so you may get confused later. Good names are more telling, use lower-case letters only, and have no spaces, e.g.
```mob_letters_1705``` or ```field_notes_limburg_2015```.

Once you have chosen a name, OCR4all will automatically create a directory for your project with the following file path:

```/var/ocr4all/data/<your-project-name>/```

However, OCR4all does not provide drag-and-drop upload. All files must be transferred to the container using ```oc cp``` or by placing them in a network drive directly mounted to the pod. At Maastricht University, upload via terminal (as described in detail above) is currently the only option available.

## Getting an overview of your uploaded image files

The feature ["Overview"](https://www.ocr4all.org/guide/user-guide/project-start-and-overview) gives you "a tabular presentation of the project’s ongoing progress". Here, each row represents an individual page with a unique identifier. The columns document the workflow progress:

> Once a particular step has been executed, it will appear as completed (green check mark) in that work stage’s specific column. (...) With the button "Export GT" (top right) all data created in the course of the project can be exported and packed as a zip folder within 'data'.

## Pre-processing images

According to the OCR4all documentation, the best results are achieve when original images are converted into "straightened binarized or greyscale images". OCR4all also offers in-built noise removal, but it is, of course, possible to manipulate images before uploading them to OCR4all. Details on pre-processing operations can be found in the [workflow](https://www.ocr4all.org/guide/user-guide/workflow) section of the OCR4all user guide.

## Segmentation with LAREX

In automated text recognition, segmentation is necessary to add "structural information about the layout regions (type and position) as well as reading order".
In the OCR4all interface, you can perform semi-automated layout analysis with [LAREX](https://github.com/OCR4all/LAREX). LAREX is an open-source tool specifically developed for early printed books. According to the Github documentation, "it uses a rule based connected components approach which is very fast, easily comprehensible for the user and allows an intuitive manual correction if necessary."
If the automated layout recognition with LAREX fails, you can try and segment your scans manually. All these options can be selected in the collapsing menu on the left-hand side of your screen.

## Processing the images

In the web interface, you can finally move on to run a transcription model once your layout recognition is complete. Under the menu item **Recognition** in the right sidebar, you will find your files for which all pre-processing steps have been successfully completed. Here you can select those scans for which you want to create an automated transcript using one OCR model. Ideally, the selected image files should be similar in style, e.g. hand-written by the same person or printed by the same publisher. Go to **line recognition models** (under **available**) to find models or model packages. You will be able to choose from different historial and modern font types. The OCR4all developers "expressly advise the use of a model package, where five models simultaneously work and interact with each other! This is much preferable to using only one model at a time." Finding suitable models for your use case is made easier via a search function. The final results will be displayed under the menu item **ground truth production**.

## Model training

Of course, it is also possible to improve the OCR4all performance with model training based on corrected transcriptions. The training process and post-correction options are explained in detail at the bottom of the [workflow description in the OCR4all user guide](https://www.ocr4all.org/guide/user-guide/workflow).

## Technology support at Maastricht University

DSRI platform help: [dsri-support-l@maastrichtuniversity.nl](mailto:dsri-support-l@maastrichtuniversity.nl)
Local OCR4all coordination at Maastricht University: Dr. Arnoud Wils, Research Software Engineer at [The Plant](https://theplant.maastrichtuniversity.nl/team/)

## Suggested readings

Dennerlein, K., Rupnig, M., & Kastenhofer, N. (2024). Guidelines zur Volltexdigitalisierung von Dramen des 17 bis 19. Jahrhunderts mit OCR4all. [https://doi.org/10.5281/zenodo.12805233](https://doi.org/10.5281/zenodo.12805233)

Graf, K. (2023). OCR4all is better than Google. [https://doi.org/10.58079/CI04](https://doi.org/10.58079/CI04)

Langhanki, F., Wehner, M., Roeder, T., & Reul, C. (2023, June 30). Ocr4all — Open-source OCR and HTR across the centuries. [https://doi.org/10.5281/zenodo.8108008](https://doi.org/10.5281/zenodo.8108008)

Reul, C. (2021, November 12). Erschließung gedruckter und handschriftlicher historischer Quellen mit OCR4all [Billet]. Digital History Berlin. [https://doi.org/10.58079/nl2w](https://doi.org/10.58079/nl2w)

Schumacher, M. (2024). Toolbeitrag: OCR4all, forTEXT, Texdigitalisierung und Edition, 1(3). [https://doi.org/10.48694/fortext.3743](https://doi.org/10.48694/fortext.3743)

Winkler, A. (2021). OCR4all/calamari models for historical prints (Paris/Rome/Florence, 1582-1591). [https://doi.org/10.5281/zenodo.4608791](https://doi.org/10.5281/zenodo.4608791)

Please let us know if you come across other interesting papers covering OCR4all use cases that should be added here! Making an issue here on Github is the easiest way to suggest updates.
