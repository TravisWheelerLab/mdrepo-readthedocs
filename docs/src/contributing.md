![logo](./logo.png)

# Submission Guidelines

If you wish to submit data to MDRepo, you must first [login](https://mdrepo.org/login) using [ORCID](https://orcid.org/).

You must then ask the MDRepo administrators for the ability to upload.

When contributing your data, you acknowledge that it will be freely available in perpetuity under the [CC BY 4.0 license](https://creativecommons.org/licenses/by/4.0/).

Ask any questions at [help@mdrepo.org](mailto:help@mdrepo.org).

## Preparing Your Upload

Your prepared submission should consist of a single directory containing a set of subdirectories. There should be one subdirectory for each simulation that you wish to upload, and no other subdirectories. Your directory (and subdirectories) can be prepared on a server (i.e. they do not need to be present on the computer where you access the MDRepo website). 

Each simulation directory should contain the following:

* One trajectory file.
* One structure/coordinate file.
* One topology file (.psf for CHARMM, NAMD, XPLOR, .top, .itp, .tpr for GROMACS, .prmtop for AMBER, etc.)
* Any additional files produced with the simulation. Maximum file size for all uploaded files is 40GB.
* A simulation metadata file named `mdrepo-metadata.toml` in TOML format. Use the [metadata](https://mdrepo.org/metadata) form or the [mdr-meta](https://github.com/TravisWheelerLab/mdrepo-rs/releases) command-line tool to generate and validate metadata files in the [TOML specification](./toml_spec.html).

## Upload Tokens

Use the [upload tokens](https://mdrepo.org/upload-tokens) page to get an upload token.

Note that you can submit multiple simulations per token if you place each simulation directory in a parent directory.

## Uploading with mdrepo

Install the [mdrepo](https://github.com/MD-Repo/md-repo-cli/releases) command line tool into your `$PATH` on the machine that contains the submission data.

Once your token is ready for upload, use the command `mdrepo submit <upload_directory>` where `<upload_directory>` is the local parent directory of your simulation files. Enter your upload token when prompted.

If your upload is interrupted, you may restart the process using the same command and upload token. Any files that have already been uploaded will be skipped.
