FROM fedora:35

LABEL Description="Docker for development"
# User Settings
ARG USERNAME=some_user
ARG UID=1000
ARG GID=1000
ARG GROUPNAME=some_user

#getting this info from binds in devcontainer.json of /etc
RUN groupadd --gid $GID $GROUPNAME
RUN useradd --uid $UID --gid $GID -m $USERNAME
RUN usermod --append --groups $GROUPNAME $USERNAME
RUN usermod -a -G dialout $USERNAME
RUN usermod --shell /bin/bash $USERNAME    
# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Option] Install Node.js
ARG INSTALL_NODE="false"
ARG NODE_VERSION="lts/*"
RUN if [ "${INSTALL_NODE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

RUN dnf update -y
RUN dnf install -y java-11-openjdk-headless git ant cmake zlib-devel nano libxml2-devel libxslt-devel gdb cppcheck gdb-gdbserver valgrind ccache graphviz doxygen gcc gcovr lcov python3 python3-pip clang-tools-extra clang-analyzer javacc SFML SFML-devel glx-utils mesa-dri-drivers lldb curl ninja-build dos2unix wget lbzip2 make glibc-locale-source findutils clang-tools-extra 
#arm-none-eabi-*

RUN pip3 install cmake-format 
RUN pip3 install conan 

#instalando JLink
RUN curl --data "accept_license_agreement=accepted" https://www.segger.com/downloads/jlink/JLink_Linux_V768c_x86_64.rpm --output jlinkV768c.rpm
RUN wget -c https://www.segger.com/downloads/systemview/systemview_linux_rpm64 -O systemview.rpm
RUN dnf install -y jlinkV768c.rpm systemview.rpm
RUN wget -c https://www.segger.com/downloads/systemview/systemview_target_src -O /opt/systemview-target-src.zip
RUN rm jlinkV768c.rpm systemview.rpm

RUN mkdir /workspaces

# download das ferramentas direto da ARM
RUN wget -v https://developer.arm.com/-/media/Files/downloads/gnu/11.2-2022.02/binrel/gcc-arm-11.2-2022.02-x86_64-arm-none-eabi.tar.xz -O /tmp/gcc-arm-none-eabi-11.2.tar.xz
RUN mkdir -p /opt/gcc-arm-none-eabi-11.2
RUN tar -xf /tmp/gcc-arm-none-eabi-11.2.tar.xz -C /opt/gcc-arm-none-eabi-11.2 --strip-components=1
RUN ln -s /opt/gcc-arm-none-eabi-11.2/bin/* /usr/local/bin
RUN rm -rf /tmp/*

RUN dnf update -y
RUN dnf install -y stlink stlink-gui openocd ncurses-libs ncurses-compat-libs
RUN dnf install -y python3.6
#st-stlink-udev-rules

#TODO: instalando rust
#RUN dnf install -y rust cargo
#RUN curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
WORKDIR /workspaces

RUN dnf clean packages

USER $USERNAME

CMD ["/bin/bash"]
