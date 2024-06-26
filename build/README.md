# Build scripts for container definition files

This folder contains scripts and other files to create (or assist in creating) definition files needed to build Rocker
container images.

## Automatic update of container definition files

The Dockerfiles for pre-build images will be generated by executing the R scripts in the following order.

1. `build/scripts/generate-variables.R`
2. `build/scripts/generate-args.R`
3. `build/scripts/generate-matrix.R`
4. `build/scripts/generate-bakefiles.R`
5. `build/scripts/generate-dockerfiles.R`
6. `build/scripts/clean-files.R`

These scripts are run daily by GitHub Actions and automatically create a Pull Request if the run results in a variance
from the default branch.

## Reports of built images

Since 2021-08-10, the information of images built from this repository will be registered in [the Wiki](https://github.com/rocker-org/rocker-versioned2/wiki) automatically.

Running `make inspect-image-all` (requires `docker`), then `make report-all` (requires R >= 4.1.0 and some packages) will generate reports about rocker images, in `reports/` directory (ignored by git). The template for the reports is `build/reports/template.Rmd` and the script for giving variables to the template is `./build/knit-report.R`.

The wiki HOME(`Home.md`) is generated from `build/reports/wiki_home.Rmd`.

## How to control which Docker images to build

### Stop building a specific R version

If you no longer need to build a particular R version, please edit the `build/scripts/generate-matrix.R` script
and exclude that R version from the build matrix.
Matrix files are reffered to in many places.

### Stop building a specific image of a specific R version

Images that are not included in the `groups` defined in `docker-bake.json` files will not be built, so if you want to select images to build in each version of the stack file, edit the stack files and the `make-stacks.R` script to adjust the contents of `groups`.
