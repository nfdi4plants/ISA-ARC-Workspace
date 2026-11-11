# WR-XLSX format, version 1.0.0-draft.1

For detail on ISA framework terminology, please read the [ISA Abstract Model specification](https://isa-specs.readthedocs.io/en/latest/isamodel.html).

This document describes the workflow and run extensions of the ISA Abstract Model reference implementation specified in the ISA-XLSX format. The XLSX format uses the SpreadsheetML markup language and schema to represent a spreadsheet document. Conceptually, using the terminology of the Spreadsheet ML specification [ISO/IEC 29500-1](https://www.loc.gov/preservation/digital/formats/fdd/fdd000398.shtml#:~:text=The%20XLSX%20format%20uses%20the,a%20rectangular%20grid%20of%20cells.), the document comprises one or more worksheets in a workbook.

This specification is an extension of [ISA-XLSX](./ISA-XLSX.md) and reuses its concepts, structures, terminology, and conventions for representing metadata in XLSX format.

**Table of contents**

- [Workflow File](#workflow-file)
- [Run File](#run-file)
- [Top-level metadata sheets](#top-level-metadata-sheets)
  - [WORKFLOW section](#workflow-section)
  - [RUN section](#run-section)
  - [Multiple Values](#multiple-values)
- [Annotation Table sheets](#annotation-table-sheets)
  - [Examples](#examples)
- [Datamap Table sheets](#datamap-table-sheets)

WR-XLSX extends the ISA-XLSX format by introducing additional files and sheets to capture computational workflows and runs. Specifically, it defines:
  - Run file
  - Workflow file

The Investigation file contains all the information needed to understand the overall goals and means used in an experiment; experimental steps (or sequences of events) are described in the Study and in the Assay file(s). For each Investigation file there may be one or more Studies defined with a corresponding Study file; for each Study there may be one or more Assays defined with corresponding Assay files; one assay file may be registered in different studies.

In order to facilitate identification of ISA-XLSX component files, specific naming patterns MUST be followed:

- `isa.workflow.xlsx` for identifying the [Workflow file](#workflow-file)
- `isa.run.xlsx` for identifying the [Run file](#run-file)

Sheets described in this specification MUST follow one of the two given formats:

- [`Top-level metadata sheets`](#top-level-metadata-sheets) for listing top-level metadata
- [`Annotation Table sheets`](#annotation-table-sheets) for describing experimental workflows

Sheets which do not follow any of these two formats are considered additional payload and are ignored in this specification.

All labels are case-sensitive:

Dates SHOULD be supplied in the [ISO8601](http://www.iso.org/iso/home/standards/iso8601.htm) format.

For maximal portability file names SHOULD contain only ASCII characters not excluded
already (that is `A-Za-z0-9._!#$%&+,;=@^(){}'[]` - we exclude space as many utilities
do not accept spaces in file paths): non-English alphabetic characters cannot be guaranteed
to be supported in all locales. It is recommended to avoid the shell metacharacters
`(){}'[]$."`.

# Workflow File

The `Workflow` represents the prospective orchestration of other tools or workflows. A `Workflow File` contains contextualizing information about the workflow.

The `Workflow File` MUST contain one [`Top-Level Metadata sheet`](#top-level-metadata-sheets). This sheet MUST be named `isa_workflow` and MUST contain the following sections:

- [`WORKFLOW`](#workflow-section)
- [`WORKFLOW CONTACTS`](#workflow-contacts)

# Run File

The `Run` represents the application of a `workflow`, i.e. the execution of a computational tool that has orchestrated the execution of other tools. A `Run File` contains metadata about the executed workflows, information about the people creating, picking and parametrizing these workflow exections, and most importantly, details about the input data, parametrization and the result data.

The `Run File` MUST contain one [`Top-Level Metadata sheet`](#top-level-metadata-sheets). This sheet MUST be named `isa_run` and MUST contain the following sections:

- [`RUN`](#run-section)
- [`RUN PERFORMERS`](#run-performers)

Additionally, the `Run File` SHOULD contain one or more [`Annotation Table sheet(s)`](#annotation-table-sheets), which MAY record provenance of data through application of computational workflows and the parameters that describe these computations.

Therefore, the main entities of the `Run File` should be `Data`.


# Top-level metadata sheets

The purpose of top-level metadata sheets is aggregating and listing top-level metadata. Each sheet consists of sections consisting of a section header and key-value fields. Section headers MUST be completely written in upper case (e.g. STUDY), field headers MUST have the first letter of each word in upper case (e.g. Study Identifier); with the exception of the referencing label (REF).

In the following sections, examples of each section block are given beside the specification of each section.

> ### ATTENTION
> Rows in which the first character in the first column is Unicode
> [U+0023](http://www.fileformat.info/info/unicode/char/0023/index.htm)  (the `#` character) > MUST be interpreted as
> comments, where reference implementation parsers SHOULD ignore those lines entirely.

> Rows where the label `Comment[<comment name>]` appear can also appear within any of the > section blocks. Where these appear, the comment name must be unique within the context of a single block (e.g. you cannot have multiple occurrences of `Comment[external DB REF]` within `STUDY ASSAYS`. Also, the value cells MUST match the number of values indicated by the rest of the section in context.

## WORKFLOW section

This section is organized in several subsections, described in detail below. The subsections in the block are arranged vertically; the intent being to enhance readability and presentation, and possibly to help with parsing. These subsections MUST remain within
this block; the fields MUST remain within their subsection.

These sections implement the metadata for a `Workflow`.

### WORKFLOW

This section MUST contain zero or one values.

This section MUST contain the following labels, with the specified datatypes for values supported:


| Label | Datatype | Description |
|-------|----------|-------------|
| Workflow Identifier          | String | A mandatory unique identifier, either a temporary identifier supplied by users or one generated by a repository or other database. For example, it could be an identifier complying with the LSID specification. A value MUST be given for this label. |
| Workflow Title               | String                               | A concise phrase used to encapsulate the purpose and goal of the workflow.  |
| Workflow Description         | String | A textual description of the workflow, with components such as objective or goals. |
| Workflow Subworkflow Identifiers | String | Identifiers of the Workflows who're orchestrated in the context of this workflow. Multiple values can be provided by using a semicolon (“;”) Unicode (U0003+B) as a separator (e.g.: Imputation;Testing;Plotting). |
| Workflow Type                                  | String     | Term to classify the workflow. The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. |
| Workflow Type Term Accession Number            | String     | The accession number from the Term Source associated with the selected term. |
| Workflow Type Term Source REF                  | String     | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match one of the Term Source Name declared in the Ontology Source Reference section. |
| Workflow URI                                   | String     | Pointer to workflow resources external to the ISA-Tab that can be accessed by their Uniform Resource Identifier (URI). |
| Workflow Version                               | String     | An identifier for the version to ensure workflow tracking. |
| Workflow Parameters Name                       | String     | A semicolon-delimited (“;”) list of parameter names, used as an identifier within the ISA-XLSX document. These names are used in [Run Annotation Tables](#annotation-table-sheets) to list the values used for each Workflow parameter. Refer to section [Multiple values](#multiple-values) on how to encode multiple values in one field and match term sources. |
| Workflow Parameters Term Accession Number      | String     | The accession number from the Term Source associated with the selected term. |
| Workflow Parameters Term Source REF            | String     | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match one of the Term Source Name declared in the Ontology Source Reference section. |
| Workflow Components Name                       | String     | A semicolon-delimited (“;”) list of a workflow’s components; e.g. software names. Refer to section [Multiple values](#multiple-values) on how to encode multiple values in one field and match term sources. |
| Workflow Components Type                       | String     | Term to classify the workflow components listed, e.g command line tool or software. The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. |
| Workflow Components Type Term Accession Number | String     | The accession number from the Source associated to the selected terms. |
| Workflow Components Type Term Source REF       | String     | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match a Term Source Name previously declared in the ontology section |
| Workflow File Name           | String     | A field to specify the name of the Workflow Table file corresponding the definition of that workflow. There can be only one file per cell. |



**Example**

For example, the `WORKFLOW` section of an ISA-XLSX `isa.workflow.xlsx` file may look as follows:

|      |      |
|------|------|
| WORKFLOW |
| Workflow Identifier     | ProteomIQonPipeline | 
| Workflow Title     | Proteome Quantification | 
| Workflow Description     | Calculate protein abundancies using ProteomIQon. | 
| Workflow Subworkflow Identifiers | PeptideSpectrumMatching;ProteinInference;ProteinQuantification | 
| Workflow Type           | Proteomic Quantification Method | 
| Workflow Type Term Accession Number | http://purl.obolibrary.org/obo/NCIT_C161810 |  
| Workflow Type Term Source REF | NCIT |  
| Workflow URI            | | 
| Workflow Version        | |
| Workflow Parameters Name | Protein quantification  | 
| Workflow Parameters Name Term Accession Number | http://edamontology.org/operation_3630 | 
| Workflow Parameters Name Term Source REF | EDAM | 
| Workflow Components Name | Proteomiqon | 
| Workflow Components Type | software | 
| Workflow Components Type Term Accession Number | http://purl.obolibrary.org/obo/IAO_0000010 | 
| Workflow Components Type Term Source REF | IAO | 
| Workflow File Name     | workflows/ProteomIQon/isa.workflow.xlsx | 

### WORKFLOW CONTACTS

This section MUST contain zero or more values.

This section MUST contain the following labels, with the specified datatypes for values supported:

| Label | Datatype | Description |
|-------|----------|-------------|
| Workflow Person Last Name | String  | The last name of a person associated with the workflow.  |
| Workflow Person First Name  | String | Workflow Person Name |
| Workflow Person Mid Initials  | String  | The middle initials of a person associated with the workflow.|
| Workflow Person Email | String formatted as email | The email address of a person associated with the workflow. |
| Workflow Person Phone  | String | The telephone number of a person associated with the workflow. |
| Workflow Person Fax  | String | The fax number of a person associated with the workflow.  |
| Workflow Person Address  | String | The address of a person associated with the workflow. |
| Workflow Person Affiliation | String | The organization affiliation for a person associated with the workflow. |
| Workflow Person Roles | String or Ontology Annotation if accompanied by Term Accession Numbers and Term Source REFs | Term to classify the role(s) performed by this person in the context of the workflow, which means that the roles reported here need not correspond to roles held withing their affiliated organization. Multiple annotations or values attached to one person can be provided by using a semicolon (“;”) Unicode (U0003+B) as a separator (e.g.: submitter;funder;sponsor) .The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. Refer to section [Multiple values](#multiple-values) on how to encode multiple values in one field and match term sources. |
| Workflow Person Roles Term Accession Number | String | The accession number from the Term Source associated with the selected term. |
| Workflow Person Roles Term Source REF | String | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match one of the Term Source Names declared in the Ontology Source Reference section. |

**Example**

For example, the `WORKFLOW CONTACTS` section of an ISA-XLSX `isa.workflow.xlsx` file may look as follows:

|                |         |       |
|----------------|---------|-------|
| WORKFLOW CONTACTS |         |       |
| Workflow Person Last Name     | Ott | Zimmer  |
| Workflow Person First Name    | Caroline    | David   |
| Workflow Person Mid Initials  |       |       |
| Workflow Person Email         |  |       |
| Workflow Person Phone         |         |       |
| Workflow Person Fax           |         |       |
| Workflow Person Address       | Erwin-Schrödinger-Straße 56, 67663 Kaiserslautern, Germany | Erwin-Schrödinger-Straße 56, 67663 Kaiserslautern, Germany | 
| Workflow Person Affiliation   | RPTU Kaiserslautern-Landau | RPTU Kaiserslautern-Landau | 
| Workflow Person Roles         | Data Manager | Statistician | 
| Workflow Person Roles Term Accession Number | http://purl.obolibrary.org/obo/NCIT_C51820 | http://purl.obolibrary.org/obo/NCIT_C51877 | 
| Workflow Person Roles Term Source REF       | NCIT | NCIT | 


## RUN section

This section is organized in several subsections, described in detail below. The subsections in the block are arranged vertically; the intent being to enhance readability and presentation, and possibly to help with parsing. These subsections MUST remain within
this block; the fields MUST remain within their subsection.

These sections implement the metadata for a `Run`.

### RUN

This section MUST contain zero or one values.

This section MUST contain the following labels, with the specified datatypes for values supported:


| Label | Datatype | Description |
|-------|----------|-------------|
| Run Identifier          | String | A mandatory unique identifier, either a temporary identifier supplied by users or one generated by a repository or other database. For example, it could be an identifier complying with the LSID specification. A value MUST be given for this label. |
| Run Title               | String                               | A concise phrase used to encapsulate the purpose and goal of the run.  |
| Run Description         | String | A textual description of the run, with components such as objective or goals. |
| Run Workflow Identifiers | String | Identifiers of the Workflows whose execution was orchestrated in the context of this run. Multiple values can be provided by using a semicolon (“;”) Unicode (U0003+B) as a separator (e.g.: Imputation;Testing;Plotting). |
| Run Measurement Type    | String     | A term to qualify the endpoint, or what is being computed (e.g. gene expression profiling or protein identification). The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. |
| Run Measurement Type Term Accession Number | String     | The accession number from the Term Source associated with the selected term.  |
| Run Measurement Type Term Source REF       | String     | The Source REF has to match one of the Term Source Name declared in the Ontology Source Reference section. |
| Run Technology Type     | String | Term to identify the technology used to perform the computation, e.g. statistical testing. The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. |
| Run Technology Type Term Accession Number  | String     | The accession number from the Term Source associated with the selected term.  |
| Run Technology Type Term Source REF        | String     | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match one of the Term Source Names declared in the Ontology Source Reference. |
| Run Technology Platform | String     | Software Publisher/Creator and/or Software name used for the computation. |
| Run File Name           | String     | A field to specify the name of the Run Table file corresponding the definition of that run. There can be only one file per cell. |

**Example**

For example, the `RUN` section of an ISA-XLSX `isa.run.xlsx` file may look as follows:

|      |      |
|------|------|
| RUN |
| Run Identifier     | ProteomicsAnalysis     | 
| Run Title     | Preparatory Proteome Quantification | 
| Run Description     | Raw Data Files measured from Orbitrap device were processed using ProteomIQon to calculate protein quantities. | 
| Run Workflow Identifiers | ProteomIQonPipeline | 
| Run Measurement Type | Proteomics | 
| Run Measurement Type Term Accession Number | http://purl.obolibrary.org/obo/NCIT_C20085 |
| Run Measurement Type Term Source REF | NCIT | 
| Run Technology Type | Protein quantification | 
| Run Technology Type Term Accession Number | http://edamontology.org/operation_3630 | 
| Run Technology Type Term Source REF  | EDAM |
| Run Technology Platform | ProteomIQon  |
| Run File Name     | runs/ProteomicsAnalysis/isa.run.xlsx     | 


### RUN PERFORMERS

This section MUST contain zero or more values.

This section MUST contain the following labels, with the specified datatypes for values supported:

| Label | Datatype | Description |
|-------|----------|-------------|
| Run Person Last Name | String  | The last name of a person associated with the run.  |
| Run Person First Name  | String | Run Person Name |
| Run Person Mid Initials  | String  | The middle initials of a person associated with the run.|
| Run Person Email | String formatted as email | The email address of a person associated with the run. |
| Run Person Phone  | String | The telephone number of a person associated with the run. |
| Run Person Fax  | String | The fax number of a person associated with the run.  |
| Run Person Address  | String | The address of a person associated with the run. |
| Run Person Affiliation | String | The organization affiliation for a person associated with the run. |
| Run Person Roles | String or Ontology Annotation if accompanied by Term Accession Numbers and Term Source REFs | Term to classify the role(s) performed by this person in the context of the run, which means that the roles reported here need not correspond to roles held withing their affiliated organization. Multiple annotations or values attached to one person can be provided by using a semicolon (“;”) Unicode (U0003+B) as a separator (e.g.: submitter;funder;sponsor) .The term can be free text or from, for example, a controlled vocabulary or an ontology. If the latter source is used the Term Accession Number and Term Source REF fields below are required. Refer to section [Multiple values](#multiple-values) on how to encode multiple values in one field and match term sources.|
| Run Person Roles Term Accession Number | String | The accession number from the Term Source associated with the selected term. |
| Run Person Roles Term Source REF | String | Identifies the controlled vocabulary or ontology that this term comes from. The Source REF has to match one of the Term Source Names declared in the Ontology Source Reference section. |

**Example**

For example, the `RUN PERFORMERS` section of an ISA-XLSX `isa.run.xlsx` file may look as follows:

|                |         |       |
|----------------|---------|-------|
| RUN PERFORMERS |         |       |
| Run Person Last Name     | Ott | Zimmer  |
| Run Person First Name    | Caroline    | David   |
| Run Person Mid Initials  |       |       |
| Run Person Email         |  |       |
| Run Person Phone         |         |       |
| Run Person Fax           |         |       |
| Run Person Address       | Erwin-Schrödinger-Straße 56, 67663 Kaiserslautern, Germany | Erwin-Schrödinger-Straße 56, 67663 Kaiserslautern, Germany | 
| Run Person Affiliation   | RPTU Kaiserslautern-Landau | RPTU Kaiserslautern-Landau | 
| Run Person Roles         | Data Manager | Statistician | 
| Run Person Roles Term Accession Number | http://purl.obolibrary.org/obo/NCIT_C51820 | http://purl.obolibrary.org/obo/NCIT_C51877 | 
| Run Person Roles Term Source REF       | NCIT | NCIT | 

## Multiple values

Refer to the [Multiple values](./ISA-XLSX.md#multiple-values) section of the ISA-XLSX specification for guidance on handling cells with multiple annotation values.

# Annotation Table sheets

Refer to the [Annotation Table sheets](./ISA-XLSX.md#annotation-table-sheets) section of the ISA-XLSX specification for guidance on structuring and using annotation tables.

## Examples

todo

# Datamap Table sheets

reference back