---
title: Conda Commands
date: 2023-12-28
lastmod: 2023-12-28
tags:
- software
enableToc: true
---
If you're like me and you have trouble remembering the conda commands to make environments, remove environments and list environments, here are commands that I added to my `.bashrc` file to make it easier.

## Create Conda Environment
Command to make a conda environment that takes in the name of the environment and the python version. 
```bash
#!/bin/bash

# Create a conda environment. Take in as input the name of the environment and the python version.
# If the python version is not specified, use the default python version.
# If environment name is not specified, or if the environment already exists, exit with an error message.

function mkconda() {
    echo "Specify the name of the environment:"
    read env_name

    # Check if the environment name is empty
    if [ -z "$env_name" ]; then
        echo "Environment name cannot be empty."
        return 1
    fi

    if conda env list | grep -q "^$env_name\s"; then
        echo "Environment $env_name already exists."
        return 1
    fi
    echo "Specify the python version (default: 3.11):"
    read py_version

    if [ -z "$py_version" ]; then
        py_version=3.11
    fi

    # Multiply by 10 and remove the decimal point
    py_version_int=${py_version//.}
    min_version=38
    max_version=311

    if (( py_version_int < min_version )) || (( py_version_int > max_version )); then
        echo "Python version must be between 3.8 and 3.11."
        return 1
    fi

    conda create -n $env_name python=$py_version
}
```

## Remove Conda Environment
Command to remove a conda environment that takes in the name of the environment. 
```bash
#!/bin/bash

# Remove a conda environment. Take in as input the name of the environment.
# If the environment name is not specified, or if the environment does not exist, exit with an error message.

function rmconda() {
    echo "Specify the name of the environment:"
    read env_name

    # Check if the environment name is empty
    if [ -z "$env_name" ]; then
        echo "Environment name cannot be empty."
        return 1
    fi

    if ! conda env list | grep -q "^$env_name\s"; then
        echo "Environment $env_name does not exist."
        return 1
    fi

    conda remove -n $env_name --all
}
```
## List Conda Environments
Command to list all conda environments.
```bash
#!/bin/bash

# List all conda environments.

function lsconda() {
    conda env list
}
```
Run `source ~/.bashrc` to update the bash commands. Now you can run `mkconda`, `rmconda` and `lsconda` to create, remove and list conda environments respectively.