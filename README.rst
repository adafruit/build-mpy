.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord

.. image:: https://github.com/circuitpython/circuitpython-unified-build-ci/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_VEML7700/actions/
    :alt: Build Status

build-mpy
=========

GitHub Action for building packages of .mpy files for CircuitPython projects and attaching them to releases
as ZIP files.

Examples
========

If you have just a repository with files intended for a CircuitPython board, your release
file could be very simple!  This release CI creates .mpy files for CircuitPython 7.2.0:

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
          uses: tekktrik/build-mpy@main
          with:
            github-token: ${{ secrets.GITHUB_TOKEN }}
            cpy-tag: "7.2.0"

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
          uses: tekktrik/build-mpy@main
          with:
            github-token: ${{ secrets.GITHUB_TOKEN }}
            cpy-tag: "7.2.0"
            mpy-directory: "microcontroller"
            mpy-manifest-file: "mpy_manifest.txt"
            mpy-manifest-type: "exclude"
            zip-directory: "microcontroller"
