# Case study: opdc

The [OPDC](https://www.opdc.ox.ac.uk/home) project used the DPUK XNAT Node so that it could share imaging date the [DPUK data portal](https://info.dpuk.org).

## XNAT Upload instructions using .csv file

To upload (Assuming you have already an xnat-project).

1. Organise the DICOMs you want to upload into a structure like:
mymaindirectory/
* DICOMdir1
* DICOMdir2
* DICOMdirN

Where DICOMdir contains the DICOM data for a specific session for a certain subject (subject_ID)

2. Create a .csv file (e.g. myproject.csv) with 3 columns:

|subject_ID   | DICOMdir   | scan_date     |
|-------------|------------|---------------|

* subject_ID is uniquely associated with the participant
* directory is the name of the directory containing the DICOM files to upload. If more sessions of a certain subject exist, they have the code of the session (e.g. Calpendo ID)

3. Run the [csvupload.sh](https://git.fmrib.ox.ac.uk/-/snippets/32) script:

E.g. usage
``./csvupload.sh myproject.csv <xnat-username> <xnat-project_id> mymaindirectory > output_log.txt``

This script calls [uploaddcmtk.sh](https://git.fmrib.ox.ac.uk/-/snippets/31) so make sure you have both.
