ARG SINGLEUSER_BASE=quay.io/jupyterhub/singleuser:5.2.1

FROM ${SINGLEUSER_BASE} AS base
USER root

ADD conda-envs /tmp/conda-envs
ADD apt /tmp/apt

#install dependencies and other stuff that might be useful to users and turn on man pages
RUN \
    apt-get -y update && \
    xargs apt-get -y install < /tmp/apt/base.txt && \
    yes | unminimize ; exit 0 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \

HEALTHCHECK  --interval=15s --timeout=3s --start-period=5s --retries=3 \
    CMD wget -O- --no-verbose --tries=1 http://localhost:8888${JUPYTERHUB_SERVICE_PREFIX:-/}api || exit 1

# add common python libraries (e.g. pangeo stack) 
FROM base AS with-python-libs
RUN conda env update -n base -f /tmp/conda-envs/base.yml && conda env update -n base -f /tmp/conda-envs/pangeo.yml

# download natural earth datasets for cartopy
RUN cartopy_feature_download physical --output /opt/conda/lib/python3.11/site-packages/cartopy/data/ && \
cartopy_feature_download gshhs --output /opt/conda/lib/python3.11/site-packages/cartopy/data/ && \
cartopy_feature_download cultural --output /opt/conda/lib/python3.11/site-packages/cartopy/data/ && \
cartopy_feature_download cultural-extra --output /opt/conda/lib/python3.11/site-packages/cartopy/data/

