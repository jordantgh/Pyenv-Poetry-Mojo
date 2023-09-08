## Introduction

This repo shows you how to set up a Mojo🔥 project with Python interop using `pyenv`, `poetry` and `direnv`. Use `pyenv` to manage Python versions, `poetry` for handling dependencies, and Mojo🔥 for coding.

## Prerequisites

I assume you have Python, Mojo🔥, `pyenv`, `direnv` and `poetry` installed on your machine. I set this up on WSL2 Ubuntu 22.04.

## Setting up `pyenv`

1. **Install Python Version**: If you haven't already, install the desired Python version.
    ```bash
    pyenv install <python-version>
    ```

2. **Set Local Python Version**: Set the Python version for your project directory.
    ```bash
    pyenv local <python-version>
    ```

## Setting up `poetry`

1. **Navigate to Project**: Navigate to your existing project directory or create a new one.

2. **Install Dependencies**: Install project dependencies.
    ```bash
    poetry install
    ```

3. **Poetry Configurations**: Execute the following commands to set up `poetry` according to preferences.
    ```bash
    poetry config virtualenvs.prefer-active-python true
    poetry config virtualenvs.in-project true
    ```

## Integrating `pyenv`, `poetry`, and Mojo🔥

1. **Setting up `.envrc` with `direnv`**: Create a `.envrc` file in your project directory with the following content:

    ```bash
    # Extract major and minor Python version
    PYTHON_VERSION=$(pyenv version-name | awk -F. '{print $1"."$2}')

    # Find Python binary and poetry venv path
    PYTHON_BIN=$(pyenv which python)
    POETRY_VENV_PATH=$(poetry env info -p)

    # Set MOJO_PYTHON_LIBRARY and PYTHONPATH
    export MOJO_PYTHON_LIBRARY="${PYTHON_BIN%/bin/python}/lib/libpython${PYTHON_VERSION}.so"
    export PYTHONPATH="${POETRY_VENV_PATH}/lib/python${PYTHON_VERSION}/site-packages"
    ```

2. **Activate `.envrc`**: Run the following command to activate the `.envrc` file.
    ```bash
    direnv allow
    ```

## Verify Setup

1. **Check Environment Variables**: Ensure `MOJO_PYTHON_LIBRARY` and `PYTHONPATH` are set correctly.
    ```bash
    echo $MOJO_PYTHON_LIBRARY
    echo $PYTHONPATH
    ```

2. **Run a Mojo Script**: Create and run a simple Mojo script that imports a Python library to verify that everything is set up correctly.