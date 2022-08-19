FROM fedora:35

LABEL Description="Docker for embedded development"
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

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>


RUN dnf update -y
RUN dnf install -y java-11-openjdk-headless git ant cmake zlib-devel nano libxml2-devel libxslt-devel gdb cppcheck gdb-gdbserver valgrind ccache graphviz doxygen gcc gcovr lcov python3 python3-pip clang-tools-extra clang-analyzer javacc SFML SFML-devel glx-utils mesa-dri-drivers lldb curl ninja-build dos2unix wget lbzip2 make glibc-locale-source findutils clang-tools-extra stlink stlink-gui openocd ncurses-libs ncurses-compat-libs python3.8
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
            
RUN wget -v https://developer.arm.com/-/media/Files/downloads/gnu/11.3.rel1/binrel/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-eabi.tar.xz -O /tmp/gcc-arm-none-eabi-11.3.tar.xz
RUN mkdir -p /opt/gcc-arm-none-eabi-11.3
RUN tar -xf /tmp/gcc-arm-none-eabi-11.3.tar.xz -C /opt/gcc-arm-none-eabi-11.3 --strip-components=1
RUN ln -s /opt/gcc-arm-none-eabi-11.3/bin/* /usr/local/bin
RUN rm -rf /tmp/*

#TODO: instalando rust
#RUN curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

WORKDIR /workspaces

RUN dnf install -y python3-pyasn1-modules

RUN dnf clean packages

USER $USERNAME

CMD ["/bin/bash"]
