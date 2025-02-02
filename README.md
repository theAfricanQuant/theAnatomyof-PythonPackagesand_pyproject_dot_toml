## Python Modules and Packages

### Modules

- **Definition:** A module is any Python file (with a `.py` extension) that can contain functions, classes, and variables.
- **Usage:** Simply create a Python file (e.g., `simple.py`), add your code, and then import it into other scripts as needed.

### Packages

- **Definition:** A package is a directory that organizes multiple Python modules (and possibly subpackages) into a structured hierarchy. For the directory to be recognized as a package, it must contain an `__init__.py` file. This file can be empty or include initialization code.
- **Usage:** Packages allow you to group related modules together and prevent naming conflicts by using separate namespaces.
  
### Differences and Similarities

- **Modules vs. Packages:**
  - A **module** is a single file, whereas a **package** is a directory that can contain multiple modules and even subpackages.
- **Common Purpose:** Both modules and packages are used to organize code, making it reusable and easier to manage.
- **Importing:** With an `__init__.py` file, you can import a package just like a module:
  - Import a specific module:  
    ```python
    from utils.add import add_numbers
    ```
  - Or, if functionality is exposed in the package’s `__init__.py`:  
    ```python
    from utils import subtract_numbers
    ```

### Why Use Modules and Packages?

- **Code Reusability:** Easily share and reuse code across different projects.
- **Organization:** Break your project into logical units for improved clarity and maintainability.
- **Namespace Management:** Avoid naming conflicts by encapsulating modules within packages.

---

## Virtual Environments

### Overview

- **System Python vs. Project Python:**
  - **System Python:** The globally installed Python interpreter on your system.
  - **Project Python:** A virtual environment creates an isolated Python interpreter for your project. It’s based on the system Python but does not inherit its packages.

### Benefits

- **Isolation:** Keeps your project’s dependencies separate from the system and other projects.
- **Clean Environment:** Simplifies tracking and managing project-specific packages.
- **Reproducibility:** Ensures your project behaves consistently across different setups.

### Creating a Virtual Environment with **uv**

**uv** is a modern, unified tool for Python packaging that not only manages virtual environments but also handles dependency management and project packaging. Here’s why **uv** stands out:
- **Unified Workflow:** Combines environment creation, dependency management, and packaging into a single tool.
- **Lightweight and Fast:** Offers simplicity and efficiency over tools like Pipenv and Poetry, reducing complexity in your development workflow.

**Steps to Create a Virtual Environment with uv:**

1. **Install uv:** If you haven’t installed **uv** yet, follow the installation instructions provided on its website.
2. **Navigate to Your Project Directory:** Open your terminal and move to your project’s root directory.
3. **Run the Command:**
   ```bash
   uv init . # to initialize the project
   uv add <some package names> # to add packages. please specify eg pandas numpy etc
   ```
   This command creates a new virtual environment dedicated to your project.

4. **Activate the Virtual Environment:**
   - **On macOS/Linux:**
     ```bash
     source myenv/bin/activate
     ```
   - **On Windows:**
     ```bash
     myenv\Scripts\activate
     ```

Once activated, any Python command you run will use the packages installed in this isolated environment, ensuring that your project remains independent from system-wide Python settings.

---

## Managing Project Dependencies with `pyproject.toml`

- **Purpose:** The `pyproject.toml` file serves as a single source of truth for your project's configuration, including the project name, dependencies, and build settings.
- **Key Sections:**
  - **`[tool.poetry]`:** Contains metadata like the project name, version, and dependencies.
  - **`[build-system]`:** Specifies the build system requirements for your project.
- **Dependency Management:** You can add dependencies to your project by running `uv add <package-name>`. This command updates the `pyproject.toml` file with the new package information.
- **Key Features:**
  - **Project Settings:** Includes the project name and which directories are to be treated as packages.
  - **Dependencies:** Lists the required packages for your project.
  - **Build Configurations:** Provides instructions for building and distributing your project.
  - **Simplicity:** With the rise of modern package builders, `pyproject.toml` makes it easier to maintain and distribute your project.
  - **Unified Configuration:** Consolidates settings that were previously spread across multiple files (like `setup.py` and `requirements.txt`).
- **Modern Standard:** Introduced in [PEP 518](https://www.python.org/dev/peps/pep-0518/), the `pyproject.toml` file is designed to replace older methods like `requirements.txt` and `setup.py`, offering a single source of truth for project configuration.

- **Advantages:**
  
  - **PEP 518 Compliance:** It standardizes the way projects declare build system requirements.
  
By using modules, packages, and virtual environments (especially with tools like **uv**), you can maintain clean, organized, and reproducible Python projects with minimal hassle.

---

### Installing Build Packages with uv

Before you package your project, you'll need to install some build tools. These packages—`build`, `setuptools`, and `wheel`—are essential for building your package, but note that they are only needed during the build process. They are **not** required for someone who later installs and uses your package.

Using **uv**, you can install these build packages with the following command:

```bash
uv pip install build setuptools wheel
```

This command ensures that your project has everything it needs to build a distributable package (e.g., a `.whl` file or a source archive).

---

### Setting Up the pyproject.toml File

When you run `uv init`, a `pyproject.toml` file is automatically created for your project. This file serves as a single source of truth for your project's build configuration and metadata. You should update it to include the following content:

```toml
[build-system]
requires = ["setuptools>=42", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "project1"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11.3"
dependencies = [
    "ipykernel>=6.29.5",
]

[tool.setuptools]
packages = ["utils"]
```

- **[build-system]:** Specifies that your project requires at least version 42 of `setuptools` along with `wheel`, and it tells the build system to use `setuptools.build_meta` as the backend.
- **[project]:** Contains metadata about your project such as its name, version, description, and dependencies. This section replaces the older `setup.py` and `requirements.txt` approaches.
- **[tool.setuptools]:** Lists the packages (directories with an `__init__.py` file) that should be included in the build. In this case, it includes the `"utils"` package.

---

### Building and Sharing the Package

Once your `pyproject.toml` file is properly configured, you can build your package using the following command:

```bash
python -m build
```

This command reads your `pyproject.toml` file and generates the build artifacts, typically placing a `.whl` file (for installation via `pip`) and a `.tar.gz` source archive into a `dist` directory. These files can be shared with other projects or published to the Python Package Index (PyPI).

To install the built package in another project, simply use:

```bash
pip install <path-to-the-.whl-file>
```

This completes the process, showing how you can go from setting up your project with **uv** to building and sharing a Python package.

--- 

By following these steps, you ensure that your package is easy to build, share, and install, all while keeping your build tools separate from the end-user requirements.

---
_For more details and advanced usage of uv check out the [Astral: uv website](https://astral.sh/blog/uv-unified-python-packaging) for more details_.

_Check out the [YouTube video](https://youtu.be/m2EAQk4Qlew?list=TLGG2rl4SMhLZwswMjAyMjAyNQ) I used as a guide for this demo project. Note, I made adjustments using uv which was not used in the original_


