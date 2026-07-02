# Installing and Isolating Python 3.11 alongside systemwide Python 3.14

This document shows you how to install Python 3.11 as an alternate version alongside your existing system-wide Python 3.14 installation on Ubuntu. It also covers how to safely set up an isolated project environment using that specific version.

### Step 1: Update the Local Package Index
Before adding new software repositories or packages, make sure your system's package list is fully up to date.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Software Properties Common
You need the software-properties-common package to easily manage independent software channels like Personal Package Archives (PPAs).

```bash
sudo apt install -y software-properties-common
```

### Step 3: Add the Deadsnakes PPA Repository
Standard Ubuntu repositories usually only keep track of one or two stable Python versions per OS release. The Deadsnakes PPA is a trusted community repository that provides cleanly packaged older and newer Python versions that can run side-by-side.

Add the repository by running:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa -y
```

### Step 4: Install Python 3.11 (Target Specific Version)
Next, introduce the dedicated Python 3.11 runtime environment. You must install the python3.11-venv package explicitly; otherwise, Python 3.11 will lack the architectural ability to 
encapsulate isolated development contexts.

```bash
sudo apt install -y python3.11 python3.11-venv python3.11-dev
```
Verify the presence and release status of the newly added interpreter:

```bash
python3.11 --version
```

### Step 5: Verify the Installation Paths
To confirm that both interpreters reside in separate paths without causing systemic execution overlap, query the absolute binary mapping inside your shell environment:

```bash
which python3.14
which python3.11
```

Expected Output Structure:

- `/usr/bin/python3.14`
- `/usr/bin/python3.11`

### Step 6: Create a Virtual Environment Bound to Python 3.11
Now, step completely outside your system's global landscape and establish a sandboxed container using Python 3.11 as the core driver engine.

- Navigate to or create your project's workspace directory:

```bash
mkdir ~/project-path
cd ~/project-path
```

- Provision the virtual environment by calling the version-explicit Python 3.11 module binary. In this example, the resulting directory is named `.venv`.

```bash
python3.11 -m venv .venv    # you can use your desired name
```

### Step 7: Activate and Validate the Target Environment
To force your active terminal session to redirect all python calls to the project's sandboxed binaries, perform a manual environment shell injection

```bash
source .venv/bin/activate
```

Upon execution, notice that your terminal prompt prepends (.venv) to indicate local context encapsulation. Validate that your un-versioned python terminal command resolves to Python 3.11

```bash
python --version
```

Expected output: `Python 3.11.x`
Any software packages downloaded or updated using pip inside this terminal sequence are completely walled off from the global Python 3.14 layer.

### Step 8: Deactivate the Context (When Finished)
Once your task execution cycle concludes, safely strip away the local shell references and fall back onto your primary system environment (Python 3.14 execution scope) by dropping the active hook

```bash
deactivate
```
