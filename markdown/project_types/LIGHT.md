# CICD-Tools

This is documentation for a lightweight installation that sets up [poetry](https://python-poetry.org/) and [pre-commit](https://pre-commit.com/) for your project.

## Light Installation Into Existing Projects

If you have an existing project you'd like to use CICD-Tools with this is an excellent option for:
- Python or non-Python based projects
- projects where you would like to introduce [pre-commit](https://pre-commit.com/) hooks

Although [pre-commit](https://pre-commit.com/) and [poetry](https://python-poetry.org/) are written in Python, they can also be used by projects in other languages (such as Golang or Ruby).

There are language specific alternatives (such as [husky](https://github.com/typicode/husky) for Javascript) that may make a better choice for your specific project.  However, [pre-commit](https://pre-commit.com/) may still be viable option in many cases if a Python interpreter is available.

Currently, CICD-Tools is optimized to use [pre-commit](https://pre-commit.com/), but in the future other solutions may be supported.

If you have an existing project that you'd like to add CICD-Tools to, the [install-light.sh](../../scripts/install-light.sh) script can automate a great deal of the process, but there will be manual changes required as well.

Please read the documentation below to identify all the requirements.

## Adding CICD-Tools to an Existing Project

### Step 1. Ensure your Project Contains a `pyproject.toml` File

[Poetry](https://python-poetry.org/) keeps all it's configuration in this file, and if you are managing conventional commits with [commitizen](https://pypi.org/project/commitizen/) you can bundle its configuration here as well.  This allows you to consolidate both Python package management and tool configuration in one file.

If you don't have an existing `pyproject.toml` it's fairly easy to setup:

```bash
$ poetry init -q --dev-dependency=commitizen --dev-dependency=pre-commit
```

Depending on which CICD-Tools integrations you end up using, you may find it useful to explore formatting your `pyproject.toml` file with [tomll](https://github.com/pelletier/go-toml) and including some more advanced [commitizen](https://pypi.org/project/commitizen/) configuration.  You can find examples of this in the [installer.sh](../../scripts/libraries/installer.sh) library file.

Alternatively, the [install-light.sh](../../scripts/install-light.sh) setup script will automate this process for you giving you sensible, usable defaults.

### Step 2. Add the CICD-Tools Bootstrap Layer

In order to integrate with CICD-Tools, a minimal amount of configuration is required.

#### Step 2a. Add the CICD-Tools Configuration Files

Your project should contain the [.cicd-tools](../../.cicd-tools) folder at the root level.  This is where global configuration is kept for CICD-Tools, and it's also where [Toolboxes](../../cicd-tools/boxes) are installed ephemerally during CI/CD.

The `configuration` sub-folder needs to be populated with the [CICD-Tools configuration files](../../.cicd-tools/configuration) to facilitate and customize global CI tasks such as how Toolboxes are installed and how changelogs are generated.

The [install-light.sh](../../scripts/install-light.sh) script will perform this installation for you.

#### Step 2b. `.gitignore` Changes

Once you've copied the above content, you should also add a line to your [.gitignore](../../.gitignore) file:

```.gitignore
.cicd-tools/boxes/*
```

The [install-light.sh](../../scripts/install-light.sh) script will create this file if it doesn't exist or add this line if it does.

### Step 3. Pre-Commit Hooks

To make full use of CICD-Tools, you'll need to define some [pre-commit](https://pre-commit.com/) hooks.  These hooks are used both for local development, and by the CI itself.

Take a look at this example [.pre-commit-config.yaml](../../{{cookiecutter.project_slug}}/.pre-commit-config.yaml) file to get up and running quickly.

If you have no [.pre-commit-config.yaml](../../{{cookiecutter.project_slug}}/.pre-commit-config.yaml) for your project the [install-light.sh](../../scripts/install-light.sh) script will create a basic one for you.

Also keep in mind that each of the tools that you add may have their own configuration requirements.