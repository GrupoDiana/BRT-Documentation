# BRT Documentation

This repository contains the documentation for the **Binaural Rendering Toolbox (BRT)**, developed using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and hosted at:

📘 **Live documentation**: https://grupodiana.github.io/BRT-Documentation/

## What is BRT?

The **Binaural Rendering Toolbox (BRT)** is a set of software libraries, applications, and definitions aimed as a virtual laboratory for psychoacoustic experimentation. The BRT is developed in the framework of the [SONICOM project](https://www.sonicom.eu/) and includes the algorithms originally developed for the [3D Tune-In Toolkit](http://www.3d-tune-in.eu/toolkit.html), now integrated into a new open and extensible architecture.

This documentation aims to provide a structured and accessible resource for developers, researchers, and users of the BRT.

## Setting up a local development environment

To contribute to or work on the documentation locally, it is recommended to use **Visual Studio Code** together with a Python virtual environment.

### Prerequisites

* [Python 3.8+](https://www.python.org/)
* [pip](https://pip.pypa.io/)
* [Visual Studio Code](https://code.visualstudio.com/)
* Recommended: [Python extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

### Installation steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/grupodiana/BRT-Documentation.git
   cd BRT-Documentation
   ```

2. **Create a virtual environment**

   It is recommended to use a virtual environment to keep the project's Python dependencies isolated from the system Python installation.

   ```bash
   python -m venv venv
   ```

3. **Activate the virtual environment**

   **macOS/Linux:**

   ```bash
   source venv/bin/activate
   ```

   **Windows PowerShell:**

   ```powershell
   .\venv\Scripts\Activate.ps1
   ```

   If PowerShell prevents the activation script from running, allow locally created scripts with:

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

   Then activate the environment again:

   ```powershell
   .\venv\Scripts\Activate.ps1
   ```

   When the virtual environment is active, `(venv)` should appear at the beginning of the terminal prompt.

4. **Install dependencies**

   If `requirements.txt` is available, install the project dependencies with:

   ```bash
   pip install -r requirements.txt
   ```

   If `requirements.txt` is not available, you can manually install the main MkDocs dependencies:

   ```bash
   pip install mkdocs mkdocs-material
   ```

5. **Open the project in Visual Studio Code**

   ```bash
   code .
   ```

6. **Serve the documentation locally**

   ```bash
   mkdocs serve
   ```

   This will start a local web server, usually at:

   ```text
   http://127.0.0.1:8000/
   ```

   You can then preview changes in real time while editing the documentation.

## PDF generation support 

This documentation site includes support for generating a downloadable **PDF version** of its contents using the [MkDocs PDF Generate Plugin](https://github.com/orzih/mkdocs-with-pdf).

PDF generation is **disabled during normal local development** and is enabled only when the `ENABLE_PDF_EXPORT` environment variable is set.

> ⚠️ **Note:** Although PDF generation is optional during normal development, the `mkdocs-with-pdf` plugin is declared in `mkdocs.yml` and therefore must be installed for the current MkDocs configuration to load correctly.

### How to install the plugin locally

1. **Install the MkDocs plugin**

   Make sure the virtual environment is activated and run:

   ```bash
   pip install mkdocs-with-pdf
   ```

2. **Install WeasyPrint**

   The PDF plugin uses [WeasyPrint](https://weasyprint.org/) to generate the PDF.

   Follow the official [WeasyPrint installation documentation](https://doc.courtbouillon.org/weasyprint/stable/first_steps.html#installation).

   **On Windows:**

   * Install [MSYS2](https://www.msys2.org/).

   * Open the **MSYS2 UCRT64** terminal and install the required dependencies:

     ```bash
     pacman -S mingw-w64-ucrt-x86_64-pango
     ```

   * Then, from the project's activated virtual environment, install WeasyPrint:

     ```bash
     pip install weasyprint
     ```

### Windows troubleshooting: `libgobject-2.0-0`

If `mkdocs serve` or WeasyPrint produces an error similar to:

```text
OSError: cannot load library 'libgobject-2.0-0'
```

Windows is probably unable to find the native libraries installed by MSYS2.

Add the MSYS2 UCRT64 binaries to the current PowerShell session:

```powershell
$env:PATH = "C:\msys64\ucrt64\bin;$env:PATH"
```

Then verify that WeasyPrint works using the Python interpreter from the project's virtual environment:

```powershell
.\venv\Scripts\python.exe -c "from weasyprint import HTML; print('WeasyPrint OK')"
```

If the command prints:

```text
WeasyPrint OK
```

WeasyPrint is correctly configured.

If `python` points to the MSYS2 Python instead of the project's virtual environment, check which Python is being used:

```powershell
where.exe python
```

If MSYS2 appears before the project's virtual environment, use the virtual environment explicitly:

```powershell
.\venv\Scripts\python.exe
```

Alternatively, you can temporarily put the virtual environment first while keeping the MSYS2 libraries available:

```powershell
$env:PATH = "$PWD\venv\Scripts;C:\msys64\ucrt64\bin;$env:PATH"
```

Then verify again:

```powershell
where.exe python
```

> **Important:** MSYS2 provides the native libraries required by WeasyPrint. It should not replace the Python interpreter from the project's virtual environment.

### Build the PDF manually

To enable PDF generation locally, set the `ENABLE_PDF_EXPORT` environment variable before running MkDocs.

In PowerShell:

```powershell
$env:ENABLE_PDF_EXPORT = "1"
mkdocs build
```

For normal documentation development, simply run:

```powershell
mkdocs serve
```

without setting `ENABLE_PDF_EXPORT`.
