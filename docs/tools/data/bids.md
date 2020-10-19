# BIDS in XNAT

This page attempts to explain the current landscape of the [Brain Imaging Data Structure](https://bids.neuroimaging.io) (BIDS) in [XNAT](https://www.xnat.org/). There have been multiple BIDS related topics in the [XNAT Discussion Group](https://groups.google.com/forum/#!forum/xnat_discussion), but most show users have created their own bespoke process and tools as they are often specific to the dataset(s) in question.

---

## Importing/Uploading to XNAT

There is a BIDS Import tool developed by the XNAT team to support importing of a valid BIDS dataset into an XNAT project - details of which can be found [here](https://bitbucket.org/mohanar_radiologics/xnat_bids_importer/src/master/). This is limited in scope to MRI imaging, and does not appear to be actively developed/supported. We have contacted the developer of this tool in the past in the hopes of increasing its support to other data types such as MEG, but due to time and budgetary reasons this is unlikely to happen.

It seems unlikely there will be much demand for importing a BIDS compliant dataset into XNAT, but it is possible to create a script to do so (we have developed similar scripts in the past) so this may be the best approach if and when this is required. For an example script see [here](https://issues.dpuk.org/dpuk/node/snippets/10).

---

## Exporting/Downloading from XNAT

There does not appear to be any form of official BIDS downloading tool from XNAT, or even a recommended third party solution. This has again seen the discussion board offer various user approaches to the issue, many of which do not easily translate to other projects due to their tailor made approach to a particular dataset. We have developed a python script to download and format a project from XNAT into a BIDS compliant dataset, but this approach requires the script to be modified on a project to project basis.

Perhaps the most promising tool for this functionality is the Distributed Automation for XNAT (DAX) - details of this can be seen in the Example Workflows section below.

---

## Creating/Generating in XNAT

Through the use of the [XNAT Container System](https://wiki.xnat.org/container-service/) it is possible to create a BIDS dataset from a DICOM source within XNAT. The XNAT-developed [BIDS container](https://github.com/NrgXnat/docker-images/tree/master/dcm2bids-session) converts from DICOM to NIFTI with BIDS metadata, using the [dcm2niix](https://github.com/rordenlab/dcm2niix) tool.

This tool is currently limited to an older version of the dcm2niix tool from 2018, and only runs at a session level in XNAT (meaning sessions have to be converted individually) as official XNAT containers do not seem to be actively developed.

---

## BIDS Structure in XNAT

The BIDS file structure has certain requirements to be considered valid, and these are not compatible with the way XNAT structures its data. This means importing, exporting, and running containers need to take this into account in order to execute successfully.

---

## BIDS Apps in XNAT

There are a wide range of available BIDS App container images designed to work so long as they have a valid BIDS dataset as input, details can be found [here](https://bids-apps.neuroimaging.io/). The issue of the XNAT and BIDS format being incompatible has been addressed in the form of a Setup Command for the Container System (details of the issue and solution found [here](https://wiki.xnat.org/container-service/setup-commands-53674443.html). With that resolved, the BIDS MRIQC App was incorporated into XNAT as a proof of concept to show a BIDS App working. There are issues related to running a BIDS App across a group of subjects, summarised on [this page](https://wiki.xnat.org/container-service/list-of-containers-and-commands-36372911.html) (in the Proposed section) with a more detailed technical explanation found [here](https://wiki.xnat.org/container-service/bulk-launching-containers-35029445.html).

---

## Example Workflows

### Converting a DICOM Session to NIFTI + BIDS via Container

There are a few setup steps required in order to be able to run the container (this guide assumes it is installed and working on the XNAT instance).

*Setup:*

1. The dcm2bids-session container needs to be enabled for the project. To do so you will need to head to your project within XNAT, and then to Project Settings (found on the Actions panel on the right). Here you will see a list of available containers - enable the container "xnat/dcm2bids-session:latest".
2. A valid BIDS map of the scans in the project needs to be uploaded to XNAT via the REST API. The XNAT Wiki page explaining the process has been removed, but a reply in a topic on the discussion group explains the process [here](https://groups.google.com/g/xnat_discussion/c/qPXd_L2r-6Y/m/SC5Jxzj9AgAJ).

*Use:*

1. When on a session page (where the list of scans is shown) you will now see a "Run Containers" option on the Actions panel, clicking on the "Run dcm2niix-session on a Session" option will launch a window where you can launch the container.
2. After running, you will be able to see via the "Manage Files" Actions panel entry the newly created NIFTI and BIDS resource folders containing the output files, the original DICOM will remain untouched.

---

### Running a BIDS-App

This requires NIFTI and BIDS resource folders and files to run, as generated from the dcm2bids-session container (see above).

*Setup:*

1. Enable the container at the project level. To do so you will need to head to your project within XNAT, and then to Project Settings (found on the Actions panel on the right). Here you will see a list of available containers - enable the container "poldracklab/mriqc" (the version number following this may change).

*Use:*

1. When on a session page (where the list of scans is shown) you will now see a "Run Containers" option on the Actions panel, clicking on the "Run the MRIQC BIDS App with a session mounted" option will launch a window where you can launch the container.
2. When a container is actively running, refreshing the session page will show an "Active Processes" list at the top of the page, showing the active container names.
3. After running, you will be able to see via the "Manage Files" Actions panel entry the newly created MRIQC resource folder containing the output files, the original files will remain untouched.

---

### Downloading a valid BIDS Dataset

In order to download a valid BIDS dataset a third-party solution is required as there is no official method to do so, with the [Distributed Automation for XNAT](https://github.com/VUIIS/dax (DAX)) looking to be the most feature complete.

*Setup:*

1. The first step is to install DAX - details on how to do this can be found [here](https://dax.readthedocs.io/en/latest/).
2. The next step is to create and upload a BIDS datatype map for the project you intend to download and convert into a BIDS valid dataset, a walk through tutorial can be found [here](https://dax.readthedocs.io/en/latest/BIDS_walkthrough.html) on how to do this.

*Use:*

1. You will then be able to download a dataset and convert it into BIDS via a DAX Command Line Tool - [XnatDownload](https://dax.readthedocs.io/en/latest/dax_command_line_tools.html#xnatdownload). See below for DAX's example command to do so:

```
# Transform the XnatDownload data in BIDS format for all sessions, scantype and resources:
Xnatdownload -p PID --sess all -d /tmp/downloadPID -s all --rs all --bids --bids_dir /tmp/BIDS_dataset
```
