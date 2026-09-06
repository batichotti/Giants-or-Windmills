# Giants-or-Windmills [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21366286.svg)](https://doi.org/10.5281/zenodo.21366286)

# 📊 Analysis of Top GitHub Projects Across Popular Programming Languages

This repository offers a comprehensive analysis of the **30 most-starred GitHub projects** across the **15 most popular programming languages**, as ranked by [GitHut 2.0](https://madnight.github.io/githut/#/pull_requests/2024/1). The study delves into the lifecycle of colossal files within these projects, examining their origins, growth trajectories, and maintenance strategies, with a particular focus on their evolution and impact on overall project dynamics.

## 📄 Accepted Paper

The accepted paper associated with this artifact is available here: [PDF do artigo](./Giants_or_Windmills.pdf).

## 📁 Artifact Organization

This repository is organized into three main parts:

- **README.md**: overview of the artifact, requirements, and usage (this file).
- **LICENSE**: license terms for the artifact.
- **src/**: all analysis scripts.

Inside **src/**, the code is split into **41 numbered folders, from `0` to `40`**. These represent the pipeline stages of the analysis and **must be executed in numerical order**, since a script in folder `N` may depend on outputs produced by scripts in folders with a smaller number than `N` (not necessarily `N-1`).

Each numbered folder follows the same internal layout:

```
src/
├── _00/
│   ├── input/      # data consumed by this stage (may include outputs from earlier stages)
│   ├── output/      # data produced by this stage, consumed by later stages
│   └── <script>.py  # the stage's code
├── _01/
│   ├── input/
│   ├── output/
│   └── <script>.py
...
└── _40/
    ├── input/
    ├── output/
    └── <script>.py
```

Running the folders in order (`_00`, `_01`, `_02`, ..., `_40`) reproduces the full analysis pipeline, from raw repository metadata extraction to the final figures and tables reported in the paper.

---

## 🚀 Project Goals

The objectives of this project are to:

- **Examine repository metadata** to identify patterns in highly starred projects.
- **Analyze the lifecycle of large files**, including their creation, modifications, and role within the project.
- **Evaluate contributor dynamics**, focusing on how key maintainers influence project development.
- **Identify trends in codebase size and complexity** over time.

---

## Requirements

- `requirements.txt`

### Software environment

- **Python 3.10+**
- A Unix-like environment: Linux, macOS, or WSL on Windows (scripts rely on Unix shell utilities and are not tested on native Windows)
- [Git](https://git-scm.com/) (used by PyDriller to clone and traverse repository histories)
- [CLoC](https://github.com/AlDanial/cloc) installed and available on `PATH`
- Internet access, since the pipeline clones the 450 target repositories (30 projects × 15 languages) and retrieves metadata from the GitHub API

### Hardware / storage

- **Disk space:** at least **1 TB free** is recommended. The pipeline clones the full commit history of 450 repositories, several of which are large (some individual repositories can exceed several GB), and intermediate `output/` folders across the 41 stages also consume disk space.
- **RAM:** at least **8 GB** recommended; some stages that process full commit histories with PyDriller/Pandas may benefit from **16 GB** on larger repositories.
- **CPU:** a multi-core machine is recommended to keep repository mining and CLoC analysis within a reasonable time; no GPU is required.
- **Runtime:** cloning and mining 450 repositories' full history is time-consuming; running the complete pipeline end-to-end can take from several hours to more than a month depending on network speed and hardware.

### Python dependencies

A `requirements.txt` is provided at the repository root. Install it as described in [Installation](#installation).

---

## Installation

1. Clone the repository:

	```bash
	git clone <repository_url>
	cd Giants-or-Windmills
	```

2. Create and activate a Python virtual environment:

	```bash
	python3 -m venv venv
	source venv/bin/activate
	```

3. Install Python dependencies:

	```bash
	pip install -r requirements.txt
	```

4. Install CLoC (Count Lines of Code) if not already installed. Follow the instructions on the [CLoC GitHub page](https://github.com/aldanial/cloc).

5. Run each stage in `src/` in numerical order. For example, to run the first three stages:

	```bash
	cd src/_00 && python3 *.py
	cd ../_01 && python3 *.py
	cd ../_02 && python3 *.py
	```

	(Replace `*.py` with the actual script filename inside each folder.)

### Verifying the installation

To confirm the environment is correctly set up without running the full 41-stage pipeline, run only stage `0`:

```bash
cd src/_00
python3 *.py
```

A successful run will populate `src/_00/output/` with the initial repository metadata files (e.g., a CSV listing the 450 target repositories and their metadata) and will finish without raising an exception. If `src/_00/output/` is populated and no errors were printed to the console, the installation is working correctly. Subsequent stages can then be run in order, each reading from the `output/` folders of the appropriate earlier stage(s) and writing to its own `output/`.

---

## 💾 Storage and Data Considerations

This artifact operates on data mined from **public GitHub repositories** (source code, commit history, and repository metadata such as stars and contributor counts). No private or sensitive data is collected. All repositories analyzed are publicly available open-source projects, and only metadata that is already public is used.

Because the pipeline clones and stores full commit histories for 450 repositories, users should ensure sufficient free disk space (see [Requirements](#requirements)) before running the full pipeline. Cloned repositories and intermediate outputs are not included in this artifact due to their size; the pipeline regenerates them locally when executed.

---

## 🛠️ Tools and Technologies

This analysis leverages a suite of powerful tools and libraries to extract, process, and visualize data:

- **[PyDriller](https://github.com/ishepard/pydriller)**: For extracting repository metadata and commit history.
- **[CLoC](https://github.com/AlDanial/cloc)**: For detailed analysis of code structure, including size and complexity.
- **[Pandas](https://pandas.pydata.org/)**: For robust data manipulation and statistical analysis.
- **[NumPy](https://numpy.org/)**: For numerical computations and efficient array handling.
- **[Matplotlib](https://matplotlib.org/)**: For generating insightful visualizations.
