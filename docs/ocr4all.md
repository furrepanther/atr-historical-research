# OCR4all - an open source OCR alternative

## What is OCR4all

[OCR4all](https://www.ocr4all.org/) offers "Optical Character Recognition (and more) for everyone". It is entirely free and open-source, running most smoothly on Linux machines and Linux-based servers.
Installations on Windows and Mac with Docker are also possible, but this may be a bit more challenging. The advantages of OCR4all are that it is flexibly applicable, fully transparent,
and integrates layout (with Larex) as well as options for text annotation.

## Creating and using an institutional instance of OCR4all

In 2024, Arnould Wils (FASoS Maastricht) set up an institutional instance of OCR4all for researchers at the Maastricht Faculty of Arts and Social Science. 
This instance runs on the university's Data Science Research Infrastructure (based on Kubernetes) and can be accessed by institution members via VPN.

## Creating a project folder for new document scans from scratch

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

## Step 2: optionally view OCR4all demo projects

You should now be able to see the user interface with options to select a project. Here you will see a dropdown with several pre-installed projects.
Projects like ```Geography``` are **demo projects** created by OCR4all and included for testing purposes only. You cannot use these projects to load
your own data. Rather, you must create your own project before processing any files. 
Since OCR4all does not have built-in user accounts or access control, every user should create their own project to avoid overwriting each other’s data.

## Step 3: Workflow for making a new OCR project

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

2. **Switch to the right project**

To ensure that you are working within the right DSRI project, run the command below:

```oc project ocr4all-p$$$```

3. **Create a new OCR project**

Open OCR4all in your browser and go to ```https://ocr4all-p$$$-ocr4all-p$$$.apps.dsri2.unimaas.nl/ocr4all/```.
Then you should select ```Create Project```. Choose a unique and descriptive name for your project. A project name like ```test``` is much too vague,
and something like ```ocr_project_1``` does not allow you any immediate insight into what the project is about,
so you may get confused later. Good names are more telling, use lower-case letters only, and have no spaces, e.g.
```mob_letters_1705``` or ```field_notes_limburg_2015```.

Once you have chosen a name, OCR4all will automatically create a directory for your project:

```/var/ocr4all/data/<your-project-name>/```

5. **Copy image files into your project folder in the running pod**

OCR4all does not provide drag-and-drop upload. All files must be transferred to the container using oc cp or by placing them in a volume mounted to the pod.
In your terminal, run ```oc get pods``` and look for a pod name starting with ```ocr4all``` like ```ocr4all-p$$$-xxxxx-yyyyy```. Copy the full pod name.

If your images are stored locally at ```~/ocr_input/```, run:

```oc cp ~/ocr_input/. <pod-name>:/var/ocr4all/data/<your-project-name>/input/```

Replace <pod-name> and <your-project-name> with your actual values. Make sure that you copy only image files (TIFF, PNG, JPG) and that all files are directly inside ```/input/``` (not nested in subfolders). You cannot use PDFs or compressed ZIP folders.
If you accidentally uploaded the wrong files, re-run ```oc cp``` with updated files. You may need to manually delete unwanted images inside the pod using ```oc rsh```.

7. **Segmentation with LAREX**

Return to the OCR4all interface and select your newly created project. You should now see your uploaded image files listed. You can now proceed with the
automated layout analysis. If this fails, try and segment your scan manually with [LAREX](/larex.md). All these options can be selected in the collapsing menu on the left-hand side of your screen.

8. **Process the images**

In the web interface, you can then move on to run a transcription model. [MORE DETAILS TO FOLLOW] 

### Contacts

DSRI platform help: 
Local OCR4all coordination:

### Related links
