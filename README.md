# ISA-ARC Workspace

Specification for ARC Workspaces with ISA and CWL flavours.

## Overview

The ISA-ARC Workspace facilitates iterative RDM by providing a folder structure and metadata file specifications for managing experimental and computational provenance.

Experimental metadata is organized in [ISA-XLSX] format, an implementation of the ISA abstract model 

Computational metadata is organized in a combination of workflows and runs defined by CWL (Common Workflow Language) and associated metadata files in [WR-XLSX](), an extension of the ISA abstract model for capturing computational provenance.

Specifications in this repo:
- [ISA ARC Workspace description (human readable overview)]()
- [ISA-ARC Workspace definition (machine readable specification of the folder structure)]()
- [ISA-XLSX specification]()
- [WR-XLSX specification]()

## Contributing to this repository

The default branch `release` of this repo is kept in sync with the latest release tag and the zenodo record. 

Therefore, all contributions to this repo must target the `dev` branch, which is representing the work in progress for the next versioned release.
