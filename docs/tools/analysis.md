---
title: Open Analysis
parent: Open WIN Tools
has_children: false
nav_order: 3
---



# Open Analysis
{: .fs-9 }

How to share reproducible FSL analysis pipelines
{: .fs-6 .fw-300 }

---

![open-analysis](../img/img-open-anal-flow.png)

## Purpose

The Open Analysis Working Group has worked with researchers to capture details of the magnetic resonance imaging (MRI) tools that they use in data processing. WIN is the developmental home of the popular [FMRIB Software Library (FSL)](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/) MRI analysis package, which has been free to all users since its inception. FSL this is the tool of choice for most WIN researchers. The Working Group has devised a processes which researchers can slot into their normal FSL usage to ensure their analysis is reproducible and can be easily shared with others.

### Data standards for interoperable code
International community data standards - specifically the [Brain Imaging Data Structure (BIDS)](https://bids.neuroimaging.io) - have been employed to ensure that shared pipelines are compatible with tools developed elsewhere. This working group is also committed to actively engaging with the future development of BIDS, to ensure lifetime compatibility between FSL and the wide data standards.

### Education
Another large focus of this working group is developing programming literacy among WIN members, to support users in creating robust code to run their analysis. Significant efforts have been made to update the [FSL training material](https://fsl.fmrib.ox.ac.uk/fslcourse/), which is now available for free both internally and externally, and has been updated to include basic training in Unix command line access. The FSL course is being run remotely and at reduced registration rates for the first time in 2020, for improved accessibility and inclusivity.

Coming soon
{: .label .label-yellow }

**THIS TOOL IS CURRENTLY IN DEVELOPMENT. PLEASE REFER TO THE INFORMATION BELOW TO UNDERSTAND THE AIM AND AMBITION OF THIS PROJECT. THE "HOW TO" GUIDE WILL BE BUILT BY THE COMMUNITY AND TOOL DEVELOPERS IN THE COMING MONTHS.**\

<br><br>

[![For WIN members](../img/btn-win.png)](https://cassgvp.github.io/WIN-Open-Neuroimaging-Community/docs/tools/analysis.html#for-win-members)      [![For external researchers](../img/btn-external.png)](https://cassgvp.github.io/WIN-Open-Neuroimaging-Community/docs/tools/analysis.html#for-external-researchers)

## For WIN members
#### Version control ![version-control](../img/icon-version-control.png)
WIN members will be encouraged to develop their analysis pipelines into standalone scripts and store these on the [WIN GitLab instance](https://git.fmrib.ox.ac.uk). We will support our members in using git to version control their code, and employ best practice in ensuring their pipelines are robust and accurate.

#### Citable research output ![doi](../img/icon-doi.png)
Versions of analysis code can be assigned a digital object identified (DOI) using [Zenodo](https://zenodo.org) by uploading them from GitLab. Once a DOI has been created, your analysis code becomes a citable object which you can add to your list of research outputs.

#### Reproducible methods detail ![reproduce](../img/icon-reproduce.png)
Alongside your analysis code, WIN members will be supported in implementing a "[wrapper](https://techterms.com/definition/wrapper)" script which can:
1. access data stored on the WIN [Open Data](data.md) servers;
2. access your GitLab code repository;
3. pull a stable version of the FSL analysis package in a [Singularity container](https://en.wikipedia.org/wiki/Singularity_(software));
4. Run the accessed data using the supplied code and the given version of FSL via the container on a high performance cluster.

The benefit of the above comes from version control of the singularity container and that it encompasses a complete computational environment, such that it can be run on any operating system without concern over dependencies of versions of packages, making the analysis highly reproducible.

## For external researchers
External users will be able to access the shared code and singularity containers, along with data when this is shared openly, and repeat the analysis to probe the results. External users will also be able to modify shared analysis code to suit there own needs, where this is shared with a permissive license.

## How to use
### Access
Coming soon
{: .label .label-yellow }

Detailed guidance on how to use the Open Analysis tools will be produced during one of our [documentation hacks](../events/doc-hack-1.md)

## Working group members (alphabetically)
We are grateful to the following WIN members for their contributions to developing the Open Analysis tools
- [Taylor Hanayik](https://www.win.ox.ac.uk/people/taylor-hanayik)
- [Mark Jenkinson](https://www.win.ox.ac.uk/people/mark-jenkinson)
- Paul McCarthy
- [Andrew Quinn](https://www.win.ox.ac.uk/people/andrew-quinn)
- [Matthew Webster](https://www.win.ox.ac.uk/people/matthew-webster)
