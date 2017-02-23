# kentwait/onda-notebookd:python3
FROM kentwait/miniconda:python3
MAINTAINER Kent Kawashima <kentkawashima@gmail.com>

WORKDIR /tmp
# Install Jupyter
RUN apk update && apk add --no-cache docker \
  && conda update --all --yes \
  && conda install -q -y jupyter \
  && conda clean --all --yes \
  && mkdir -p /notebooks \
  # remove packages and clean up 
  && find /opt -name __pycache__ | xargs rm -r \
  && rm -rf /tmp/* \
            /var/cache/apk/* \
            /opt/conda/pkgs/*

# Set token as empty string - also disables authenticating token for XSRF protection
RUN jupyter notebook --generate-config \
  && echo "c.NotebookApp.token = ''" >> /root/.jupyter/jupyter_notebook_config.py

EXPOSE 8888
VOLUME /notebooks
WORKDIR /notebooks
ENTRYPOINT ["/init"]
CMD ["jupyter", "notebook", "--no-browser", "--port=8888", "--ip=0.0.0.0", \
"--config=/root/.jupyter/jupyter_notebook_config.py"]