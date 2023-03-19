FROM ghcr.io/mathworks-ref-arch/matlab-integration-for-jupyter/jupyter-matlab-notebook:r2021a

COPY . /tmp/build

ENV CONDA_ENV="openscapes"
ENV PYTHON_PREFIX=/opt/conda

WORKDIR /tmp/build

RUN echo "Checking for 'conda-linux-64.lock' or 'environment.yml'..." \
        ; if test -f "conda-linux-64.lock" ; then \
        mamba create --name ${CONDA_ENV} --file conda-linux-64.lock \
        ; elif test -f "environment.yml" ; then \
        mamba env create --name ${CONDA_ENV} -f environment.yml  \
        ; else echo "No conda-linux-64.lock or environment.yml! *creating default env*" ; \
        mamba create --name ${CONDA_ENV} pangeo-notebook \
        ; fi \
        && mamba clean -yaf \
        && find ${CONDA_DIR} -follow -type f -name '*.a' -delete \
        && find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete \
        && find ${CONDA_DIR} -follow -type f -name '*.js.map' -delete \
        ; if [ -d ${NB_PYTHON_PREFIX}/lib/python*/site-packages/bokeh/server/static ]; then \
        find ${NB_PYTHON_PREFIX}/lib/python*/site-packages/bokeh/server/static -follow -type f -name '*.js' ! -name '*.min.js' -delete \
        ; fi


WORKDIR ${HOME}

EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--no-browser"]
