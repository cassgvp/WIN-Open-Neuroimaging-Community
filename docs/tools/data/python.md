# Python libraries

There are several python libraries that can be used to interrogate the XNAT API.  This page has notes on the libraries that have been used in WIN.

## PyXNAT

https://pyxnat.github.io/pyxnat/ is the original python library.  It's API is a little long winded but does allow you to create objects in XNAT.  

TODO: examples, gotchyas, version woes

## XNATPy

https://xnat.readthedocs.io/en/latest/static/tutorial.html is a newer python library.  It's api is more concise and easier to use than pyXNAT by is readonly.

## DAX

https://dax.readthedocs.io/en/latest/index.html is a wrapper for pyxnat that knows about clusers, pipelines and BIDS.

---

# Examples

## Example PyXNAT usage:

For an example use case see the following page (under the "Using Python" section): https://info.dpuk.org/2018/01/10/uploading-niftis/

## Example XNATPy usage:

```
# Connecting to an XNAT instance
>>> import xnat
>>> session = xnat.connect('xnat_website_address', user='xnat_username')
# Creating a subject (the project will need to be created via the XNAT UI if it doesn't already exist)
>>> project = session.projects['projectID']
>>> subject = session.classes.SubjectData(parent=project, label='new_subject_label')
# Creating an MR experiment
>>> experiment = session.classes.MrSessionData(parent=subject, label='new_scan_label')
 # Creating an MR scan
>>> scan = session.classes.MrScanData(parent=experiment, id='unique_scan_ID', type='new_scan_type')
 # Creating a resource folder
>>> resource = session.classes.ResourceCatalog(parent=scan, label='NIFTI')
# Uploading a NIFTI file
>>> resource.upload('file_path', 'uploaded_filename')
```

Where you see a label or type input, this can be changed to something more appropriate to your data (except the NIFTI label for the resource folder). The scan id will need to be unique within the experiment, XNAT by default will count from 1 for this but it shouldn't be required so long as it is unique. The parent attribute requires a particular XNAT type depending on what is being created, but it follows the XNAT hierarchy (project > subject > experiment > scan > resource).

## PyXNAT vs XNATPy subject creation:

### PyXNAT:

```python
from pyxnat import Interface

url = ""
user = ""
project_id = ""

# establish pyxnat session and connect to project
project = Interface(server=url,user=user).select.project(project_id)

# Create a new subject
subject = project.subject('TestSubject')
subject.create()
```

### XNATPy:

```python
import xnat

url = ""
user = ""
project_id = ""

# Connect to an XNAT instance
session = xnat.connect(url, user=user)

# Create a subject
project = session.projects[project_id]
subject = session.classes.SubjectData(parent=project, label='TestSubject')
```
