# Installing and Isolating Python 3.11 alongside systemwide Python 3.14

This document details the process for installing Python 3.11 as an alternate runtime alongside a pre-existing system-wide Python 3.14 installation on Ubuntu Linux, and 
how to safely provision an isolated project environment.

### Step 1: Update the Local Package Index
Before making structural modifications or adding new software repositories, ensure your Ubuntu system package index is fully current.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Software Properties Common
To safely manage independent software channels via Personal Package Archives (PPAs), the software-properties-common manager must be present on the machine.

```bash
sudo apt install -y software-properties-common
```

### Step 3: Add the Deadsnakes PPA Repository
The official Ubuntu repositories typically only track one or two stable Python versions per OS distribution release. The Deadsnakes PPA provides optimized, cleanly split packages for older and cutting-edge Python versions side-by-side.
Add the repository using the command below:

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
