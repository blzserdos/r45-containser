# r45-container

Apptainer/Singularity `.sif` container running **R 4.5.0** with the following packages pre-installed:

| Package | Purpose |
|---|---|
| `tidyverse` | ggplot2, dplyr, tidyr, readr, purrr, stringr, … |
| `data.table` | Fast data manipulation |
| `mirt` | Multidimensional item response theory |

All package dependencies are included.

---

## How it works

Every push to `main` that touches `r45.def` or the workflow file triggers a GitHub Actions build.  
The runner installs Apptainer, calls `apptainer build`, runs a smoke-test, and uploads the finished `r45.sif` as a **GitHub Release** asset.

You can also trigger a build manually from the **Actions → Build R 4.5 Apptainer container → Run workflow** button.

---

## Downloading the container

Go to the **Releases** tab of this repository, pick the latest release, and download `r45.sif`.

Or with the GitHub CLI:

```bash
gh release download --repo <your-username>/r45-container --pattern "r45.sif"
```

---

## Using the container on your HPC / TSD cluster

```bash
# Run an R script
apptainer run r45.sif my_analysis.R

# Execute a one-liner
apptainer exec r45.sif Rscript -e "library(mirt); sessionInfo()"

# Interactive R shell
apptainer exec r45.sif R

# Open a shell inside the container
apptainer shell r45.sif
```

Bind-mount your data directory if needed:

```bash
apptainer exec --bind /path/to/data:/data r45.sif Rscript /data/my_script.R
```

---

## Rebuilding / updating

Edit `r45.def` (e.g. add packages, pin a new CRAN snapshot date) and push to `main`.  
The action will produce a new release automatically.

---

## Repository structure

```
.
├── r45.def                        # Apptainer definition file
├── .github/
│   └── workflows/
│       └── build.yml              # CI/CD pipeline
└── README.md
```
