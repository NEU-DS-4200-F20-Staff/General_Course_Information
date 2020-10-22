# Altair tips, tricks, and troubleshooting

Below are some additional info that can help with your Altair assignments:

- [Altair tips, tricks, and troubleshooting](#altair-tips-tricks-and-troubleshooting)
  - [Resources](#resources)
  - [Optional setup instructions for larger/longer projects](#optional-setup-instructions-for-largerlonger-projects)
    - [Set up git to automatically clear metadata using JQ](#set-up-git-to-automatically-clear-metadata-using-jq)
    - [Jupyter Lab extensions](#jupyter-lab-extensions)
  - [Troubleshooting](#troubleshooting)
    - [Python issues](#python-issues)
    - [venv virtual environment issues](#venv-virtual-environment-issues)
    - [Other pip install issues](#other-pip-install-issues)
    - [Windows issues](#windows-issues)
    - [Mac issues](#mac-issues)
    - [Altair issues](#altair-issues)
    - [General troubleshooting advice](#general-troubleshooting-advice)

## Resources

Check the [Altair documentation](https://altair-viz.github.io/). Feel free to take inspiration from the [Altair example gallery](https://altair-viz.github.io/gallery/index.html).

## Optional setup instructions for larger/longer projects

These steps are only recommended if you plan to use JupyterLab often, for a larger project, or with collaborators.

### Set up git to automatically clear metadata using JQ

1. Install JQ by running, e.g.,

    ```zsh
    sudo apt-get install jq
    ```

    For more options including your operating system check [the JQ download site](https://stedolan.github.io/jq/download/).

2. Append the following block of code either in your local repo `.gitconfig` file or your global `.gitconfig`. I would recommend you do it in your global `.gitconfig` so you don't need to redo that for future .ipynb files.

    ```zsh
    [core]
    attributesfile = ~/.gitattributes_global
    [filter "nbstrip_full"]
    clean = "jq --indent 1 \
            '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
            | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
            | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
            | .cells[].metadata = {} \
            '"
    smudge = cat
    required = true
    ```

    For more details look at this great tutorial [here](http://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/).
    You can use this command

    ```zsh
    git config --list --show-origin --show-scope
    ```

    to find out where your git configuration is stored.

3. Create a global gitattributes named `.gitattributes_global` file (usually placed at the root level, so `~/.gitattributes_global`).

4. Add the following line of code:

    ```zsh
    *.ipynb filter=nbstrip_full
    ```

### Jupyter Lab extensions

* To get markdown section numbering use [jupyterlab-toc](https://github.com/jupyterlab/jupyterlab-toc).
  * To install run:

    ```zsh
    jupyter labextension install @jupyterlab/toc
    ```

  * Then click the enumerated list button on the left strip in JupyterLab to bring up the table of contents. There you can click the itemized list button in the top to add section numbers to the markdown cells.
​
* There is also a useful [Spellchecker](https://github.com/ijmbarr/jupyterlab_spellchecker) extension.

  * To install run:

    ```zsh
    jupyter labextension install @ijmbarr/jupyterlab_spellchecker
    ```

* To install both of the above wtih one (faster) command run:

    ```zsh
    jupyter labextension install @jupyterlab/toc @ijmbarr/jupyterlab_spellchecker
    ```

## Troubleshooting

### Python issues

* Are you using python 3.6 or newer? Check inside your virtual environment by running `python --version`. If not, [download and install](https://www.python.org/downloads/) the updated version of python for your OS.

* Are you using the [Anaconda Distribution](https://www.anaconda.com/distribution/)? We've had nothing but trouble using Anaconda with Jupyter Lab. See the instructions at the end of the [venv virtual environment section](#anaconda) below.

### venv virtual environment issues

* Are you in your virtual environment? Your command prompt / terminal prompt should be prefixed with `(env)` to show you that.

* Are you using the python executable from your virtual environment?
    1. Check it!
        * On macOS or Linux, run:

            ```zsh
            which python
            ```

        * On Windows, run:

            ```powershell
            where.exe python
            ```

            Check the path(s) provided by `which python` or `where.exe python` — the first one listed *should* be inside the `env` folder you just created.

    1. If the first listed path is not inside the `env` folder you just created, then find a way to run the correct python executable.

        * <a name="anaconda"></a>One common problem is if you have the [Anaconda Distribution](https://www.anaconda.com/distribution/) installed. E.g., your first listed path matches [one of these](https://docs.anaconda.com/anaconda/user-guide/tasks/integration/python-path/). In that case, one fix is to uninstall Anaconda completely ([option B listed here](https://docs.anaconda.com/anaconda/install/uninstall/)) and install basic [Python](https://www.python.org/downloads/) (if you don't have it already).

* Did you rename your `env` folder after creating it? If so, delete it and run the commands to create it again. `venv` uses hard-coded paths so renaming the folder is fraught.

### Other pip install issues

* You may get a warning like `WARNING: You are using pip version 20.1.1; however, version 20.2.3 is available. You should consuder upgrading...` You don't need to worry about fixing this.

* You may run into issues where pip is using a different python than Jupyter Lab is running. E.g., you may install a package but then Jupyter complains it is unavailable. In that case:
    1. instead of `pip install` try running

        ```zsh
        python -m pip install
        ```

    1. Additionally, check if python is from the same environment as pip:

        * On macOS or linux: `which pip` or `which pip3` and `which python`

        * On Windows: `where.exe pip` or `where.exe pip3` and `where.exe python`

* You can also try installing the required packages without pinning to particular versions like we have done in `requirements.txt`. Do this by running:

    ```zsh
    pip install -r requirements-noversions.txt
    ```

    You can also run the installs one-by-one to see if there are issues. E.g., `pip install altair`.

### Windows issues

* If you are using PowerShell (not the Command Prompt) and you get an error message saying `the execution of scripts is disabled on this system`, follow these steps.
    1. Open a new PowerShell as Administrator.
    1. Enable running unsigned scripts by entering

        ```powershell
        set-executionpolicy remotesigned
        ```

        See [the documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7) for details.

* If you get a `NotImplementedError` for `asyncio` while running Python 3.8, you may be able to resolve it by running

    ```powershell
    pip install notebook --upgrade
    ```

    If not, edit `/env/Lib/site-packages/tornado/platform/asyncio.py` (instructions from [here](https://stackoverflow.com/questions/58422817/jupyter-notebook-with-python-3-8-notimplementederror/)). Right after the line `import asyncio` add these lines:

    ```python
    import sys
    if sys.platform == 'win32':
        asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
    ```

* If you get this error `numpy.distutils.system_info.NotFoundError: no lapack/blas resources found` try installing it manually. (Instructions modified from [here](https://github.com/scipy/scipy/issues/5995).)

    1. In PowerShell or the Command Prompt `CD` to your repo folder, and **enter your virtual environment**.

    1. Download numpy+mkl wheel from one of the links [here](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy). Use the version that is the same as your Python version (check using `python --version`). E.g., if your Python is 3.6.*, download the wheel which shows cp36: `numpy‑1.18.5+vanilla‑cp36‑cp36m‑win_amd64.whl`. E.g., for Python 3.9.*: `numpy-1.19.2+mkl-cp39-cp39-win_amd64.whl`

    1. Install the wheel (where `numpy.whl` is the file name you downloaded):

        ```powershell
        pip install numpy.whl
        ```

    1. Likewise, download SciPy from one of the links [here](http://www.lfd.uci.edu/~gohlke/pythonlibs/#scipy) using the same version as your Python. E.g., for Python 3.6.*: `scipy‑1.5.3‑cp36‑cp36m‑win_amd64.whl`. E.g., for Python 3.9.*: `scipy-1.5.3-cp39-cp39-win_amd64.whl`

    1. Install the wheel (where `scipy.whl` is the file name you downloaded):

        ```powershell
        pip install scipy.whl
        ```

### Mac issues

* When you run `pip install -r requirements.txt`, `pip install numpy`, or `pip install scipy` you may get this error: `RuntimeError: Broken toolchain: cannot link a simple C program`. Note that this error may be in the middle / end of a large error message. It means that [Gcc](https://gcc.gnu.org/) is not available for compiling C programs (which Python is based on). Follow these steps:

    1. Try running

        ```zsh
        brew help
        ```

        to see if you have [Homebrew](https://brew.sh/) installed. If you get `command not found`, install Homebrew by running:

        ```zsh
        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
        ```

    1. Then run

        ```zsh
        brew install gcc
        ```

    1. Try running this again:

        ```zsh
        pip install -r requirements.txt
        ```

### Altair issues

* Read the relevant part of [the Altair documentation](https://altair-viz.github.io/user_guide/API.html).

* Check [the UW Visualization Curriculum debugging guide](https://github.com/uwdata/visualization-curriculum/blob/master/altair_debugging.ipynb) and the [Altair FAQ](https://altair-viz.github.io/user_guide/faq.html) for answers.

* The developers of Altair sometimes release a new version via pip and update [the documentation](https://altair-viz.github.io/user_guide/API.html) before it is stable.
    Note that the documentation version and Altair version you are using may be out of sync. (See the top-left of the documentation page for the version number, and in Jupyter Lab run `alt.__version__` in a cell to see the Altair version). In this assignment, using the `requirements.txt` file we are asking you to install a particular version to avoid some issues like this.

### General troubleshooting advice

* Googling error messages is the best way to help you sort out weird issues with Python and Jupyter Lab.

* You can also post a discussion topic on Canvas and we and other students will try to help!
