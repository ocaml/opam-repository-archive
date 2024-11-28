# Opam-repository-archive

This repository contains archived OCaml packages, taken from the [main opam-repository](https://github.com/ocaml/opam-repository).

## Motivation

The motivation is that the [main opam-repository](https://github.com/ocaml/opam-repository) contains lots of packages, and lots of versions of packages. And a large part of older versions, patch releases, etc. - are no longer maintained or used. With the biannual releases of OCaml, there's also only a limited amount of OCaml versions supported by the main opam-repository.

The most important reason for the existence of this repository is *performance*, for OCaml developers and users: given the design of opam (text files, SMT solver), its performance is not linear with the amount of opam files in the repository. Also, it improves the experience for new users in search of the right libraries by clearing up the space from broken and unmaintained packages.

This is the home for packages that are not very likely to be of interest for the common OCaml developer. But when hunting a bug or performance regression or doing archeology, these packages may be very useful for you. Also, when doing changes to the OCaml compiler or runtime and wanting to do some quantative study.

If you're interested in more details, please read the [policy](https://github.com/ocaml/opam-repository/wiki/Package-Archiving:-Policy). There's also some history on how this repository came to be in the [initial issue: How does opam-repository scale?](https://github.com/ocaml/opam-repository/issues/23789).

This repository a medium-term solution for the question how to scale opam-repository, with the neat property that it works with deployed opam versions out of the box - no need to force everybody to have a new opam version which avoids the scaling issue by other means.

## Usage

You can add this repository to your active opam switch:

    opam repo add archive git+https://github.com/ocaml/opam-repository-archive


If you want to add it to all opam switches, add `--all` to the command above, and with `--set-default` you can add the repository to all newly created switches as well.

Afterwards you can install the available packages using `opam install mypackage` (or `mypackage.version`).

## Additional non-standard fields in the opam files

Each opam file in this repository contains some fields:

- `x-reason-for-archival`
  - Allowed values: A list of strings matching the primary repo criteria IDs (see below).
  - Meaning: Records the unmet primary repo criteria motivating the archiving.
  - Example: ["ocaml-version", "maintenance-intent"]
- `x-opam-repository-commit-hash-at-time-of-archival`
  - Allowed values: a string
  - Meaning: Records the commit hash of the primary repo at the time a package version is archived.
  - Example: "ca32ab3b976aa7abc00c7605548f78a30980d35b"

Criteria ID:
- `ocaml-version`: The package version does not satisfy the current compiler cutoff threshold.
- `source-available`: The sources of the package version are not available.
- `maintenance-intent`: The package version does not fall within a package's maintenance intent.
- `installable`: The package version is not installable.

An up-to-date field and criteria catalog is available at https://github.com/ocaml/opam-repository/wiki/Package-Archiving:-Policy

## Contributing

There are multiple ways to contribute: if you discover a package in here that does not install, and you have a fix for that (that likely involves adding some upper or lower bounds), please submit a PR.

If you want to move your old opam packages from the main opam-repository here, please:
- open a PR at opam-repository removing the opam files, and
- open a PR here to add these opam-files.

In the latter PR, please add some additional opam fields, as documented above.
Also, please have strict upper bounds for the dependencies (since opam-repository may evolve and this is not cross-tested at the time of submissions).

## License

All the metadata contained in this repository are licensed under the [CC0 1.0 Universal](http://creativecommons.org/publicdomain/zero/1.0/) license.

Moreover, as the collection of the metadata in this repository is technically a "Database" -- which is subject to a "sui generis" right in Europe -- we would like to stress that even the *collection* of the metadata contained in opam-repository is licensed under CC0 and thus the simple act of cloning opam-repository is perfectly legal.
