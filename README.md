# MRDT Embedded Docs

Welcome to the embedded software documentation for the Missouri S&T Mars Rover
Design Team. This wiki contains API docs, setup guides, tutorials,
tips & tricks, and best practices for writing our embedded software.

This is the source code for the documentation which you can find hosted live
[here]().

![teensy henge](docs/Sphinx/source/teensy_henge.jpg)

## Setup Guide

1. Clone this repo to your local machine.
2. Install `doxygen`, `python3`, and `python3-venv`
3. cd into `docs/Doxygen` and run the `doxygen` command to generate
   documentation from the embedded libraries' comments
4. cd into `../Sphinx` and open the python virtual environment with
   `python -m venv .` then `source bin/activate`
5. Once in venv, install dependencies with `pip install -r requirements.txt`
6. Build the docs with `make html`
7. Open the docs at `build/html/index.html`. The VS Code "Live Server"
   extension is very helpful for this.
8. To get out of venv type `deactivate`

## Contributing

If anything here is outdated or too confusing, *please please please* don't
hesitate to make your own improvements. This repo is free for all members to
add to, so anything that you wish you would have known going in may very well
help someone else just getting started. Thank you!
