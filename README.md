# ISA-ARC Workspace

Specification for ARC Workspaces with ISA and CWL flavours.

## Overview

The ISA-ARC Workspace facilitates iterative RDM by providing a folder structure and metadata file specifications for managing experimental and computational provenance.

Experimental metadata is organized in [ISA-XLSX](ISA-XLSX.md) format, an implementation of the ISA abstract model.

Computational metadata is organized in a combination of workflows and runs defined by CWL (Common Workflow Language) and associated metadata files in [WR-XLSX](WR-XLSX.md), an extension of ISA-XLSX for capturing computational provenance.

The specifications are separate documents that can be referenced individually:

- [ISA-ARC Workspace specification, version 3.0.0-draft.2](ISA-ARC-Workspace.md): workspace structure and content.
- [ISA-XLSX specification, version 3.0.0-draft.1](ISA-XLSX.md): experimental metadata and shared XLSX conventions.
- [WR-XLSX specification, version 1.0.0-draft.1](WR-XLSX.md): workflow and run metadata extending ISA-XLSX.

The workspace version listed above references these ISA-XLSX and WR-XLSX versions. WR-XLSX 1.0.0-draft.1 extends ISA-XLSX 3.0.0-draft.1.

The registered document identifiers and publication procedure are described in [Publication and citation](PUBLICATION.md). These editions are drafts; separate identifiers do not imply independent conformance or remove dependencies between the specifications.

[workspace-definition.yml](workspace-definition.yml) is an empty placeholder for a planned machine-readable workspace definition; it is not an active specification.

## Contributing to this repository

The default branch `release` of this repo is kept in sync with the latest release tag and the zenodo record. 

Therefore, all contributions to this repo must target the `dev` branch, which is representing the work in progress for the next versioned release.
