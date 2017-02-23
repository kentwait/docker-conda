# kentwait/anaconda:python3
FROM kentwait/alpine-glibc-base
MAINTAINER Kent Kawashima <kentkawashima@gmail.com>

ENV CONDA_PYTHON_VERSION 3
ENV CONDA_DIR /opt/conda
# Latest version as of 2017-01-27
ENV CONDA_VERSION 4.3.0
ENV PATH ${CONDA_DIR}/bin:${PATH}

WORKDIR /tmp
RUN apk update && apk upgrade \
  && apk add --no-cache --virtual temp-pkgs \
    build-base \
    ca-certificates \
    wget \
    python \
  # Install Anaconda
  && wget -q "https://repo.continuum.io/archive/Anaconda${CONDA_PYTHON_VERSION}-${CONDA_VERSION}-Linux-x86_64.sh" \
    -O /tmp/installer.sh \
  && bash installer.sh -b -p ${CONDA_DIR} \
  && conda clean --all --yes \
  && mkdir -p /notebooks \
  # remove packages and clean up 
  && apk del temp-pkgs \
  && find /opt -name __pycache__ | xargs rm -r \
  && rm -rf /tmp/* \
            /var/cache/apk/* \
            /opt/conda/pkgs/* \
            /root/.wget-hsts \
            /root/.[acpw]*

# Set token as empty string - also disables authenticating token for XSRF protection
RUN jupyter notebook --generate-config \
  && echo "c.NotebookApp.token = ''" >> /root/.jupyter/jupyter_notebook_config.py

EXPOSE 8888
VOLUME /notebooks
WORKDIR /notebooks
ENTRYPOINT ["/init"]
CMD ["jupyter", "notebook", "--no-browser", "--port=8888", "--ip=0.0.0.0", \
"--config=/root/.jupyter/jupyter_notebook_config.py"]