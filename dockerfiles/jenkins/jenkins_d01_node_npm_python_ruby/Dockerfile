# from
FROM jenkins/jenkins

MAINTAINER dodo_lihao <lihao@lihao8.cn>

# change to root
USER root

SHELL ["/bin/bash","-c"]

# install
RUN \
    apt-get autoremove && apt-get clean && \
    apt-get update && \
    apt-get install -y --no-install-recommends apt-transport-https build-essential libicu-dev 'libicu[0-9][0-9].*' libbz2-dev libncurses5-dev libreadline-dev libsqlite3-dev libssl1.0-dev libncursesw5-dev libltdl-dev libyaml-dev zlib1g-dev libxml2-dev libxslt-dev libreadline6-dev libffi-dev libgdbm3 libgdbm-dev libxslt1-dev libcurl4-openssl-dev tk-dev xz-utils autoconf bison sudo lsb-release curl git rlwrap wget zip unzip vim mysql-client ssh-client xmlstarlet sshpass rsync llvm make jq python-all && \
    rm -rf /var/lib/apt/lists/*

RUN \
    bash -c echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers && \


    mkdir /root/.ssh && \
    echo 'Host *' > /root/.ssh/config && \
    echo ' StrictHostKeyChecking no' >> /root/.ssh/config && \
    chmod 400 /root/.ssh/config

ENV NODE_VERSION=6.9.5  NPM_VERSION=3.10.10   GRADLE_VERSION=3.4.1   PYTHON_VERSION=3.6.1  RUBY_VERSION=2.3.5

RUN \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash && export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" && nvm install "${NODE_VERSION}" && nvm alias default "${NODE_VERSION}" && nvm use default && npm install -g cnpm --registry=https://registry.npm.taobao.org && cnpm install -g npm@${NPM_VERSION}  && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && apt-get autoremove && apt-get clean && apt-get update && apt-get install -y --no-install-recommends yarn && rm -rf /var/lib/apt/lists/*  && \
    curl -s "https://get.sdkman.io" | bash && . "/root/.sdkman/bin/sdkman-init.sh" && sdk install gradle "${GRADLE_VERSION}" && sdk default gradle "${GRADLE_VERSION}"  && \
    git clone --depth 1 https://github.com/yyuu/pyenv.git ~/.pyenv && rm -rfv /root/.pyenv/.git && mkdir -p /root/.pyenv/cache && curl https://mirrors.aliyun.com/pypi/simple/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tar.xz > /root/.pyenv/cache/Python-${PYTHON_VERSION}.tar.xz && export PYENV_ROOT="/root/.pyenv" && export PATH="$PYENV_ROOT/bin:$PATH" && eval "$(pyenv init -)" && echo 'export PYENV_ROOT="/root/.pyenv"' >> /root/.bashrc && echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /root/.bashrc && echo 'eval "$(pyenv init -)"' >> /root/.bashrc && pyenv install ${PYTHON_VERSION} && pyenv global ${PYTHON_VERSION} && pip install --upgrade pip && pyenv rehash && rm -rf /root/.pyenv/cache/* && \
    git clone https://github.com/sstephenson/rbenv.git /root/.rbenv && git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build && /root/.rbenv/plugins/ruby-build/install.sh && export PATH="/root/.rbenv/bin:$PATH" && eval "$(rbenv init -)" && echo 'export PATH="/root/.rbenv/bin:$PATH"' >> /root/.bashrc && echo 'eval "$(rbenv init -)"' >> /root/.bashrc && export CONFIGURE_OPTS="--disable-install-doc" && rbenv install "${RUBY_VERSION}" && rbenv global "${RUBY_VERSION}" && gem install bundler && \
    export PATH="/root/.rbenv/bin:$PATH" && eval "$(rbenv init -)" && gem install fastlane -NV && \
    export PATH="/root/.rbenv/bin:$PATH" && eval "$(rbenv init -)" && gem install fir-cli -NV && \
    ln -s /bin/bash /usr/local/bin/bash && echo 'export PATH="./deps/bin:$PATH"' >> /root/.bashrc

ENV PATH=./deps/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY entry-point.sh /entry-point.sh

ENTRYPOINT ["/entry-point.sh"]


#RUN \
#    chmod +x /entry-point.sh && chown jenkins:jenkins /entry-point.sh && \
#    USER [jenkins] && \
#    ENTRYPOINT ["/entry-point.sh"]
