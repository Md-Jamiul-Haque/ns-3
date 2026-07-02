# Installing and Isolating Python 3.11 alongside systemwide Python 3.14

This document shows you how to install Python 3.11 as an alternate version alongside your existing system-wide Python 3.14 installation on Ubuntu. It also covers how to safely set up an isolated project environment using that specific version.

### Step 1: Update the Local Package Index
Before adding new software repositories or packages, make sure your system's package list is fully up to date.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Software Properties Common
You need the `software-properties-common` package to easily manage independent software channels like Personal Package Archives (PPAs).

```bash
sudo apt install -y software-properties-common
```

### Step 3: Add the Deadsnakes PPA Repository
Standard Ubuntu repositories usually only keep track of one or two stable Python versions per OS release. The Deadsnakes PPA is a trusted community repository that provides cleanly packaged older and newer Python versions that can run side-by-side.

Add the repository by running:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa -y
```

### Step 4: Install Python 3.11
Now you can install the Python 3.11 runtime. Make sure to include the `python3.11-venv` package explicitly, as you will need it to create virtual environments later.

```bash
sudo apt install -y python3.11 python3.11-venv python3.11-dev
```
Verify that the new interpreter is installed correctly.

```bash
python3.11 --version
```

### Step 5: Verify the Installation Paths
To confirm that both Python versions live in separate places and won't conflict with each other, check their absolute paths in your terminal.

```bash
which python3.14
which python3.11
```

Expected Output Structure:

- `/usr/bin/python3.14`
- `/usr/bin/python3.11`

### Step 6: Create a Virtual Environment Bound to Python 3.11
With Python 3.11 installed, you can now create an isolated workspace for your project.

- Navigate to or create your project directory.

  ```bash
  mkdir ~/project-path
  cd ~/project-path
  ```

- Create the virtual environment using the explicit Python 3.11 binary. We will name the environment folder `.venv`

  ```bash
  python3.11 -m venv .venv    # you can use your desired name
  ```

### Step 7: Activate and Validate the Target Environment
To start using the environment, you need to activate it. This tells your terminal to use the project's local Python binaries instead of the system ones.

```bash
source .venv/bin/activate
```

Once activated, you will see (`.venv`) appear at the beginning of your terminal prompt. You can double-check that the plain python command now points to Python 3.11

```bash
python --version
```

Expected output: `Python 3.11.x`

Any packages you install using pip while this environment is active will stay entirely inside this project folder, without affecting your global Python 3.14 setup.

### Step 8: Deactivate the Context (When Finished)
When you are done working on your project, you can leave the virtual environment and return to your system's normal global setup (Python 3.14) by running:

```bash
deactivate
```
