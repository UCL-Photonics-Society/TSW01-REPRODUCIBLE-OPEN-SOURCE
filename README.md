
# [Transferable Skills Workshops] - Good Practices for Reproducible Open Source Code



---

## Why this Workshop?

Open-source software is more than just sharing code; it’s about fostering **collaboration**, **transparency**, and **reproducibility**. Whether in research, industry, or education, writing clear and reproducible code is a critical skill. Adopting good practices ensures your code is:

- Easier to read and understand.
- Reproducible across machines and environments.

This leads to higher-quality work and fosters collaboration, allowing others to confidently build upon your contributions.

---

## Learning Objectives

- Understand the importance of good practices for reproducibility and collaboration.
- Apply dependency management techniques using tools like Poetry.
- Restructure repositories using best practices.
- Create and integrate a LICENSE file for open-source compliance.
- Develop and execute unit tests to ensure code reliability.
  
---

## Table of Contents
- [Prerequisites](#prerequisites)
- [Lab Instructions](#lab-instructions)
   - [How Does the Lab Work?](#how-does-the-lab-work)
   - [Tasks](#tasks)
      - [0️⃣ - Initialize the Lab](#0️⃣---initialize-the-lab)
      - [1️⃣ - Clone the Repository Locally](#1️⃣---clone-the-repository-locally)
      - [2️⃣ - Setup a Virtual Environment and Manage Dependencies with Poetry](#2️⃣---setup-a-virtual-environment-and-manage-dependencies-with-poetry)
      - [3️⃣ - Restructure the Repository](#3️⃣---restructure-the-repository)
      - [4️⃣ - Linting and Formatting](#4️⃣---linting-and-formatting)
      - [5️⃣ - Add a LICENSE File](#5️⃣---add-a-license-file)
      - [6️⃣ - Write Tests](#6️⃣---write-tests)
      - [7️⃣ - Check Completion and Submit](#7️⃣---check-completion-and-submit)
- [Contributions](#contributions)
   - [Contributors](#contributors)
   - [Maintainers](#maintainers)
- [Licence](#licence)


---

## Prerequisites

> [!IMPORTANT]
> Please ensure you've completed the checklist in [TSW00-LABS-PREREQUISITES](https://github.com/UCL-Photonics-Society/TSW00-LABS-PREREQUISITES).

---

## Lab Instructions

### **How Does the Lab Work?**

This repository contains Python files to generate optical signals and study dispersion effects during fiber propagation. While functional (`python main.py` runs the code), it does not follow good open-source practices.

In this lab, you will **restructure** the project step-by-step to make it compliant with best practices, ensuring **readability** and **reproducibility**.

> [!TIP] 
> Use the autograding script in the open pull request `main -> feedback` (created in Task #0) to track your progress.

---

### **Tasks**

#### 0️⃣ - Initialize the Lab

> [!NOTE]  
> If this is part of a GitHub Classroom assignment, skip to Task #1.

1. **Fork** this repository using the button at the top.  
2. Create a new branch: `main -> view all branches -> new branch` and name it `feedback`.  
3. Create a pull request in your fork: branch `main` -> branch `feedback`. Leave the PR open.

---

#### 1️⃣ - Clone the Repository Locally

1. On your fork (or Classroom assignment), click `Code -> Copy URL to Clipboard`.  
2. Open a terminal and navigate to the desired folder.  
3. Clone the repository using:  
   ```bash
   git clone <repository_url>
   ```

---

#### 2️⃣ - Setup a Virtual Environment and Manage Dependencies with Poetry

Currently, the project lacks reproducibility. Let’s fix that with **Poetry**:

1. Install Poetry:  
   ```bash
   pip install poetry
   ```
2. Navigate to the repository root and check the contents:  
   ```bash
   ls
   ```
3. Initialize Poetry:  
   ```bash
   poetry init
   ```
  > [!CAUTION]
  > On some versions of git bash, running `poetry init` might result in a `command not found error`. If this is the case, run:
  > ```bash
  >  python -m poetry init
  >  ```
  >  If you are in this situation, you will need to replace `poetry` with `python -m poetry` in every command for the rest of the workshop. 

   Follow the prompts:  
   - **Package name**: `opticaldisp`  
   - **Version**: `0.0.1`  
   - **Description**: Add a short description.  
   - **Author/License**: Press Enter to skip.  
   - **Python versions**: Enter `^3.<your_version>` (at least 3.10). 
   - For dependencies: Enter `no` for both.
  


> [!TIP]
> A `pyproject.toml` file is now created with your project information.

4. Add dependencies by examining the code (e.g., `numpy`, `matplotlib`):  
   ```bash
   poetry add <package_name>
   ```
5. Verify everything works:  
   ```bash
   poetry run python main.py
   ```

> [!TIP]
> You should now see a `poetry.lock` file and generated figures.

6. Commit and push changes:  
   ```bash
   git status  
   git add .  
   git commit -m "chore: setup Poetry and dependencies"  
   git push
   ```

---

#### 3️⃣ - Restructure the Repository

Make the repository modular, following best practices:

1. **Create new files**:  
   - `waveforms.py`: Move waveform-generating functions here.  
   - `dispersion.py`: Move `apply_dispersion` function here.  
   - `optical_signals.py`: Move `OpticalSignal` class here.

2. **Organize the files**:  
   - Group all function/class files under `src/opticaldisp/`.

3. **Update imports**:  
   - Add necessary imports to each new file.  
   - In `main.py`, imports all functions needed. Example:
     ```python
      from src.opticaldisp.waveforms import generate_gaussian
     ```

4. Test the new structure:  
   ```bash
   poetry run python main.py
   ```

> [!TIP] 
> At this stage, the repository structure should look like this:  

```plaintext
├── tsw01
│   ├── src
│   │   ├── opticaldisp
│   │   │   ├── waveforms.py
│   │   │   ├── optical_signals.py
│   │   │   ├── dispersion.py
│   ├── .github
│   │   ├── workflows
│   │   │   ├── autograding.yml
│   ├── main.py
│   ├── README.md
│   ├── .gitignore
│   ├── pyproject.toml
│   ├── poetry.lock
```

5. Commit and push changes:  
   ```bash
   git status  
   git add .  
   git commit -m "refactor: change project structure"  
   git push
   ```

---

#### 4️⃣ - Linting and Formatting

In this task, we will ensure your codebase follows a consistent style and adheres to best practices using a linter called Ruff. Linters help identify and fix common issues in your code, making it more readable and maintainable.

1. Add Ruff to the Project as a development dependencies
   - Use Poetry to add Ruff as a development dependency:
   ```bash
   poetry add ruff --group dev
   ```

2. Add Ruff rules to the `pyproject.toml` file
   - Formatting rules dictate the style of your code, keep in mind that it is up to you to decide which to enforce for your projects. By default, Ruff doesn't have any formatting rules, so we need to select those we want to apply. 
   - Open `pyproject.toml` and add the following rules (you can find what they mean [here](https://docs.astral.sh/ruff/rules/)):
   ```toml
   [tool.ruff]
   [tool.ruff.lint]
   select = [
       # pycodestyle
       "E",
       # Pyflakes
       "F",
       # pyupgrade
       "UP",
       # flake8-bugbear
       "B",
       # flake8-simplify
       "SIM",
       # pep8-naming
       "N",
   ]
   ```

3. Check your code for linting issues by running:
   - Ruff will output a list of issues with line numbers and suggested fixes. 
   ```bash
   poetry run ruff check .
   ```

4. Fix the formatting issues:
   - Run the following command and notice that many errors were fixed automatically:
   ```bash
   poetry run ruff format
   ```
   - Look at the error log and fix all remaining issues by edditing the files manually.
   


5. Commit Your Changes
   - Once running ruff format stops returning errors because all issues have been resolved, commit your changes:

   ```bash
   git add .
   git commit -m "style: apply linting fixes using Ruff"
   git push
   ```

---

#### 5️⃣ - Add a LICENSE File

You can easily add a license file directly in your repository on GitHub. Follow these steps:

1. **Open the Repository**
   - Ensure you are on the **main page** of your repository on GitHub.

2. **Add a New File**
   - Click on the **"Add file"** button at the top right of the file list.
   - From the dropdown menu, select **"Create new file"**.

3. **Name the File**
   - In the **"Name your file..."** field, type `LICENSE`.

4. **Choose a License Template**
   - At the top left of the editor, click **"Choose a license template"**.
   - Select the **MIT License** from the list.

5. **Customize the License**
   - GitHub will populate the file with the MIT License text.
   - Replace `<year>` with the current year.
   - Replace `<copyright holder>` with your name (e.g., `2024 John Doe`).

6. **Commit the LICENSE File**
   - Scroll down to the "Commit changes" section.
   - Add a commit message, such as:
     ```plaintext
     docs: add MIT License
     ```
   - Select **"Commit directly to the main branch"**.
   - Click **"Commit new file"**.
     
7. **Pull the changes to your local repository**
   - Run `git pull` to update your local repository with the LICENCE file.

---

#### 6️⃣ - Write Tests

For this task, you are going to implement some unit testing to check that you code works as intented, and allow you to detect when modifications introduce bugs and break retro compatibility.

1. Setup the testing part of your repository
   - Create a `tests` folder, and create test files, for example `tests/test_waveforms.py`.
   - Add `pytest` to the development dependency group (same command as in task4.1).

2. Create testing functions, and implements methods that will assess whether the code behaves as expected.
   - You can use the following code as inspiration, and implement as many as you want, but make sure that your assertions are correct!
   - Run the following command to check that your tests are passing:
     ```bash
     poetry run pytest tests/
     ```

> [!WARNING]  
> `pystest` expects the test files to have the convention `tests/test_<module_name>.py`, and for the functions inside the files to start with `test_`.
> If these conventions are not followed, pytest will not find your tests and return an error.

```python
import pytest
import numpy as np
from src.opticaldisp.waveforms import (
    generate_gaussian,
    generate_square,
    generate_lorentzian,
    generate_sech,
)

@pytest.fixture
def setup_waveform():
    """Fixture to create a time array and set the pulse width of the waveform."""
    t = np.linspace(-5, 5, 1000)  # Time vector
    pulsewidth = 1  # Pulse width for testing
    return t, pulsewidth

def test_generate_gaussian(setup_waveform):
    """Check that the Gaussian waveform has the expected maximum value."""
    t, pulsewidth = setup_waveform
    waveform = generate_gaussian(t, pulsewidth)
    expected_max = np.exp(0)  # Max value of Gaussian at t=0
    assert np.isclose(np.max(waveform), expected_max, atol=1e-3), f"Gaussian max: {np.max(waveform)} != {expected_max}"

def test_generate_square(setup_waveform):
    """Check that all the values of the square waveform are either 0 or 1."""
    t, pulsewidth = setup_waveform
    waveform = generate_square(t, pulsewidth)
    assert np.all((waveform == 0) | (waveform == 1)), "Square waveform contains values other than 0 or 1"

def test_generate_lorentzian(setup_waveform):
    """Check that the Lorentzian waveform has the expected maximum value."""
    t, pulsewidth = setup_waveform
    waveform = generate_lorentzian(t, pulsewidth)
    expected_max = 1 / (1 + (0 / (pulsewidth / 1.287)) ** 2)  # Max value at t=0
    assert np.isclose(np.max(waveform), expected_max, atol=1e-3), f"Lorentzian max: {np.max(waveform)} != {expected_max}"

def test_generate_sech(setup_waveform):
    """Check that the Sech waveform has the expected maximum value."""
    t, pulsewidth = setup_waveform
    waveform = generate_sech(t, pulsewidth)
    expected_max = 1 / np.cosh(0)  # Max value of Sech at t=0
    assert np.isclose(np.max(waveform), expected_max, atol=1e-3), f"Sech max: {np.max(waveform)} != {expected_max}"
   ```

> [!TIP]
> If running `poetry run pytest tests/` returns a `ModuleNotFoundError`, you can fix it by adding the following to `pyproject.toml`:
> ```yaml
> [tool.pytest.ini_options]
>  pythonpath = [
>    ".", "src",
>  ]
> ```

3. Commit and push changes:  
   ```bash
   git status  
   git add .  
   git commit -m "test: add tests for waveforms module"  
   git push
   ```
---

#### 7️⃣ - Check Completion and Submit

> [!TIP] 
> The final repository structure should look like this:  

```plaintext
├── tsw01
│   ├── src
│   │   ├── opticaldisp
│   │   │   ├── waveforms.py
│   │   │   ├── optical_signals.py
│   │   │   ├── dispersion.py
│   ├── tests
│   │   ├── tests.py
│   ├── .github
│   │   ├── workflows
│   │   │   ├── autograding.yml
│   ├── main.py
│   ├── README.md
│   ├── LICENCE
│   ├── .gitignore
│   ├── pyproject.toml
│   ├── poetry.lock
```

If you did each task correctly, all of the checks in the pull request main->feedback should be green. You can go ahead and merge the pull request.

**Congratulations on finishing this lab!** 🥳

---

## Contributions

### Contributors

- [David Gerard](https://github.com/David-GERARD) - PhD student @ UCL  
- [Mikulas Poul](https://github.com/mikicz) - Staff engineer @ Xelix  

> [!TIP]
> - Found a bug? [Open an issue](https://github.com/UCL-Photonics-Society/TSW01-REPRODUCIBLE-OPEN-SOURCE/issues).  
> - Have improvements ideas? *Fork* the repository, implement your changes, and submit a pull request with a changelog.

---

### Maintainers

For any questions, reach out to [David Gerard](https://github.com/David-GERARD) at david.gerard.23@ucl.ac.uk.

---

## Licence

MIT License

Copyright (c) 2024 David Gerard

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
