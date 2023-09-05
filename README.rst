.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord

.. image:: https://github.com/circuitpython/circuitpython-unified-build-ci/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_VEML7700/actions/
    :alt: Build Status

build-mpy
=========

GitHub Action for building packages of ``.mpy`` files for CircuitPython projects and attaching them to releases
as ZIP files.  Files other than ``.mpy`` and ``.py`` files will be added to the ZIP file as well.  Note that
any files named or ``code.py`` are automatically not compiled for convenience.

Inputs
======

======================= ===================================================================== ==================== =====================================================================
     Argument Name                                 Description                                       Default                              Notes
======================= ===================================================================== ==================== =====================================================================
github-token            Your GitHub token                                                     N/A                  N/A
circuitpy-tag           The version of CircuitPython to compile for                           N/A                  You can use any valid tag (or branch) from ``adafruit/circuitpython``
zip-filename            The name of the ZIP file that will be attached                        "mpy-release.zip"    N/A
circuitpython-repo-name The name of the clone CircuitPython repo                              "circuitpython-repo" Change if it conflicts with another file
mpy-directory           The directory to search for files to compile                          "." (top folder)     Becomes the basis for filepaths in ``mpy-manifest-file``
mpy-manifest-file       A file with a list of files to compile or exclude                     ""                   If none is given, all files in ``mpy-directory`` are used
mpy-manifest-type       Whether the files in the manifest file should be included or excluded "include"            N/A
zip-directory           The directory to add to the ZIP file                                  "." (top folder)     Becomes the basis for filepaths in ``zip-manifest-file``
zip-manifest-file       A file with a list of files to add to the ZIP file or exclude from it ""                   If none is given, all files in ``zip-directory`` are used
zip-manifest-type       Whether the files in the manifest file should be included or excluded "include"            N/A
======================= ===================================================================== ==================== =====================================================================

Examples
========

If you have just a repository with files intended for a CircuitPython board, your release
file could be very simple!  This release CI creates .mpy files for CircuitPython 8.2.0:

.. code-block:: yaml

    on:
      release:
        types: [published]

    jobs:
      upload-mpy-zips:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout the current repo
          uses: actions/checkout@v3
          with:
            submodules: true
        - name: Run MPY Action
          uses: adafruit/build-mpy@v1
          with:
            github-token: ${{ secrets.GITHUB_TOKEN }}
            circuitpy-tag: "8.2.0"

You also have granular control of which directories to compile and zip and the ability to specify which
files should or should not be compiled and/or zipped.  For example, if you wanted to compile and zip
files in a folder named ``microcontroller`` and you wanted to use a manifest file named ``mpy_manifest.txt``
to specify certain files NOT to compile, you could modify the script above to be:

.. code-block:: yaml

    on:
      release:
        types: [published]

    jobs:
      upload-mpy-zips:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout the current repo
          uses: actions/checkout@v3
          with:
            submodules: true
        - name: Run MPY Action
          uses: adafruit/build-mpy@v1
          with:
            github-token: ${{ secrets.GITHUB_TOKEN }}
            circuitpy-tag: "8.2.0"
            mpy-directory: "microcontroller"
            mpy-manifest-file: "mpy_manifest.txt"
            mpy-manifest-type: "exclude"
            zip-directory: "microcontroller"
