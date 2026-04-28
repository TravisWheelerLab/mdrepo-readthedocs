![logo](./logo.png)

# Preparing Your Upload

Your prepared submission should consist of a single directory containing a set of subdirectories. There should be one subdirectory for each simulation that you wish to upload, and no other subdirectories. Your directory (and subdirectories) can be prepared on a server (i.e. they do not need to be present on the computer where you access the MDRepo website). 

Each simulation directory should contain the following:

* One trajectory file.
* One structure/coordinate file.
* One topology file (.psf for CHARMM, NAMD, XPLOR, .top, .itp, .tpr for GROMACS, .prmtop for AMBER, etc.)
* Any additional files produced with the simulation. Maximum file size for all uploaded files is 40GB.
* A simulation metadata file named `mdrepo-metadata.toml` in TOML format. Use the provided form to generate a file or download the [mdr-meta](https://github.com/TravisWheelerLab/mdrepo-rs/releases) to generate and validate metadata files in the TOML Specification.
