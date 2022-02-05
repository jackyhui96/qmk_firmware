# [Choice] alpine,...
# ARG VARIANT="alpine"
ARG VARIANT="latest"
# FROM erlang:${VARIANT}
FROM arm32v7/buildpack-deps:${VARIANT}



ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1001
ARG GRPNAME=vscodegrp

ENV DEBIAN_FRONTEND=noninteractive

# RUN addgroup -g $USER_GID $GRPNAME \
#     && adduser -u $USER_UID -g $GRPNAME -D $USERNAME
RUN addgroup --gid $USER_GID $GRPNAME \
    && adduser --uid $USER_UID $USERNAME \
    && adduser $USERNAME $GRPNAME

RUN apt-get update && apt-get install --no-install-recommends -y \
    avrdude \
    binutils-arm-none-eabi \
    binutils-riscv64-unknown-elf \
    build-essential \
    ca-certificates \
    clang-format-11 \
    dfu-programmer \
    dfu-util \
    dos2unix \
    ca-certificates \
    gcc \
    gcc-arm-none-eabi \
    gcc-riscv64-unknown-elf \
    git \
    libfl2 \
    libnewlib-arm-none-eabi \
    picolibc-riscv64-unknown-elf \
    python3 \
    python3-pip \
    software-properties-common \
    tar \
    teensy-loader-cli \
    unzip \
    tar \
    wget \
    zip \
    && rm -rf /var/lib/apt/lists/*

# upgrade avr-gcc... for reasons?
RUN /bin/bash -c "set -o pipefail && \
    wget -q https://blog.zakkemble.net/download/avr-gcc-8.3.0-x64-linux.tar.bz2 -O - | tee /tmp/asdf.tar.bz2 | md5sum -c <(echo '588D0BEA4C5D21A1A06AA17625684417  -') && \
    tar xfj /tmp/asdf.tar.bz2 --strip-components=1 -C / && \
    rm -rf /share/ /tmp/*"

# Install python packages
RUN python3 -m pip install --upgrade pip setuptools wheel
RUN python3 -m pip install nose2 qmk

# Copy package in
# ADD dist /tmp/dist

# # Install python packages
# RUN python3 -m pip uninstall -y qmk || true
# RUN python3 -m pip install --upgrade pip setuptools wheel nose2 && \
#     python3 -m pip install /tmp/dist/qmk-*.whl && \
#     rm -rf /tmp/dist

# Set the default location for qmk_firmware
ENV QMK_HOME /qmk_firmware

USER $USERNAME
