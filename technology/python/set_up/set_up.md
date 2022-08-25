---
permalink: /python/set_up/
---

# Setting Up Your Environment

[Back to Python](/docs/python/)

## Table of Contents

- [Getting Started with Python](#getting-started-with-python)
- [Installing an IDE](#installing-an-ide)
- [Running a Program](#running-a-program)
- [Installing Packages](#installing-packages)

## Getting Started with Python

The primary programming language used by the software team is Python. To begin using Python, you first need to install it.

**Linux:**

Install Python 3 using your command line package manager.

For example, on Ubuntu, run the command:
```
# sudo apt-get install python3
```

*Make sure you specify that it is Python 3. The default Python package is often Python 2*

To check that you have installed it correctly, run:
```
python3 --version
```

**Windows:**

- Navigate to [https://www.python.org/downloads/](https://www.python.org/downloads/) and download the latest version.
- Once downloaded, follow the prompts in the installer to install Python 3. *Make sure you have 'pip' checked under Optional Features. Make sure you have 'Add Python to environment variables' checked under Advanced Options.*

To check that you have installed it correctly, run the following command in Command Prompt:
```
python --version
```

## Running a Program

You can run a program (.py file) from the command line.

**Linux:**

```
python3 <file>
```

**Windows:**

```
python <file>
```

## Installing an IDE

An IDE (Integrated Development Environment) makes it easy to edit and run you code from one convinient program.

The most popular IDE for Python is [Visual Studio Code](https://code.visualstudio.com/download), which has packages that make working with Python simple. Install VS Code and run it. Navigate to the extension tab and install the Python package. It should automatically find your Python installation and use it for running your programs directly from VS Code.

You can choose to use a different IDE as well, such as Spyder. Find one that you like! There are tons out there!

## Installing Packages

pip is Python's package manager. This allows you to install additional libraries to use in Python. If you need to install an additional library, it can be done by running the following command:

```
pip3 install <package>
```
