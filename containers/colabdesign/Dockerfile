FROM nvcr.io/nvidia/ai-workbench/python-cuda122:1.0.3

ARG COLABDESIGN_VERSION=d024c4e846fea83c090afcbe89a313eeee8ec01e

RUN git clone https://github.com/sokrypton/ColabDesign.git /opt/colabdesign \
	&& cd /opt/colabdesign \
	&& git checkout $COLABDESIGN_VERSION \
	&& pip install --no-cache-dir . \
	&& pip install --no-cache-dir jax[cuda12]==0.6.2 \
	&& pip freeze | grep nvidia-cudnn

WORKDIR /
