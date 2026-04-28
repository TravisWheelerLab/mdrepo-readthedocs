# TOML Tools (mdr-meta)

Use [mdr-meta](https://github.com/TravisWheelerLab/mdrepo-rs/releases) tool to help create and validate the TOML/metadata file.

Run the command with no arguments to see the available commands:

```
$ mdr-meta
Usage: mdr-meta [COMMAND]

Commands:
  check    Check TOML
  eg       Create a full example file
  gen      Generate metadata file from directory contents
  to-json  Print metadata in JSON format
  to-toml  Print metadata in TOML format
  upgrade  Upgrade binary to the latest release
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

Following are some of the most important commands

## eg

The `eg` command will generate a full example TOML file you can use as a template:

```
$ mdr-meta eg -h
Create a full example file

Usage: mdr-meta eg [OPTIONS]

Options:
  -f, --format <FORMAT>   Output format [possible values: json, toml]
  -o, --outfile <OUTPUT>  Output filename [default: -]
  -m, --minimal           Only output required fields
  -h, --help              Print help
```

For example:

```
$ mdr-meta eg
lead_contributor_orcid = "0000-0000-0000-0000"
trajectory_file_name = "traj.xtc"
structure_file_name = "struct.pdb"
topology_file_name = "topology.gro"
temperature_kelvin = 300
integration_timestep_fs = 2
short_description = "<short_description> (required)"
software_name = "GROMACS"
software_version = "2024.5"
description = "<longer description>"
toml_version = 2
alias = "ABC123"
run_commands = "gmx_mpi mdrun"
forcefield = "CHARMM36m"
forcefield_comments = "ligand params: CGenFF and SwissParam"
protonation_method = "PropKa"
pdb_id = "5emo"
uniprot_ids = [
    "A0A0H2UWN8",
    "S8G8I1",
]
dois = ["10.1017/j.str.2019.08.032"]

[water]
model = "TIP3P"
density_kg_m3 = 986.0

[[additional_files]]
file_name = "README.txt"
file_type = "User-defined file"
description = "Documentation of methods"

[[ligands]]
name = "Foropafant"
smiles = "CC(C)C1=CC(=C(C(=C1)C(C)C)C2=CSC(=N2)N(CCN(C)C)CC3=CN=CC=C3)C(C)C"

[[solutes]]
name = "Na+"
concentration_mol_liter = 0.15

[[solutes]]
name = "Cl-"
concentration_mol_liter = 0.15

[[external_links]]
url = "http://aol.com"
label = "My Link"

[[contributors]]
name = "Barbara McClintock"
orcid = "0000-0002-6897-9608"
email = "barb@cshl.edu"
institution = "Cold Spring Harbor Laboratory"
```

Or use the `-m|--minimal` flag to generate TOML with the fewest acceptable fields:

```
$ mdr-meta eg -m
lead_contributor_orcid = "0000-0000-0000-0000"
trajectory_file_name = "traj.xtc"
structure_file_name = "struct.pdb"
topology_file_name = "topology.gro"
temperature_kelvin = 300
integration_timestep_fs = 2
short_description = "<short_description> (required)"
software_name = "GROMACS"
software_version = "2024.5"
```

## check

You can use the `check` option to find errors in your TOML. 
For example, this minimal example has a valid TOML structure:

```
$ cat minimal_bad.toml
lead_contributor_orcid = ""

trajectory_file_name = ""

structure_file_name = ""

topology_file_name = ""

temperature_kelvin = 0

integration_timestep_fs = 0

software_name = ""

software_version = ""

short_description = ""
```

But every field has a validation error:

```
$ mdr-meta check minimal_bad.toml
integration_timestep_fs: value 0 must be >= 1 and <= 5
lead_contributor_orcid: value "" invalid
short_description: value "" invalid
software_name: "" invalid, choose from ACEMD, AMBER, CHARMM, GROMACS, NAMD, SPONGE
software_name: value "" invalid
software_version: value "" invalid
structure_file_name: filename is missing extension
structure_file_name: value "" invalid
temperature_kelvin: value 0 must be >= 275 and <= 700
topology_file_name: filename is missing extension
topology_file_name: value "" invalid
trajectory_file_name: filename is missing extension
trajectory_file_name: value "" invalid
Filename "" is duplicated 3 times
Missing PDB and Uniprot IDs (skip with --no-id)
```

NOTE: We would prefer every example contain a `pdb_id` and/or one or more `uniprot_ids`, but it's possible to validate using the `--no-id` option.

## gen

Use the `gen` option to generate a TOML file from a prepared directory:

```
$ mdr-meta gen -h
Generate metadata file from directory contents

Usage: mdr-meta gen [OPTIONS]

Options:
  -d, --directory <DIR>     Output format
  -f, --format <FORMAT>     Output format [possible values: json, toml]
  -o, --outfile <OUTPUT>    Output filename [default: -]
      --trajectory <TRAJ>   Trajectory filename
      --structure <STRUCT>  Structure filename
      --topology <TOPO>     Topology filename
      --template <TMPL>     TOML template
  -h, --help                Print help
```

Using the original files from [MDR00016593](https://mdrepo.org/explore/MDR00016593) as an example:

```
$ ls -1 MDR00016593
5jz9_cleaned.pdb
5jz9_gromacs_cleaned.gro
5jz9_gromacs_cleaned.top
5jz9_per_frame_features.csv
5jz9_production_gromacs.gro
5jz9_production_gromacs.top
5jz9_production.pdb
5jz9.xtc
production.rst
production.top

$ mdr-meta gen -d MDR00016593 -o MDR00016593/mdrepo-metadata.toml
```

Will generate the following TOML:

```
lead_contributor_orcid = "0000-0000-0000-0000"
trajectory_file_name = "production.rst"
structure_file_name = "5jz9_production_gromacs.gro"
topology_file_name = "production.top"
temperature_kelvin = 300
integration_timestep_fs = 2
short_description = "<short_description> (required)"
software_name = "<software_name> (required)"
software_version = "<software_version> (required)"

[[additional_files]]
file_name = "5jz9.xtc"
file_type = "Trajectory"

[[additional_files]]
file_name = "5jz9_cleaned.pdb"
file_type = "Structure"

[[additional_files]]
file_name = "5jz9_gromacs_cleaned.gro"
file_type = "Structure"

[[additional_files]]
file_name = "5jz9_gromacs_cleaned.top"
file_type = "Topology"

[[additional_files]]
file_name = "5jz9_per_frame_features.csv"
file_type = "Other"

[[additional_files]]
file_name = "5jz9_production.pdb"
file_type = "Structure"

[[additional_files]]
file_name = "5jz9_production_gromacs.top"
file_type = "Topology"

[[additional_files]]
file_name = "mdrepo-metadata.toml"
file_type = "Other"

[[contributors]]
name = "<Your Name>"
orcid = "<orcid> (optional)"
email = "<email> (optional)"
institution = "<institution> (optional)"
```

## upgrade

Use the `upgrade` command to download and install the latest version of the program:

```
$ mdr-meta upgrade
Checking target-arch... x86_64-unknown-linux-gnu
Checking current version... v0.3.6
Checking latest released version... v0.3.8
New release found! v0.3.6 --> v0.3.8
New release is compatible

mdr-meta release status:
  * Current exe: "/home/u20/kyclark/.local/bin/mdr-meta"
  * New exe release: "mdr-meta-x86_64-unknown-linux-gnu.tar.gz"
  * New exe download url: "https://api.github.com/repos/TravisWheelerLab/mdrepo-rs/releases/assets/407522444"

The new release will be downloaded/extracted and the existing binary will be replaced.
Do you want to continue? [Y/n] y
Downloading...
[00:00:00] [========================================] 2.80 MiB/2.80 MiB (0s) Done                                                                               Extracting archive... Done
Replacing binary file... Done
Update status: `0.3.8`!
```
