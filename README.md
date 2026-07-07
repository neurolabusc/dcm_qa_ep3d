## About

This is a simple DICOM to NIfTI validator script and dataset based on Siemens XA60 enhanced DICOM images. This repository illustrates how to extract parameters from the popular DZNE Bonn 3D-EPI research sequences.

## Details

For BIDS specification sidecars the `RepetitionTime` key value pair refers to the time between 3D volumes in a time series (the volume TR). These DZNE 3D-EPI sequences report the *per-shot* TR in DICOM tag 0018,0080, so dcm2niix must recover the volume TR. It does this from the inter-volume `AcquisitionDateTime` spacing (primary), falling back to `shot TR × SlicesPerSlab / AccelerationFactor3D × MultiEchoShots`. dcm2niix emits the per-shot TR as `RepetitionTimeExcitation`, and `MultiEchoShots` (> 1 only) for the multi-shot case.

* XA60 Common Parameters 
  * Manufacturer: Siemens
  * Model: 3T Vida

| Series | Sequence name                          | Echoes | Multi-echo shots | Per-shot TR (ms) | Volume TR (ms) |
| ------ | -------------------------------------- | ------ | ---------------- | ---------------- | -------------- |
| 1      | anat-scout (localizer, derived — skipped) |     |                  |                  |                |
| 5, 6   | func-bold_acq-3d (mag, phase)          | 1      | 1                | 46.1             | 1014.2         |
| 7, 8   | func-bold_acq-3d3echo (mag, phase)     | 2      | 1                | 56.2             | 1236.4         |
| 9, 10  | func-bold_acq-3d2shots (mag, phase)    | 2      | 2                | 46.1             | 2028.4         |

Note series 9/10: with 2 multi-echo shots the volume TR is **twice** the single-shot value (2 × 1014.2 = 2028.4 ms). The scanner console and PDF report the single-shot VolTR (1014.2 ms); the true volume-to-volume time is 2028.4 ms. dcm2niix reads the shot count from the CSA `sSliceArray.lConc` (concatenations) field.

## Running

Assuming that the executable dcm2niix is in your path, you should be able to simply run the script `batch.sh` from the terminal.

## Links

These images were acquired with the `dzne_ep3d` sequences, version 2025.09.12 (DZNE Bonn 3D-EPI) maintained by Rüdiger Stirnberg.

- Le Ster et al. (2019) Comparison of SMS-EPI and 3D-EPI at 7T in an fMRI localizer study with matched spatiotemporal resolution and homogenized excitation profiles. PLoS One. [PMID: 31751410](https://pubmed.ncbi.nlm.nih.gov/31751410/)
