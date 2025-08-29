# Use the official NVIDIA CUDA base image
FROM nvidia/cuda:12.4.1-cudnn-runtime-ubuntu20.04

# Set environment variables for versions and URLs to keep the Dockerfile clean
# --- User and timezone settings ---
ARG USER_NAME=developer
ARG USER_UID=1000
ARG USER_GID=1000
ENV HOME=/home/${USER_NAME}
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

# --- PyCharm settings ---
ENV PYCHARM_TAG=2025.2
ENV PYCHARM_VERSIONS=2025.2.0.1
ENV PYCHARM_URL=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Pycharm/pycharm-${PYCHARM_VERSIONS}.tar.gz
ENV PYCHARM_CONFIG_URL=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Pycharm/Configs/PyCharm2025.1.zip
ENV PYCHARM_PLUGINS_URL=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Pycharm/Plugins/PyCharm2025.1.zip

# --- Pyenv and Python versions ---
ENV PYENV_VERSION=2.5.0
ENV PYENV_URL=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Python/pyenv-${PYENV_VERSION}.zip
ENV PYTHON_VERSIONS_1=3.6.0
ENV PYTHON_URL_1=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Python/Python-${PYTHON_VERSIONS_1}.tar.xz
ENV PYTHON_VERSIONS_2=3.9.9
ENV PYTHON_URL_2=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Python/Python-${PYTHON_VERSIONS_2}.tar.xz
ENV PYTHON_VERSIONS_3=3.11.0
ENV PYTHON_URL_3=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Python/Python-${PYTHON_VERSIONS_3}.tar.xz
ENV PYTHON_VERSIONS_4=3.12.5
ENV PYTHON_URL_4=https://bj.bcebos.com/petite-mark/public_read/vipshop/jhc/Jetbrains/Python/Python-${PYTHON_VERSIONS_4}.tar.xz

# --- PATH configuration ---
ENV PYENV_ROOT="${HOME}/.pyenv"
ENV PATH="${PYENV_ROOT}/bin:${PYENV_ROOT}/shims:${HOME}/.local/bin:${PATH}"

# Install all system dependencies and create the non-root user (as root)
RUN apt-get update && apt-get install -y --no-install-recommends \
    # Basic utilities
    sudo \
    wget \
    curl \
    unzip \
    git \
    vim \
    net-tools \
    # For PyCharm GUI
    openjdk-11-jdk \
    libx11-6 \
    libxcomposite1 \
    libxrandr2 \
    libxss1 \
    libgdk-pixbuf2.0-0 \
    libgtk-3-0 \
    xauth \
    # For building Python with pyenv
    build-essential \
    libbz2-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libffi-dev \
    libreadline-dev \
    libssl-dev \
    libsqlite3-dev \
    liblzma-dev \
    tk-dev \
    libgdbm-dev \
    libc6-dev \
    # For Chinese fonts and input method
    fonts-arphic-uming \
    fonts-noto-cjk \
    locales \
    ibus \
    ibus-pinyin \
    # For Chrome
    fonts-liberation \
    libgbm1 \
    xdg-utils \
    && \
    # Create a non-root user with sudo privileges
    groupadd --gid ${USER_GID} ${USER_NAME} && \
    useradd --uid ${USER_UID} --gid ${USER_GID} -m --shell /bin/bash ${USER_NAME} && \
    echo "${USER_NAME} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    # Set up timezone and locale
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    locale-gen zh_CN.UTF-8 && update-locale LANG=zh_CN.UTF-8 && \
    # Install PyCharm and Chrome
    wget ${PYCHARM_URL} -O /tmp/pycharm.tar.gz && \
    tar -xzf /tmp/pycharm.tar.gz -C /opt/ && \
    ln -s /opt/pycharm-${PYCHARM_VERSIONS} /opt/pycharm && \
    rm /tmp/pycharm.tar.gz && \
    wget "https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb" -O /tmp/chrome.deb && \
    dpkg -i /tmp/chrome.deb || apt-get install -f -y && \
    rm /tmp/chrome.deb || true && \
    # Clean up
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Switch to the non-root user and set working directory for subsequent commands
USER ${USER_NAME}
WORKDIR ${HOME}

# Set language and input method environment for the user
ENV LANG=zh_CN.UTF-8
ENV LC_ALL=zh_CN.UTF-8
ENV GTK_IM_MODULE=ibus
ENV QT_IM_MODULE=ibus
ENV XMODIFIERS=@im=ibus

# Perform user-specific installations and configurations
RUN \
    mkdir -p ${HOME}/.config/JetBrains ${HOME}/.local/share/JetBrains && \
    # Configure PyCharm configs and settings
    wget ${PYCHARM_CONFIG_URL} -O /tmp/pycharm_config.zip && \
    tar -zxvf /tmp/pycharm_config.zip -C ${HOME}/.config/JetBrains/ && \
    if [ ! -d "${HOME}/.config/JetBrains/PyCharm${PYCHARM_TAG}" ]; then mv "${HOME}/.config/JetBrains/PyCharm2025.1" "${HOME}/.config/JetBrains/PyCharm${PYCHARM_TAG}"; fi && \
    # Configure PyCharm plugins and settings
    wget ${PYCHARM_PLUGINS_URL} -O /tmp/pycharm_plugins.zip && \
    tar -zxvf /tmp/pycharm_plugins.zip -C ${HOME}/.local/share/JetBrains/ && \
    if [ ! -d "${HOME}/.local/share/JetBrains/PyCharm${PYCHARM_TAG}" ]; then mv "${HOME}/.local/share/JetBrains/PyCharm2025.1" "${HOME}/.local/share/JetBrains/PyCharm${PYCHARM_TAG}"; fi && \
    # Install pyenv and Python versions
    wget ${PYENV_URL} -O /tmp/pyenv.zip && \
    unzip /tmp/pyenv.zip -d /tmp && \
    mv /tmp/pyenv-${PYENV_VERSION} ${PYENV_ROOT} && \
    echo 'export PATH="$HOME/.pyenv/bin:$PATH"\neval "$(pyenv init --path)"\neval "$(pyenv init -)"\nunset PYENV_VERSION' >> ~/.bashrc && \
    # Pre-download Python source tarballs to pyenv cache
    mkdir -p ${PYENV_ROOT}/cache && \
    wget ${PYTHON_URL_1} -O ${PYENV_ROOT}/cache/Python-${PYTHON_VERSIONS_1}.tar.xz && \
    wget ${PYTHON_URL_2} -O ${PYENV_ROOT}/cache/Python-${PYTHON_VERSIONS_2}.tar.xz && \
    wget ${PYTHON_URL_3} -O ${PYENV_ROOT}/cache/Python-${PYTHON_VERSIONS_3}.tar.xz && \
    wget ${PYTHON_URL_4} -O ${PYENV_ROOT}/cache/Python-${PYTHON_VERSIONS_4}.tar.xz && \
    # Install Python versions
    pyenv install ${PYTHON_VERSIONS_1} && \
    pyenv install ${PYTHON_VERSIONS_2} && \
    pyenv install ${PYTHON_VERSIONS_3} && \
    pyenv install ${PYTHON_VERSIONS_4} && \
    pyenv global ${PYTHON_VERSIONS_2} && \
    # Start ibus for Chinese input
    ibus-daemon -d -x && \
    # Create helper scripts and give them execute permissions
    echo '/usr/bin/google-chrome --no-sandbox --start-maximized --force-dark-mode --proxy-server="socks5://172.17.0.1:10808" $1' > ${HOME}/chrome.sh && \
    echo '#!/bin/bash\nrm -f ${HOME}/.config/JetBrains/PyCharm*/.lock\n# Start PyCharm\n/opt/pycharm/bin/pycharm.sh' > ${HOME}/start.sh && \
    chmod +x ${HOME}/chrome.sh ${HOME}/start.sh && \
    rm -rf ~/.pyenv/cache/Python-*.tar.xz && \
    rm /tmp/*.zip

# Install uv (fast package manager)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# Set the default command to run the startup script
CMD ["sh", "-c", "/home/developer/start.sh"]