---
title: "Guided Tutorial: data sweep and download across the ESGF archive using our CLI tool precog-esgf-intake"
date: 2026-11-11
author: Leo Bertini
author_profile: true
categories:
  - blog
tags:
  - ESGF Datasets
  - Coding and Data Management
header:
  overlay_image: /assets/images/Globe_Grid.png
  overlay_filter: "0.5"
  overlay_color: "#103280"

layout: single
author_profile: true
read_time: false
comments: false
share: true
related: true
tagline: "" #this stops the first paragraph/line appearing in the header!

---

The ocean quietly moves carbon from the sunlit surface into its deep interior. Tiny organisms capture carbon
dissolved within seawater, build organic material, and when that material decays and sinks, it carries carbon away from
the atmosphere. Scientists
call this the biological carbon pump and, although small in terms of living carbon standing stock, the BCP is the
essential conduit through which most of the organic carbon fixed within the upper ocean get transported,
remineralised back into its inorganic forms and stored into the ocean interior. Being in the interior, this carbon is
locked away from contact with the atmosphere for centuries to millenia. Understanding how the BCP varies across
different Earth System Models is essential for understanding Earth’s climate.

In this post, we’ll follow a
beginner-friendly [Jupyter notebook workflow](https://github.com/precog-ocean/precog-esgf-intake/blob/main/notebooks/Workflow_Example_POCflux.ipynb)
that uses a Python Command-Line-Interface software we developed called ```precod-esgf-intake``` to search for, validate,
and download Earth System Model outputs for Particulate Organic Carbon Export (POC export). If you're curious about the 
source code [you can go here](https://github.com/precog-ocean/precog-esgf-intake). But you do not need to be a
coding savage to follow along: the key idea is that coding lets us turn a huge, scattered scientific data catalogue into a manageable, reproducible research workflow.

## What beginners can learn here

This is a strong introduction to real research coding because it teaches more than Python syntax.

| Skill                          | Why it matters                                                     |
|--------------------------------|--------------------------------------------------------------------|
| Using Jupyter notebooks        | Lets you combine explanation, code, and results in one place.      |
| Activating environments        | Keeps project dependencies organised and reproducible.             |
| Working with folders and paths | Helps you manage real research data responsibly.                   |
| Searching catalogues           | Turns a vast archive into a targeted dataset.                      |
| Validating results             | Prevents incomplete or inconsistent data from misleading analysis. |
| Downloading selectively        | Saves time, storage, and computing resources.                      |
| Documenting a workflow         | Makes your science easier to understand, repeat, and share.        |

## Takeaways

Coding in science is not about memorising commands. It is about building a reliable path from a scientific question to
evidence you can trust.
Here, the question is ambitious: where, and how efficiently, does the ocean export carbon into its interior? The
notebook’s answer begins modestly—with an environment, a folder, a search, and a download—but those small steps are the
foundation of large-scale climate research.
If you are new to coding, try not to worry about understanding every line immediately. Follow the workflow, inspect the
outputs, break something safely in a copy of the notebook, and ask what each command is doing. That curiosity is exactly
how computational scientists are made.

**So let's get started**

## What are we looking for?

The notebook searches for Earth System Grid Federation (ESGF) catalogues for two CMIP6 variables:

```expc```: export production of organic carbon (a fully resolved grid Lat,Lon,Depth)

```epc100```: export production at 100 m depth (essentially a 2D slice of expc where Depth is fixed to 100 m)

Together, these variables help researchers investigate how much carbon is produced by marine life and exported downward
through the ocean. POC flux—the sinking flux of particulate organic carbon—is a central part of ocean biogeochemical
cycling and the biological pump.
Think of it like this: phytoplankton near the surface are the ocean’s tiny solar-powered factories. Some of the carbon
they fix into their soft tissue becomes particles that sink. The deeper those particles travel before being decomposed 
into dissolved inorganic carbon (DIC), the longer this carbon (either in POC or remineralised DIC forms) can 
remain locked away from contact with the atmosphere.

**Why use code for this?**
Climate-model archives contain enormous amounts of data: different models, experiments, time periods, variables, grids,
and file formats. Searching manually would be slow, inconsistent, and easy to get wrong.
The notebook uses a set of Python scripts from the precog-data-intake repository to automate the process. It walks
through running those scripts inside Jupyter, following the prompts as though you were using a terminal.

**The workflow has four main jobs:**

1. Set up a Python environment.

2. Create a folder for results.

3. Search the ESGF catalogue.

4. Download the matching data.

That may sound technical, but each step is really just teaching the computer to do something repetitive on your behalf

## Step 1: Prepare your Python workspace

Before analysing data, the notebook activates a virtual environment:

```bash
cd ..
source .venv/bin/activate
```

A virtual environment is like a self-contained toolbox for a project. It keeps the packages needed for this workflow
separate from other Python projects on your computer, reducing the risk that one project’s software breaks another’s.

If you are new to coding, this is one of the most useful habits you can learn early. It makes your work more
reproducible: someone else—or future you—can recreate the same software setup more easily.

## Step 2: Make a home for your results

Next, the notebook creates a folder to store the search results:

```bash
mkdir -p ~/Desktop/test_search
```

This creates a directory called test_search on the Desktop. The -p option means “create the folder if it does not
already exist, and do not complain if it already does.”

It sounds trivial, but organised folders matter. In real research, a clear directory structure helps you distinguish raw
search results, downloaded model files, processed data, figures, and notes. Good data habits begin before any exciting
plot appears.

## Step 3: Search the climate-data catalogue

The main search step runs:

```bash
python intake_CatalogueSearch.py
```

When prompted, the notebook asks you to provide the variables of interest:

```bash
['expc', 'epc100']
```

The script then sweeps ESGF catalogue records for matching Earth-system model outputs. Rather than asking you to inspect
thousands of entries by hand, it records what is available and checks important details such as grid consistency and
whether the time coverage is continuous. The notebook explains that the search produces two useful outputs:

- An Excel spreadsheet containing the raw search results from ESGF nodes. 
- A log file recording the search, grid-consistency tests, and time-stamp continuity checks.
This is a great example of scientific coding: the computer does the tedious searching, while the researcher retains a
transparent record of what was found and how it was checked.

**What does “valid data” mean?**

A file existing online does not automatically mean it is usable for your analysis. The workflow checks whether models
provide complete sets of the requested variables across the relevant experiments and grids.

For example, the notebook notes that one model may have expc available on its native grid, gn, but lack the
corresponding Historical-run output. In other words, a partial dataset may be interesting, but it may not support a
clean comparison between pre-industrial and historical conditions.

The notebook identifies models with complete pre-industrial and Historical runs for both ```epc100``` and ```expc```,
including models such as ```MPI-ESM1-2-HR```, ```MPI-ESM1-2-LR```, ```UKESM1-0-LL```, ```GFDL-ESM4```,
```GISS-E2-1-G```, ```IPSL-CM6A-LR```, and ```MPI-ESM-1-2-HAM```.

For a newcomer, this is an important lesson: data quality control is part of science. Before plotting or interpreting
anything, you need to know whether the data are complete, consistent, and appropriate for your question.

## Step 4: Download only what you need

Once the search has identified suitable files, the notebook moves to downloading:

```bash 
python scripts/intakeOceanVarsDL.py
```

The downloads run in parallel and are organised beneath a CMIP6 directory, with separate folders for each model.
Parallel downloading means more than one file can be fetched at once rather than one after another. That can save a 
lot of time—but it also highlights why the earlier checks matter. Downloading hundreds of gigabytes of unsuitable files would
waste storage, bandwidth, and patience.

The notebook even includes a practical demonstration: it trims a dataframe for UKESM1-0-LL to its first five rows,
because otherwise the next step could prompt a download of roughly 200 GB. That is a very beginner-friendly reminder: test your workflow on a small subset first. Once you know it works, scale it
up.

At this point, the workflow has done something genuinely powerful. It has:

- Searched a global climate-data catalogue.
- Filtered results by variable, experiment, model, and grid.
- Checked consistency and time continuity.
- Organised and downloaded a manageable subset of data.

For students starting out, the exciting part is not merely downloading a file. It is realising that a few well-designed
scripts can turn an overwhelming archive into a question you can actually investigate.

## Next steps

The next stage is scientific interpretation: examining POC-related export fluxes, comparing models, and asking what
the patterns reveal about the ocean carbon cycle. POC flux attenuation at depth is influenced by processes involving 
physical transport and microbial remineralisation for example. Microbial activity on particles is in turn 
influenced by the type of particles (their lability, porosity, potential for aggregation) as well as their sinking 
speeds through the water column, the oxygen availability and temperature for instance.
All these factors influencing the fate and remineralisation of POC at depth make this a rich and active research 
area where  we still have a lot to discover in order to have models better reflect observations.

Below is a guided tutorial where I demonstrate how to fetch download data from the ESGF catalogue and 
how to visualise the global mean POC flux profile with shaded interval representing one 
standard deviation from the mean.

## Youtube Tutorial


<iframe width="560" height="315" src="http://www.youtube.com/embed/RZfH7hAYqyU&t" frameborder="0"> </iframe>
