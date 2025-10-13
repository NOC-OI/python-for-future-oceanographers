---
title: Setup
---

## Setup

We expect most learners to use a pre-configured environment for this and not to install it themselves. 
If you are taking this course with the National Oceanography Centre please follow the instructions you have been given on how to 
connect to a system with everything you need. 

As an emergency fallback a pre-configured MyBinder image is available which will provide all the required software. If you have not been provided 
with any other system then use this. Click on the "launch binder" icon below. Please note that this system
does not have many computational resources and will stop your notebook if you leave it idle for a few minutes. 

[![Run this lesson on MyBinder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/NOC-OI/python-for-future-oceanographers/binder)


## If you really want/need to run this on your own computer

**Anybody taking this course with the National Oceanography Centre should not have to do this!**

This lesson is designed to be run on a computer with Python and the Matplotlib, Numpy, Argopy and Cartopy libraries installed.

### Install Python

Users of the NOC Data Science Platform or Binder Hub can skip this section and move on to the "Obtain lesson materials" section below.

#### Install Miniforge

If Conda has not been installed on your machine, then install [Miniforge](https://conda-forge.org/download/) for your OS. As the name
suggests, Miniforge is a "mini" version of the
[Anaconda Python distribution](https://www.anaconda.com/download) that includes only Conda, a
Python 3 distribution, and any necessary OS-specific dependencies.

For convenience here are links to the 64-bit Miniconda installers.

* [Windows](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Windows-x86_64.exe)
* [Mac OSX - Intel CPU](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh)
* [Mac OSX - Apple M1/2/3 CPU](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh)
* [Linux](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh)


##### Windows installation

After you downloaded the [Windows installer](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Windows-x86_64.exe), double click on it and follow the instructions (accept license, etc.).
Make sure you tick on **"Add Miniforge3 to my PATH environment variable"** option.

##### Mac OSX or Linux installation

First, download the 64-bit Python 3 install script for Miniforge
either by clicking the link above or using this command in your terminal:

```bash
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
```

Run the Miniforge install script from your terminal. Follow the prompts on the installer screens. If you are unsure
about any setting, accept the defaults (you can change them later if necessary).

```bash
bash Miniforge3-$(uname)-$(uname -m).sh
```

Once the install script completes, you can remove it.

```bash
rm Miniforge3-$(uname)-$(uname -m).sh
```

##### Initalizing Conda (Mac and Linux Only)

Near the end of the installation you will be asked if you wish to initialize conda. This will place the
conda executables in your path, meaning your system can easily find them. You want to do this.

When asked the following question:

```output
Do you wish to update your shell profile to automatically initialize conda?
This will activate conda on startup and change the command prompt when activated.
```

Answer with "yes" (the default is no). 
If you pressed enter during the install and followed the default no then you can fix this typing
`conda init` immediately after running the installer. 

In order for these settings to be found you might need to close and re-open your terminal to apply the new path settings.

#### Verifying your Conda installation

In order to verify that you have installed Conda correctly run the `conda help` command. Output
of the command should look similar to the following.

```bash
$ conda help
usage: conda [-h] [-V] command ...

conda is a tool for managing and deploying applications, environments and packages.

Options:

positional arguments:
  command
    clean        Remove unused packages and caches.
    config       Modify configuration values in .condarc. This is modeled
                 after the git config command. Writes to the user .condarc
                 file (/Users/drpugh/.condarc) by default.
    create       Create a new conda environment from a list of specified
                 packages.
    help         Displays a list of available conda commands and their help
                 strings.
    info         Display information about current conda install.
    init         Initialize conda for shell interaction. [Experimental]
    install      Installs a list of packages into a specified conda
                 environment.
    list         List linked packages in a conda environment.
    package      Low-level conda package utility. (EXPERIMENTAL)
    remove       Remove a list of packages from a specified conda environment.
    uninstall    Alias for conda remove.
    run          Run an executable in a conda environment. [Experimental]
    search       Search for packages and display associated information. The
                 input is a MatchSpec, a query language for conda packages.
                 See examples below.
    update       Updates conda packages to the latest compatible version.
    upgrade      Alias for conda update.

optional arguments:
  -h, --help     Show this help message and exit.
  -V, --version  Show the conda version number and exit.

conda commands available from other packages:
  env
```

At the bottom of the help menu you will see a section with some optional arguments for the
`conda` command. In particular you can pass the `--version` flag which will return the version
number. Again output should look similar to the following.

```bash
$ conda --version
conda 4.8.2
```


#### Install the Required Python Libraries

Run the following command to install the required dependencies.
```bash
$ conda install jupyterlab pip python=3.11 "numpy>2.0" matplotlib-base matplotlib-inline argopy cartopy
```

### Launch Python interface

To start working with Python, we need to launch a program that will interpret and execute our
Python commands. Below we list several options. If you don't have a preference, proceed with the
top option in the list that is available on your machine. Otherwise, you may use any interface
you like.

A Jupyter Notebook provides a browser-based interface for working with Python.
If you installed Miniforge, you can launch a notebook from the terminal:


::::::::::::::::: spoiler

### Unix shell

If you're using a Unix shell application, such as Terminal app in macOS, Console or Terminal
in Linux, or [Git Bash](https://gitforwindows.org/) on Windows, execute the following command:

```bash
jupyter lab
```

:::::::::::::::::::::::::

::::::::::::::::: spoiler

## Command Prompt (Windows)

On Windows, you can use its native Command Prompt program.  The easiest way to start it up is
pressing <kbd>Windows Logo Key</kbd>\+<kbd>R</kbd>, entering `cmd`, and hitting
<kbd>Return</kbd>. In the Command Prompt, use the following command to start Jupyter:


```bash
jupyter lab
```

:::::::::::::::::::::::::
