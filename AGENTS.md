# mdrepo-readthedocs

Contributor-facing documentation for MDRepo, built with mdBook and published to
Read the Docs. This is what a scientist reads before their first submission.

## Commands

```console
mdbook serve docs     # live preview
mdbook build docs
```

`.readthedocs.yaml` drives the hosted build.

## Things to know

- **`docs/src/SUMMARY.md` is the navigation.** A page not listed there is not
  built into the book and is unreachable, even though the file exists.
- **`upload_prep.md` is currently orphaned** — absent from `SUMMARY.md`, and stale
  where it contradicts `contributing.md` ("one trajectory file" versus "one or
  more"). The reachable copy is the correct one.
- **`toml_spec.md` is the contributor-facing contract** for
  `libmdrepo::metadata::Meta`. When a validation rule changes in `mdrepo-rs`, this
  page has to change with it — they drift silently otherwise.
- **Version skew is a real support burden.** The spec already notes that `mdr-meta`
  releases up to v0.3.23 reject an InChI-only ligand the archive accepts. Any
  validation change that lands server-side before contributors upgrade needs the
  same treatment.
- **`citation.md` cites the MDRepo paper**, which is how to cite the *archive*.
  There is currently no guidance for citing an individual simulation, and no DOI
  is minted per simulation.
