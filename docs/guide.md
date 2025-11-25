# 1. Project Packaging Structure

## 1.1. Directory Layout

```text
jingle-shells/                <-- Root Directory
├── .gitignore                <-- Ignored files (Trash code)
├── pyproject.toml            <-- MAIN CONFIGURATION FILE
├── MANIFEST.in               <-- Asset inclusion (Visa for Audio)
├── README.md                 <-- Project Description
└── jingle_shells/            <-- MAIN PACKAGE (Source Code)
    ├── __init__.py           <-- Package marker
    ├── main.py               <-- Entry point script
    └── res/                  <-- Resources folder
        └── christmas_song.mp3
```

## 1.2. Configuration: `pyproject.toml`

```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "jingle-shells"
version = "1.0.0"
description = "A singing Christmas Tree in your terminal 🎄"
readme = "README.md"
requires-python = ">=3.7"
dependencies = [
    "pygame",
]
authors = [
  {name = "EurusDevSec", email = "your.email@example.com"}
]

# --- ENTRY POINT ---
# Maps command 'jingle-shells' to function 'main' inside 'jingle_shells/main.py'
[project.scripts]
jingle-shells = "jingle_shells.main:main"

[tool.setuptools.package-data]
# Instructs setuptools to include .mp3 files inside the package
jingle_shells = ["res/*.mp3"]
```

## 1.3. Asset Inclusion: `MANIFEST.in`

Ensures non-code files (like audio) are packaged.

```text
include jingle_shells/res/*.mp3
```

## 1.4. Clean up: `.gitignore`

Prevents uploading garbage/secrets to GitHub.

```gitignore
__pycache__/   # Compiled bytecode
*.pyc
.env           # Secrets/Env variables
.venv/         # Virtual Environment
dist/          # Distribution artifacts
build/         # Build artifacts
*.egg-info/    # Metadata
```

## 1.5. Deployment to GitHub

```bash
git init
git add .
git commit -m "Initial Project Structure"
git branch -M main
git remote add origin [https://github.com/Your_github_name/Your_repo_name](https://github.com/Your_github_name/Your_repo_name)
git push -u origin main
```

---

# 2\. 📦 PyPI Packaging & Publishing Guide

This guide outlines the modern workflow to publish a Python project to PyPI (2025 Standard).

## Prerequisites

1.  **PyPI Account:** Register at [pypi.org](https://pypi.org/).

2.  **Install Build Tools:**

    ```bash
    python -m pip install build twine
    ```

3.  **Setup Authentication (One-time Setup):**
    Create a `.pypirc` file to store your API Token safely.

    - Open Terminal at User folder (e.g., `C:\Users\YOUR_NAME`).
    - Run: `notepad .pypirc` (Click Yes to create).
    - Paste the following content:

    <!-- end list -->

    ```ini
    [distutils]
    index-servers =
        pypi

    [pypi]
      repository = [https://upload.pypi.org/legacy/](https://upload.pypi.org/legacy/)
      username = __token__
      password = pypi-AgEI... <--- PASTE YOUR API TOKEN HERE
    ```

---

## Workflow: Update & Re-publish 🚀

**Important:** You cannot overwrite an existing version on PyPI. You **MUST** increment the version number for every update.

### Step 1: Modify Code

Make changes, fix bugs, or add features.

### Step 2: Bump Version

Open `pyproject.toml` and increment `version`.

```toml
[project]
version = "0.1.1"  # Changed from 0.1.0
```

### Step 3: Clean Old Artifacts

Remove the `dist/` folder to ensure a clean build.

- **Windows:** `rmdir /s /q dist`
- **Linux/Mac:** `rm -rf dist/`

### Step 4: Build the Package

Generate distribution files.

```bash
python -m build
```

- `tar.gz`: Source Archive (Raw code).
- `.whl`: Built Distribution (Ready-to-install).

### Step 5: Upload to PyPI

Use `twine` to upload. If you configured `.pypirc`, it won't ask for a password.

```bash
python -m twine upload dist/*
```

---

## Troubleshooting

### 1\. `HTTPError: 400 Client Error: File already exists`

- **Cause:** You forgot to change the version number.
- **Fix:** Go back to **Step 2** and increment version (e.g., `0.1.1` -\> `0.1.2`).

### 2\. `Fatal error in launcher` (Local Machine)

- **Cause:** Broken pip path in Windows.
- **Fix:** Use `python -m` prefix:
  ```bash
  python -m pip install jingle-shells --upgrade
  ```

---

### Cheatsheet (Quick Workflow)

```bash
# 1. Code & Bump Version
# 2. Build
python -m build

# 3. Upload
python -m twine upload dist/*
```
