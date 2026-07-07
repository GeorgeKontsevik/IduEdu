# IduEdu

---

[![PyPi](https://badge.fury.io/py/iduedu.svg)](https://badge.fury.io/py/iduedu)
![License](https://img.shields.io/github/license/GeorgeKontsevik/IduEdu?style=flat&logo=opensourceinitiative&logoColor=white&color=blue)
[![OSA-improved](https://img.shields.io/badge/improved%20by-OSA-yellow)](https://github.com/aimclub/OSA)

Built with:

![numba](https://img.shields.io/badge/Numba-00A3E0.svg?style={0}&logo=Numba&logoColor=white)
![numpy](https://img.shields.io/badge/NumPy-013243.svg?style={0}&logo=NumPy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458.svg?style={0}&logo=pandas&logoColor=white)
![python](https://img.shields.io/badge/Python-3776AB.svg?style={0}&logo=Python&logoColor=white)
![scipy](https://img.shields.io/badge/SciPy-8CAAE6.svg?style={0}&logo=SciPy&logoColor=white)
![sphinx](https://img.shields.io/badge/Sphinx-000000.svg?style={0}&logo=Sphinx&logoColor=white)
![tqdm](https://img.shields.io/badge/tqdm-FFC107.svg?style={0}&logo=tqdm&logoColor=black)

---

## Table of Contents

- [Overview](#overview)
- [Core Features](#core-features)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Architecture](#architecture)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)
- [Citation](#citation)

---

## Overview

IduEdu is a Python library for building and analyzing transport networks for public transport, walking, and intermodal use, with support for origin–destination and accessibility matrix computation. It is aimed at developers, researchers, and data scientists working with geospatial and network analysis workflows. The package provides graph-building, graph-transformation, and matrix-computation surfaces, and examples are available in the documentation notebooks for readers who want a runnable starting point. For practical usage and setup, see the Getting Started section.

---

## Core Features

- Build public-transport graphs from OpenStreetMap/Overpass data, giving developers a ready-to-use network representation for transit analysis.
- Build walking graphs and intermodal graphs that connect transit platforms to nearby pedestrian edges, enabling multimodal routing in one network.
- Transform NetworkX graphs to and from geospatial forms, including clipping, reprojection, and graph cleaning, so networks stay usable across spatial workflows.
- Compute origin-to-destination adjacency matrices and nearest-node matches over weighted graphs, supporting accessibility and shortest-path analysis at scale.
- Expose a consolidated API for the main graph builders, transformers, and matrix utilities, making the library straightforward to import and use from one place.

---

## Installation

**Prerequisites:** requires Python >=3.11,<3.13

Install IduEdu using one of the following methods:

**Using PyPi:**

```sh
pip install iduedu
```

---

## Getting Started

### Prerequisites

- Python dependencies are installed from the project package and, for documentation builds, from `docs/requirements.txt`.
- The library code lives under `src/iduedu`, and the documented examples and API are in `docs/`.

### Quickstart

1. Install the project:

```bash
   python -m pip install -U pip
   pip install .
```

2. Install the documentation dependencies if you want to build the docs or run the examples that rely on them:

```bash
   pip install -r docs/requirements.txt
```

3. Build the Sphinx documentation locally:

```bash
   sphinx-build -E -b html docs docs/_build/html --keep-going
```

4. Import the package and start with the high-level helpers exposed from `iduedu`, such as `get_drive_graph`, `get_walk_graph`, `get_public_transport_graph`, `get_intermodal_graph`, and `join_pt_walk_graph`.

5. For matrix workflows, use `get_adj_matrix_gdf_to_gdf` or `get_closest_nodes` from `iduedu`.

---

## Architecture

IduEdu is organized as a small Python package under `src/iduedu` with a few major subsystems:

- **Public-transport graph building**: builders in `modules/graph_builders/public_transport_builders.py` and the Overpass download/parser modules fetch and convert transport data into directed graphs. The public API exposes helpers such as `get_public_transport_graph`, `get_all_public_transport_graph`, and `get_single_public_transport_graph`.
- **Walking and intermodal networks**: separate builders create walk graphs and combine them with public-transport graphs. The intermodal flow is handled by `join_pt_walk_graph`, which aligns platform nodes to nearby walk edges and composes a single network.
- **Graph transformation utilities**: `modules/graph_transformers.py` provides graph-level operations such as reprojection, clipping, converting between GeoDataFrames and NetworkX graphs, and exporting/importing GML.
- **Accessibility and OD matrix computation**: `modules/matrix/matrix_builder.py` builds shortest-path matrices and nearest-node lookups from GeoDataFrames over weighted graphs, using a CSR-backed Numba-accelerated Dijkstra implementation.
- **Shared transport metadata**: `constants/transport_specs.py` and related enums define transport registries/specifications that are used when building graphs and calculating travel times.
- **External data access**: the Overpass modules handle downloading, caching, and parsing source data before it is turned into graph structures.

The package appears to be library-first rather than service-based: users call the exported API functions from Python, then move data through the pipeline from Overpass sources to transport/walk/intermodal graphs and finally to matrix computations.

---

## API Reference

The package exposes a small public API through `iduedu._api`:

- `DEFAULT_REGISTRY`, `TransportRegistry`, `TransportSpec` — transport specification registry types and default registry.
- `get_drive_graph`, `get_walk_graph` — build drive and walk graphs.
- `get_public_transport_graph`, `get_single_public_transport_graph`, `get_all_public_transport_graph` — build public-transport graphs.
- `join_pt_walk_graph`, `get_intermodal_graph` — combine public-transport and walking graphs into an intermodal network.
- `clip_nx_graph`, `gdf_to_graph`, `graph_to_gdf`, `keep_largest_strongly_connected_component`, `read_gml`, `reproject_graph`, `write_gml` — graph transformation and I/O helpers.
- `get_closest_nodes`, `get_adj_matrix_gdf_to_gdf` — snap geometries to graph nodes and compute origin/destination matrices.
- `get_4326_boundary` — fetch an Overpass boundary in EPSG:4326.

For lower-level graph-building details, see `src/iduedu/modules/graph_builders/public_transport_builders.py` and `src/iduedu/modules/graph_builders/intermodal_builders.py`.

---

## Examples

Examples in `docs/examples` show how to use the library in practice, including runnable notebooks and workflows for building transport, walking, and intermodal graphs, as well as matrix-based analysis.

---

## Documentation

The project documentation at [https://iduclub.github.io/IduEdu/](https://iduclub.github.io/IduEdu/) includes the API reference, explanatory material, and notebook examples for graph building, intermodal workflows, and matrix computation.

---

## Contributing

- **[Report Issues](https://github.com/GeorgeKontsevik/IduEdu/issues)**: Submit bugs found or log feature requests for the project.

- **[Submit Pull Requests](https://github.com/GeorgeKontsevik/IduEdu/blob/main/CONTRIBUTING.md?raw=1)**: To learn more about making a contribution to IduEdu.

---

## License

This project is protected under the BSD 3-Clause "New" or "Revised" License. For more details, refer to the [LICENSE](https://github.com/GeorgeKontsevik/IduEdu/blob/main/LICENSE.txt?raw=1) file.

---

## Citation

If you use this software, please cite it as below.

### APA format:

    GeorgeKontsevik (2026). IduEdu repository [Computer software]. https://github.com/GeorgeKontsevik/IduEdu

### BibTeX format:

    @misc{IduEdu,

        author = {GeorgeKontsevik},

        title = {IduEdu repository},

        year = {2026},

        publisher = {github.com},

        journal = {github.com repository},

        howpublished = {\url{https://github.com/GeorgeKontsevik/IduEdu}},

        url = {https://github.com/GeorgeKontsevik/IduEdu}

    }
