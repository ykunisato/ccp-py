# Python Development Docker Image

This repository contains a lightweight Python development image for Orca or VSCode remote/container workflows. It includes Jupyter Notebook/Lab and the scientific Python packages required for numerical computing, machine learning, Bayesian modeling, active inference, and network analysis.

Maintainer is Yoshihiko Kunisato (ykunisato@psy.senshu-u.ac.jp).

Keywords: VSCode, Orca, Docker, Python, Jupyter

## Usage

1. Install ["Docker Desktop"](https://www.docker.com/products/docker-desktop).

2. Build the image in this repository.

```bash
docker build -t ccp-py:latest .
```

3. Start a container and mount your working directory.

```bash
docker run --rm -it -p 8888:8888 -v "$(pwd):/workspace" --name ccp-py ccp-py:latest
```

4. Open the printed Jupyter URL in your browser, or attach VSCode/Orca to the running container.
