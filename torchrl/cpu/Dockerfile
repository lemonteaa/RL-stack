# Start from the base Jupyter notebook image
FROM quay.io/jupyter/base-notebook

# Set environment variables for runtime
ENV MUJOCO_GL=osmesa
ENV PYOPENGL_PLATFORM=osmesa

ENV GRANT_SUDO=yes

# Switch to root user to install additional packages
USER root

# Install additional packages via apt-get
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y \
    libxrender1 libgl1-mesa-dev libglew-dev libosmesa6-dev xpra patchelf libglfw3-dev libglfw3 \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Switch back to jovyan user (default user for Jupyter images)
USER jovyan

# Install Python packages from requirements.txt
COPY requirements.txt /home/jovyan/
RUN pip3 install --no-cache-dir torch torchvision --index-url https://download.pytorch.org/whl/cpu
RUN pip3 install --no-cache-dir -r /home/jovyan/requirements.txt

# Expose the port for Jupyter
EXPOSE 8888

# Reuse the existing entrypoint of the base image
# The entrypoint is already correctly set up in the base image

# Switch back to root?
USER root
