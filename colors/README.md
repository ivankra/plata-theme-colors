## Plata Colors

[Plata](https://gitlab.com/tista500/plata-theme) is a Gtk theme based on Material Design Refresh and a successor to the popular [Adapta](https://github.com/adapta-project/adapta-gtk-theme) theme.

This directory contains scripts for building it and its variants with different key colors using Docker.

Usage: install Docker and run in a Unix environment:
  * `make Plata` - to build standard theme. Run this command first to warm up Docker's cache.
  * `make -j` - to build the rest of variants in parallel.
  * `make REVISION=0.9.1 -j` - to build from a particular git tag/branch/commit.

You will find distributable output files in `./dist/` directory at the end.

For further information about the theme, please refer to the [upstream repository](https://gitlab.com/tista500/plata-theme).
