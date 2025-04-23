# BRT Documentation

This repository contains the documentation for the **Binaural Rendering Toolbox (BRT)**, developed using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and hosted at:

📘 **Live documentation**: [https://grupodiana.github.io/BRT-Documentation/](https://grupodiana.github.io/BRT-Documentation/)

## What is BRT?

The **Binaural Rendering Toolbox (BRT)** is a set of software libraries, applications, and definitions aimed as a virtual laboratory for psychoacoustic experimentation. The BRT is developed in the framework of the [SONICOM project](https://www.sonicom.eu/) and includes the algorithms originally developed for the [3D Tune-In Toolkit](http://www.3d-tune-in.eu/toolkit.html), now integrated into a new open and extensible architecture.

This documentation aims to provide a structured and accessible resource for developers, researchers, and users of the BRT.

## Setting up a local development environment

To contribute or work on the documentation locally using **Visual Studio Code**, follow these steps:

### Prerequisites

- [Python 3.8+](https://www.python.org/)
- [pip](https://pip.pypa.io/)
- [Visual Studio Code](https://code.visualstudio.com/)
- Recommended: [Python extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

### Installation steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/grupodiana/BRT-Documentation.git
   cd BRT-Documentation
   ```
2. **(Optional) Create a virtual environment**
    ```bash
    python -m venv venv
    ```
    Then activate it:
    -  On macOS/Linux:
        ```bash
        source venv/bin/activate
        ```
    - On Windows:
        ```bash
        .\venv\Scripts\activate
        ```
3. Install dependencies
    ```bash
    pip install -r requirements.txt
    ```
    If the requirements.txt file is missing, you can manually install the basics:
    ```bash
    pip install mkdocs mkdocs-material
    ```
4. Open the project in Visual Studio Code
    ```bash
    code .
    ```
5. Serve the documentation locally
    ```bash
    mkdocs serve
    ```
    This will start a local web server, usually at http://127.0.0.1:8000/, where you can preview changes in real time.

## PDF generation support (optional)

This documentation site includes the functionality to automatically generate a downloadable **PDF version** of all its contents.

This is enabled using the **PDF Generate Plugin for MkDocs**  
🔗 [https://github.com/orzih/mkdocs-with-pdf](https://github.com/orzih/mkdocs-with-pdf)

> ⚠️ **Note**: The PDF file is only generated when the documentation is deployed via **GitHub Pages**. It is not built during local development by default. Therefore, installing the plugin and its dependencies locally is optional unless you want to test PDF generation on your machine. This behavior is currently based on preliminary testing and may change.

### How to install the plugin locally (optional)

1. **Install the MkDocs plugin**
   ```bash
   pip install mkdocs-with-pdf
    ```
2. **Install WeasyPrint**

   At the time of writing, version **65.1** is recommended.  
   Follow the official guide: [WeasyPrint Installation Docs](https://doc.courtbouillon.org/weasyprint/stable/first_steps.html#installation)

   On Windows:
    - Install **MSYS2**, using the default options  
     🔗 [https://www.msys2.org/#installation](https://www.msys2.org/#installation)

   - Open the MSYS2 shell and install the required dependency:
     ```bash
     pacman -S mingw-w64-x86_64-pango
     ```
   - Then, install WeasyPrint via pip:
     ```bash
     pip install weasyprint
     ```
     
3. **Build the PDF manually (optional)**
    To simulate GitHub Actions locally and build the PDF:
    ```bash
    $env:GITHUB_ACTIONS = 1; mkdocs build
    ```
