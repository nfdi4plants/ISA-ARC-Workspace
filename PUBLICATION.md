# Publication and citation

The workspace, ISA-XLSX, and WR-XLSX specifications are separately referenceable documents. Their registered persistent identifiers use the existing ARC namespace on w3id.org.

| Document | Current version | Persistent identifier |
|---|---|---|
| [ISA-ARC Workspace specification](ISA-ARC-Workspace.md) | 3.0.0-draft.2 | <https://w3id.org/arc/workspace/isa-arc> |
| [ISA-XLSX format](ISA-XLSX.md) | 3.0.0-draft.1 | <https://w3id.org/arc/workspace/isa-arc/isa-xlsx> |
| [WR-XLSX format](WR-XLSX.md) | 1.0.0-draft.1 | <https://w3id.org/arc/workspace/isa-arc/wr-xlsx> |

All three editions are drafts. These identifiers name the documents; they are not version-specific identifiers or separate DOIs. For a reproducible citation, include the document title, version, and a GitHub permalink to the exact commit, as well as its persistent identifier.

## Dependencies

ISA-ARC Workspace 3.0.0-draft.2 references ISA-XLSX 3.0.0-draft.1 and WR-XLSX 1.0.0-draft.1. WR-XLSX extends that ISA-XLSX edition. ISA-XLSX retains the shared sheet and field rules, while the workspace specification retains folder-layout and data-path requirements.

Versions are stated per document. Publishing separate documents does not introduce a new registration mechanism, make the workspace layout optional, or define standalone ontology-source resolution.

## Publishing a snapshot

The existing [w3id redirects](https://github.com/perma-id/w3id.org/blob/master/ids/arc/workspace/.htaccess) resolve through corresponding pages on arc-rdm.org. The [website's reference-route configuration](https://github.com/nfdi4plants/arc-rdm.org/blob/main/src/data/reference.ts) controls their final destinations.

1. Review the specification changes against the source edition and validate document links. Contributions target this repository's `dev` branch.
2. Make the reviewed documents available at one immutable Git commit. Use that same commit for all three source targets so their relative links resolve within one consistent snapshot.
3. Point the three existing website routes to GitHub URLs of the form `https://github.com/nfdi4plants/ISA-ARC-Workspace/blob/<commit>/<document>.md`. Do not point citation targets at a moving development branch.
4. Run the website's build and reference-route checks, publish through its existing deployment process, and verify all three w3id redirect chains, including section fragments.
5. For a later edition, update document versions and explicit dependencies before deliberately advancing the document-level routes. Previously cited commit permalinks remain unchanged.

This procedure publishes the existing draft documents; it does not replace the repository's existing release tags, issue a new formal release, or mint new DOIs. Version-specific w3id routes would require a separate namespace change and are not introduced by this split.

## Scope of the extraction

The source for the split is [revision b597624](https://github.com/nfdi4plants/ISA-ARC-Workspace/commit/b597624b95b1ed951cdd447e2ea78bc08aa1509d). Metadata labels, datatypes, cardinalities, and existing examples are retained. Common requirements are referenced from ISA-XLSX rather than duplicated in WR-XLSX.

The extraction does not resolve inherited semantic inconsistencies. In particular, the original introduction recognizes two sheet formats although datamap sheets are defined separately, and the ISA datamap introduction mentions Study/Assay data while the workspace already describes workflow/run datamaps. These statements retain their original scope pending a separate clarification. Likewise, the shared block-containment rule applies only to explicitly declared blocks, and the WR filename list describes existing workspace conventions without adding a new mandatory naming rule.

Further changes are tracked separately: [registration](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/188), [row-to-workflow mapping](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/189), [software components](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/190), [standalone ontology sources](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/191), [workspace externalization](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/192), [example labels](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/193), and [a computational example](https://github.com/nfdi4plants/ISA-ARC-Workspace/issues/194).
