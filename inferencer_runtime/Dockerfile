FROM nvcr.io/nvidia/pytorch:21.12-py3

# apt & Timezone Settings & sudo
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Seoul
RUN apt update && apt install -y tzdata && apt install -y sudo

# apt for library
RUN apt install -y net-tools iputils-ping openjdk-8-jre

# Install Nodejs for jupyterlab-manager
# node lts
RUN curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && \
 sudo apt-get install -yq --no-install-recommends \
 nodejs \
 && sudo apt-get clean && \
 sudo rm -rf /var/lib/apt/lists/*

# install latest JL, ipywidgets
RUN pip uninstall jupyterlab -y && pip install jupyterlab ipywidgets

# Add Tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN sudo mv /tini /usr/local/bin/tini && \
	sudo chmod +x /usr/local/bin/tini

# set the home directory
ENV CODE_WORKINGDIR="/home/jovyan"

RUN mkdir -p /home/jovyan
ENV HOME /home/jovyan
WORKDIR ${HOME}

# Configure container startup
EXPOSE 8888
ENTRYPOINT ["tini", "--"]

CMD ["sh", "-c", \
 "jupyter lab --notebook-dir=/home/jovyan --ip=0.0.0.0 --no-browser \
 --allow-root --port=8888 --LabApp.token='' --LabApp.password='' --NotebookApp.disable_check_xsrf='True' \
 --LabApp.allow_origin='*' --LabApp.base_url=${NB_PREFIX}"]