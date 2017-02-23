# kentwait/miniconda:python3
FROM kentwait/alpine-glibc-base
MAINTAINER Kent Kawashima <kentkawashima@gmail.com>

ENV CONDA_DIR /opt/conda
ENV CONDA_VERSION 4.2.12
ENV PATH ${CONDA_DIR}/bin:${PATH}

WORKDIR /tmp
RUN apk update && apk upgrade \
  && apk add --no-cache --virtual temp-pkgs \
    build-base \
    ca-certificates \
    wget \
    python \
  # Install Miniconda
  && wget -q "https://repo.continuum.io/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh" -O /tmp/installer.sh \
  && bash installer.sh -b -p ${CONDA_DIR} \
  # remove packages and clean up 
  && apk del temp-pkgs \
  && find /opt -name __pycache__ | xargs rm -r \
  && rm -rf /tmp/* \
            /var/cache/apk/* \
            /opt/conda/pkgs/* \
            /root/.wget-hsts \
            /root/.[acpw]*

WORKDIR /
ENTRYPOINT ["/init"]
CMD ["python"]