FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends\
    build-essential \
    python \
    python-dev \
    python-pip \
    python-pip \
    python-wheel \
    python-setuptools \
    python-numpy \
    python3 \
    python3-dev \
    python3-pip \
    python3-wheel \
    python3-setuptools \
    python3-numpy \
    wget \
    git \
    zsh \
    locales \
    sudo \
    tmux \
    vim \
    && \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/* && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

ARG GROUP_ID=51000
ARG GROUP_NAME=fulltime
RUN addgroup --gid ${GROUP_ID} ${GROUP_NAME}

ARG USER_ID=2072
ARG USER_NAME=shunta
RUN mkdir -p /home/${USER_NAME}
RUN useradd -d /home/${USER_NAME} -g ${GROUP_ID} -G sudo -u ${USER_ID} ${USER_NAME}
RUN addgroup --gid ${USER_ID} ${USER_NAME}
RUN chown ${USER_NAME}:root /home/${USER_NAME}
ENV HOME /home/${USER_NAME}
RUN usermod -s /usr/bin/zsh ${USER_NAME}

# Switch to USER_NAME
USER ${USER_NAME}
WORKDIR /home/${USER_NAME}
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Install oh-my-zsh
RUN git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh && \
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

# Install pyenv
RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv && \
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc && \
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc && \
    echo 'export PATH="$PYENV_ROOT/shims:$PATH"' >> ~/.zshrc && \
    echo 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
ENV PYENV_ROOT $HOME/.pyenv
ENV PATH $PYENV_ROOT/bin:$PATH
ENV PATH $PYENV_ROOT/shims:$PATH
RUN eval "$(pyenv init -)"

# Install miniconda3-4.3.0
RUN pyenv install miniconda3-4.3.30 && \
    pyenv global miniconda3-4.3.30 && \
    pyenv rehash

RUN CONDA=$(pyenv which conda) && \
    PIP=$(pyenv which pip) && \
    $PIP install -U pip && \
    $PIP install PyHamcrest && \
    $CONDA install -y numpy scipy scikit-learn scikit-image jupyter matplotlib cython protobuf pandas h5py cmake && \
    $CONDA install -y pytorch=0.3.0 torchvision=0.2.0 cuda90 -c pytorch

RUN PIP=$(pyenv which pip) && \
    $PIP install tqdm dill ipdb pycocotools
