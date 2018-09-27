FROM jenkinsci/jenkins:2.141

MAINTAINER dodo_lihao <lihao@lihao8.cn>

USER root

SHELL ["/bin/bash", "-c"]

# Set the env variable DEBIAN_FRONTEND to noninteractive
ENV DEBIAN_FRONTEND noninteractive

# RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
# COPY ./assets/sources.list.jessie /etc/apt/sources.list

RUN apt-get autoremove \
    && apt-get clean \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
      apt-transport-https \
      build-essential \
      libicu-dev \
      'libicu[0-9][0-9].*' \
      libbz2-dev \
      libncurses5-dev \
      libreadline-dev \
      libsqlite3-dev \
      libssl1.0-dev \
      libncursesw5-dev \
      libltdl-dev \
      libyaml-dev \
      zlib1g-dev \
      libxml2-dev \
      libxslt-dev \
      libreadline6-dev \
      libffi-dev \
      libgdbm3 \
      libgdbm-dev \
      libxslt1-dev \
      libcurl4-openssl-dev \
      tk-dev \
      xz-utils \
      autoconf \
      bison \
      sudo \
      lsb-release \
      curl \
      git \
      rlwrap \
      wget \
      zip \
      unzip \
      vim \
      mysql-client \
      ssh-client \
      xmlstarlet \
      sshpass \
      rsync \
      llvm \
      make \
      jq \
      python-all \
    && rm -rf /var/lib/apt/lists/*

# sudoers
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

# ssh
RUN mkdir /root/.ssh && \
    echo 'Host *' > /root/.ssh/config && \
    echo '  StrictHostKeyChecking no' >> /root/.ssh/config && \
    chmod 400 /root/.ssh/config

# nodejs
ENV NODE_VERSION=6.9.5
ENV NPM_VERSION=3.10.10
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    . "$NVM_DIR/nvm.sh" && \
    nvm install "${NODE_VERSION}" && \
    nvm alias default "${NODE_VERSION}" && \
    nvm use default && \
    npm install -g cnpm --registry=https://registry.npm.taobao.org && \
    cnpm install -g npm@${NPM_VERSION}

# yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
    apt-get autoremove && \
    apt-get clean && \
    apt-get update && \
    apt-get install -y --no-install-recommends yarn && \
    rm -rf /var/lib/apt/lists/*

# gradle
ENV GRADLE_VERSION=3.4.1
RUN curl -s "https://get.sdkman.io" | bash && \
    . "/root/.sdkman/bin/sdkman-init.sh" && \
    sdk install gradle "${GRADLE_VERSION}" && \
    sdk default gradle "${GRADLE_VERSION}"

# python3
ENV PYTHON_VERSION=3.6.1
RUN git clone --depth 1 https://github.com/yyuu/pyenv.git ~/.pyenv && \
  rm -rfv /root/.pyenv/.git && \
  mkdir -p /root/.pyenv/cache && \
  curl http://mirrors.sohu.com/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tar.xz > /root/.pyenv/cache/Python-${PYTHON_VERSION}.tar.xz && \
  export PYENV_ROOT="/root/.pyenv" && \
  export PATH="$PYENV_ROOT/bin:$PATH" && \
  eval "$(pyenv init -)" && \
  echo 'export PYENV_ROOT="/root/.pyenv"' >> /root/.bashrc && \
  echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /root/.bashrc && \
  echo 'eval "$(pyenv init -)"' >> /root/.bashrc && \
  pyenv install ${PYTHON_VERSION} && \
  pyenv global ${PYTHON_VERSION} && \
  pip install --upgrade pip && \
  pyenv rehash && \
  rm -rf /root/.pyenv/cache/*

# ruby
ENV RUBY_VERSION=2.3.5
RUN git clone https://github.com/sstephenson/rbenv.git /root/.rbenv && \
    git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build && \
    /root/.rbenv/plugins/ruby-build/install.sh && \
    export PATH="/root/.rbenv/bin:$PATH" && \
    eval "$(rbenv init -)" && \
    echo 'export PATH="/root/.rbenv/bin:$PATH"' >> /root/.bashrc && \
    echo 'eval "$(rbenv init -)"' >> /root/.bashrc && \
    export CONFIGURE_OPTS="--disable-install-doc" && \
    rbenv install "${RUBY_VERSION}" && \
    rbenv global "${RUBY_VERSION}" && \
    gem install bundler

# fastlane
RUN export PATH="/root/.rbenv/bin:$PATH" && \
    eval "$(rbenv init -)" && \ 
    gem install fastlane -NV

# fir.im
RUN export PATH="/root/.rbenv/bin:$PATH" && \
    eval "$(rbenv init -)" && \
    gem install fir-cli -NV

# bash
RUN ln -s /bin/bash /usr/local/bin/bash && \
  echo 'export PATH="./deps/bin:$PATH"' >> /root/.bashrc

# bpkg
ENV PATH ./deps/bin:$PATH

# entry-point
COPY entry-point.sh /entry-point.sh
RUN chmod +x /entry-point.sh && chown jenkins:jenkins /entry-point.sh

USER jenkins

ENTRYPOINT ["/entry-point.sh"]