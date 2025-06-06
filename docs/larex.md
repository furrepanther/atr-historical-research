# Using LAREX for manual segmentation & line detection in OCR4all (DSRI)

## About LAREX and when to use it

LAREX is the integrated tool in OCR4all for **layout analysis and segmentation**. While a standalone version of LAREX exists,
it is recommended to use the built-in one to avoid file compatibility issues. LAREX should be used when automated layout recognition in OCR4all fails.
It is especially helpful when layouts are challenging because of several columns, marginalia or table formats.

## How to use LAREX on the Maastricht instance of OCR4all

Before using LAREX, you have to make sure that your OCR4all pod is running and that you have selected a valid project with image file.
You also have to complete the pre-processing including image conversion and binarization first. Then you can open LAREX (**Layout Analysis**) via the collabsible 
menu and adjust or create regions manually. Click the **“LAREX Editor”** button next to a page thumbnail. The LAREX editor will open in a new tab.

In the LAREX interface, you can:

- Use the **selection tool** to adjust or delete wrongly detected blocks.
- Use the **rectangle or polygon tools** to draw new text regions (e.g., main text, marginalia).
- Assign **region types** (e.g. Text, Image, Table) from the right-hand sidebar.

Each region must be labeled to be used in OCR processing later. To detect or adjust lines in your text, first select a text region, then click **“Detect Lines”** in the right sidebar.
and use the sliders to adjust detection sensitivity. If automatic line detection fails, use the **“Add Line” tool** manually. Define the line base and bounding box. Use **Shift + Click** to define the reading order if necessary.
Once you're done, click **“Save Segmentation”** and **“Close”** to return to the OCR4all layout view. You will now see the updated segmentation in OCR4all and can proceed with OCR.

### Batch or copy segmentation

LAREX supports **batch operations** (apply settings to multiple pages) and **copy segmentation** from one page to another if layouts are identical. Access these under **“Tools” > Batch Processing** or via the top toolbar.
Make sure to save all changes before returning to OCR4all.

### Additional advice

Zoom in when drawing lines to avoid incorrect alignment or overlap. Use consistent region types to guarantee correct OCR and export behavior.
Start with middle pages as automatic detection often works better there. Adjust binarization first, which affects accuracy of segmentations.
To undo and redo, use **Ctrl+Z** (undo) and **Ctrl+Y** (redo).

## Further information

- OCR4all Docs: [https://github.com/OCR4all](https://github.com/OCR4all)
- LAREX GitHub: [https://github.com/LAREX-toolkit/LAREX](https://github.com/LAREX-toolkit/LAREX)
