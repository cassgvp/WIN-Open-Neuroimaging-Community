# Docker

XNAT provides a [Container System](https://wiki.xnat.org/container-service/introduction-32538756.html that offers (primarily)) Docker image support. Initial image installation and setup is carried out by an admin, but using containers and tailoring them to individual projects is done by users (with sufficient privileges). Where containers are run depends on the specific image and use case, but can range from individual scans to project wide.

---

## Current Images

We are currently working on developing and supporting the following Docker images, however please be aware they are all still in various levels of testing states.

* DICOM to NIFTI + BIDS - for converting DICOM to NIFTI with BIDS json files within XNAT (this approach is required for the BIDS MRIQC image to run)
* BIDS MRIQC - an example of how BIDS apps can be run within XNAT
* FSL - this currently requires extra setup for each individual tool, so only the following have so far been implemented:
  * fsl-bet
  * fsl-deface
  * fsl-reorient2std
  * fsl-robustfov
* Snapshot Generator - used to generate snapshot gifs for existing projects

---

## Example Usage

### Enabling Containers for a project

You will first need to enable a container for a project (provided you have sufficient privileges). To do so you will need to head to your project within XNAT, and then to Project Settings (found on the Actions panel on the right). Here you will see a list of available containers - enable the container(s) you want available within your project.

### BIDS Containers (DICOM to NIFTI + BIDS, BIDS MRIQC)

For examples on how to use BIDS related containers see the [BIDS](bids.md) page.

### FSL Containers

The fsl containers can be run in two ways: individually or in bulk. To do either you will need to be on a subjects experiment page, where the scans are listed. To individually run a tool you will see the "Run" column on the right of a scan, hover over this to select the specific fsl tool you want to run. To run in bulk, select the scans you want to use with the check boxes on the left of the scans, and then choose the fsl tool in "Run Container" found where it says "Bulk Actions".

### Snapshot Generator

This is an example of a project level container. When on a project home page you will see "Run Containers" at the bottom of the Actions menu - click on this to select the snapshot-generator container. When run it will go through all of the scans within the project and either attempt to create a snapshot for a NIFTI file or run the XNAT pipeline to create snapshots for DICOM files.

### Setting up bids-mapping for the dicom2bids container

Due to the XNAT documentation now being deleted, this guide is based on an XNAT discussion group [post](https://groups.google.com/g/xnat_discussion/c/stxargOhvO4/m/VKoggWWtBwAJ) laying out the requirements. As explained in the post: "An XNAT pipeline that uses dcm2niix to generate the NIFTI and sidecar, labels them according to a user-defined mapping between series description and BIDS type (this is the BIDS map mentioned), and uploads the results to two directories at the scan level (NIFTI for the images and BIDS for the BIDS sidecar)."

The post does not offer an example command for uploading a project-wide bids-mapping, but it does offer the side-wide approach:

``curl -X PUT -i -k -u username https://xnat.url.here/REST/config/bids/bidsmap?inbody=true -d @conversion-sample.json``

This is the contents of the conversion-sample.json:

```json
[
	{ "series_description": "AX T1", "bidsname": "T1w" },
	{ "series_description": "AX T2", "bidsname": "T2w" },
	{ "series_description": "BOLD", "bidsname": "task-rest_bold" }
	{ "series_description": "FLAIR", "bidsname": "FLAIR" },
	{ "series_description": "FLASH", "bidsname": "FLASH" },
	{ "series_description": "MPRAGE", "bidsname": "T1w" },
	{ "series_description": "DTI", "bidsname": "dwi" }
]
```

Without a bids-mapping file on the XNAT instance, the container will fail to convert any scans. To see the currently active site-wide bids-mapping:

https://xnat-url/REST/config/bids/bidsmap
