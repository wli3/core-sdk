#
# Copyright (c) .NET Foundation and contributors. All rights reserved.
# Licensed under the MIT license. See LICENSE file in the project root for full license information.
#

FROM fedora:30

# Install the base toolchain we need to build anything (clang, cmake, make and the like)
# this does not include libraries that we need to compile different projects, we'd like
# them in a different layer.
RUN dnf install -y \
        clang \
        cmake \
        findutils \
        gdb \
        lldb-devel \
        llvm-devel \
        make \
        python \
        python2-lldb \
        sudo \
        which \
    && dnf clean all

# Install tools used by build automation.
RUN dnf install -y \
        git \
        tar \
        procps \
        zip \
    && dnf clean all

# Dependencies of CoreCLR and CoreFX.
RUN dnf install -y \
        compat-openssl10-devel \
        glibc-locale-source \
        iputils \
        krb5-devel \
        libcurl-devel \
        libgdiplus \
        libicu-devel \
        libunwind-devel \
        libuuid-devel \
        lttng-ust-devel \
        uuid-devel \
    && dnf clean all

RUN dnf install -y nss

RUN dnf clean all

# Override RID set by the dotnet-buildtools-prereqs docker image
ENV __PUBLISH_RID=fedora.30-x64

# Setup User to match Host User, and give superuser permissions 
ARG USER_ID=0 
RUN useradd -m code_executor -u ${USER_ID} -g wheel
RUN echo 'code_executor ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers 
 
# With the User Change, we need to change permissions on these directories 
RUN chmod -R a+rwx /usr/local 
RUN chmod -R a+rwx /home
 
# Set user to the one we just created 
USER ${USER_ID} 
 
# Set working directory 
ARG WORK_DIR 
WORKDIR ${WORK_DIR}