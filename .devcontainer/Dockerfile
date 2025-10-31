# Use a lightweight base image with Conda
FROM continuumio/miniconda3:latest

# Set working directory
WORKDIR /workspace


# Install R and system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        r-base r-base-dev \
        libcurl4-openssl-dev \
        libssl-dev \
        libxml2-dev \
        libgit2-dev \
        build-essential git && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create and activate conda environment
RUN conda create -y -n env python=3.11 \
    r-base r-essentials r-lme4 r-broom.mixed r-usdm r-irkernel jupyterlab \
    pandas numpy matplotlib seaborn && \
    conda clean -afy

# Ensure Jupyter sees R kernel
RUN conda run -n env R -e "IRkernel::installspec(user = FALSE)"

# Use conda environment by default
SHELL ["conda", "run", "-n", "env", "/bin/bash", "-c"]

# Copy your project into the image (optional)
COPY . /workspace

# Expose JupyterLab default port
EXPOSE 8888

# Default command: launch JupyterLab
CMD ["conda", "run", "-n", "env", "jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root"]