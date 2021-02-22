## Install the dask-labextension on an AI Notebook - https://github.com/dask/dask-labextension

## DISCLAIMER: THIS JUPYTER LAB EXTENSION IS NOT SUPPORTED BY AI NOTEBOOKS, AND THE INSTRUCTIONS BELOW ARE NOT GUARANTEED TO WORK

# Create the notebook instance (either using the UI, or by submitting the command below)
gcloud beta notebooks instances create rapids-dask-on-gcp-gpu-t4 \
    --vm-image-project=deeplearning-platform-release \
    --vm-image-family=rapids-latest-gpu-experimental \
    --machine-type=n1-standard-4 \
    --location=us-central1-b \
    --boot-disk-size=100 \
    --accelerator-core-count=1 \
    --accelerator-type=NVIDIA_TESLA_T4 \
    --install-gpu-driver \
    --network=default
    
# SSH into the notebook terminal

# upgrade jupyterlab
pip install --upgrade jupyterlab

# upgrade node version to rebuild extensions
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
rm /opt/conda/bin/node
ln -s /usr/bin/node /opt/conda/bin/node

# install dask extension and git extensions
pip install dask_labextension
pip install --pre jupyterlab-git==0.30.0b1

# install/upgrade lab extensions
/opt/conda/bin/jupyter labextension install dask-labextension
/opt/conda/bin/jupyter labextension install @jupyterlab/git

# enable dask extension
/opt/conda/bin/jupyter serverextension enable dask_labextension

# update and build extensions
/opt/conda/bin/jupyter labextension update --all
/opt/conda/bin/jupyter lab build


# Restart (stop and start) the notebook instance
