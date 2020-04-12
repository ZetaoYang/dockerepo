# debian Linux 10, Python 3.7.3, JupyterLab 2.1.0/Jupyter Notebook, Tensorflow 2.1.0
# docker container run -d -p 8888:8888 --name jupyterlab -v d:/Projects/data:/opt/notebooks -e NB_PASSWORD="" vitzy/jupyterlab
FROM docker.io/debian:10-slim

LABEL maintainer="Ahmed Zetao Yang <vitzys@outlook.com>" \
      name="Docker Python Deep Learning" \
      description="Docker container for Python Deep Learning, with almost everything that you may need." \
      url="" \
      vcs-url="" \
      vendor="Ahmed Zetao Yang" \
      version="1.0.0"

# NB_OR_LAB: "lab" or "notebook"
ENV LANGUAGE=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    LANG=C.UTF-8 \
    NB_OR_LAB="lab" \
    NB_PORT=8888 \
    NB_TOKEN="" \
    NB_PASSWORD="" \
    NB_PATH="/opt/notebooks"

RUN APT_PACKAGES="\
        python3 \
        python3-pip \
        python3-wheel \
        python3-setuptools" \
    PIP_PACKAGES="\
        ipython \
        numpy \
        pandas \
        matplotlib \
        jupyter \
        jupytext \
        jupyterlab \
        tensorflow" \
    JUPYTER_CONFIG_DIR=/home/jupyter/.jupyter \
    # && sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list; \
    # sed -i 's|security.debian.org/debian-security|mirrors.tuna.tsinghua.edu.cn/debian-security|g' /etc/apt/sources.list; \
    && set -ex; \
    apt-get update -y && apt-get upgrade -y; \
    apt-get install -y --no-install-recommends ${APT_PACKAGES}; \
    pip3 install -U -V pip; \
    pip3 install -U -v setuptools wheel; \
    pip3 --default-timeout=600 install -U -v ${PIP_PACKAGES}; \
    apt-get clean; \
    apt-get autoclean; \
    apt-get autoremove; \
    rm -rf /tmp/* /var/tmp/*; \
    rm -rf /var/lib/apt/lists/*; \
    rm -f /var/cache/apt/archives/*.deb \
        /var/cache/apt/archives/partial/*.deb \
        /var/cache/apt/*.bin; \
    find / -name __pycache__ | xargs rm -r; \
    rm -rf /root/.[acpw]*; \
    jupyter nbextension enable --py widgetsnbextension; \
    mkdir -p ${JUPYTER_CONFIG_DIR}; \
    echo "import warnings" | tee ${JUPYTER_CONFIG_DIR}/jupyter_notebook_config.py; \
    echo "warnings.filterwarnings('ignore')" | tee -a ${JUPYTER_CONFIG_DIR}/jupyter_notebook_config.py; \
    echo "c.NotebookApp.token = u''" | tee -a ${JUPYTER_CONFIG_DIR}/jupyter_notebook_config.py;

WORKDIR ${NB_PATH}

EXPOSE ${NB_PORT}

CMD [ "sh", "-c", "jupyter ${NB_OR_LAB} --port=${NB_PORT} --no-browser --allow-root --ip=0.0.0.0 --NotebookApp.token=${NB_TOKEN} --NotebookApp.password=${NB_PASSWORD}" ]