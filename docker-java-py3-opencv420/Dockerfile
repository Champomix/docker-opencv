FROM alpine:latest
MAINTAINER fjuillet <fjuillet@gmail.com>

RUN cwd=$(pwd)

# ENV OPENCV_VERSION=4.3.0
ENV OPENCV_VERSION=4.2.0
ENV JAVA_VERSION=jdk-14+36
ENV ANT_VERSION=1.9.14

ARG DEBIAN_FRONTEND=noninteractive

########################################
#### update and upgrade OS
########################################
RUN apt -y update \
	&& apt -y upgrade

########################################
#### install ant
########################################
RUN cd && \
    wget -q https://downloads.apache.org/ant/binaries/apache-ant-${ANT_VERSION}-bin.tar.gz && \
    tar -xzf apache-ant-${ANT_VERSION}-bin.tar.gz && \
    mkdir -p /opt/ant && \
    mv apache-ant-${ANT_VERSION} /opt/ant && \
    rm apache-ant-${ANT_VERSION}-bin.tar.gz && \
	ls -al /opt/ant && \
    cd $cwd 
	
ENV ANT_HOME /opt/ant/apache-ant-${ANT_VERSION}
ENV ANT_BIN ${ANT_HOME}/bin
ENV PATH ${ANT_BIN}:${PATH}

########################################
#### install JAVA
#### https://github.com/AdoptOpenJDK/openjdk-docker/blob/3b28dd3fb7230f208ed49da507432a01741b1259/14/jdk/ubuntu/Dockerfile.hotspot.releases.full
########################################
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl ca-certificates fontconfig locales && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen en_US en_US.UTF-8 && \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
       aarch64|arm64) \
         ESUM='902899c9c4781a5ef16fc79bf153e9decff595a6597e39e2e4672faf2ec9c99f'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk14-binaries/releases/download/jdk-14%2B36/OpenJDK14U-jdk_aarch64_linux_hotspot_14_36.tar.gz'; \
         ;; \
       armhf|armv7l) \
         ESUM='fa958d9eebb37c0767fce4446e73f40a82e43bcfc447c4b5279e35aa159894ac'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk14-binaries/releases/download/jdk-14%2B36/OpenJDK14U-jdk_arm_linux_hotspot_14_36.tar.gz'; \
         ;; \
       ppc64el|ppc64le) \
         ESUM='1439630557d2045727708d838325a54c8423cb13eb8837315f2cc8f084b07f03'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk14-binaries/releases/download/jdk-14%2B36/OpenJDK14U-jdk_ppc64le_linux_hotspot_14_36.tar.gz'; \
         ;; \
       s390x) \
         ESUM='b000a725e10293841e49183902f73b91f475f2f0876a26084b7983224c0a8dc6'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk14-binaries/releases/download/jdk-14%2B36/OpenJDK14U-jdk_s390x_linux_hotspot_14_36.tar.gz'; \
         ;; \
       amd64|x86_64) \
         ESUM='6c06853332585ab58834d9e8a02774b388e6e062ef6c4084b4f058c67f2e81b5'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk14-binaries/releases/download/jdk-14%2B36/OpenJDK14U-jdk_x64_linux_hotspot_14_36.tar.gz'; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac; \
    curl -LfsSo /tmp/openjdk.tar.gz ${BINARY_URL}; \
    echo "${ESUM} */tmp/openjdk.tar.gz" | sha256sum -c -; \
    mkdir -p /opt/java/openjdk; \
    cd /opt/java/openjdk; \
    tar -xf /tmp/openjdk.tar.gz --strip-components=1; \
    rm -rf /tmp/openjdk.tar.gz;

ENV JAVA_HOME=/opt/java/openjdk \
    PATH="/opt/java/openjdk/bin:$PATH"
	
########################################
#### update ubuntu and Install Python 3
########################################
RUN apt-get update && \
    apt-get install -y apt-utils automake pkg-config libpcre3-dev zlib1g-dev liblzma-dev && \
    apt-get install -y curl iputils-ping nmap net-tools build-essential software-properties-common libsqlite3-dev sqlite3 bzip2 libbz2-dev git wget unzip vim python3-pip python3-setuptools python3-dev python3-venv python3-numpy python3-scipy python3-pandas python3-matplotlib python3-testresources && \
    apt-get install -y git xz-utils && \
    apt-get install -y sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV PATH="/usr/local/bin:/usr/local/sbin:/usr/sbin:/usr/bin:/sbin:/bin:$PATH"

########################################
#### OpenCV depedencies
########################################
RUN apt -y remove x264 libx264-dev ; exit 0

RUN apt-get update && \
    apt-get install -y \
        build-essential \
		cmake \
		doxygen \
		gfortran \
		git \
		libatlas-base-dev \
		libavcodec-dev \
		libavformat-dev \
		libavresample-dev \
		libdc1394-22-dev \
		libeigen3-dev \
		libgflags-dev \
		libgoogle-glog-dev \
		libgphoto2-dev \
		libgtk2.0-dev \
		libhdf5-dev \
		libjpeg-dev \
		libmp3lame-dev \
		libopencore-amrnb-dev \
		libopencore-amrwb-dev \
		libpng-dev \
		libpq-dev \
		libprotobuf-dev \
		libswscale-dev \
		libtbb2 \
		libtbb-dev \
		libtheora-dev \
		libtiff5-dev \
		libtiff-dev \
		libtiff-dev \
		libv4l-dev \
		libvorbis-dev \
		libxine2-dev \
		libxvidcore-dev \
		pkg-config \
		protobuf-compiler \
		qt5-default \
		unzip \
		v4l-utils \
		wget \
		x264 \
		yasm && \
	cd /usr/include/linux && \
	ln -s -f ../libv4l1-videodev.h videodev.h && \
	cd $cwd && \
    rm -rf /var/lib/apt/lists/*

########################################
#### get and compile OPENCV
########################################
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    unzip ${OPENCV_VERSION}.zip && \
    mkdir /opencv-${OPENCV_VERSION}/cmake_binary && \
    cd /opencv-${OPENCV_VERSION}/cmake_binary && \
    cmake -DBUILD_TIFF=ON \
        -DBUILD_opencv_java=ON \
        -DWITH_CUDA=OFF \
        -DWITH_OPENGL=ON \
        -DWITH_OPENCL=ON \
        -DWITH_IPP=ON \
        -DWITH_TBB=ON \
        -DWITH_EIGEN=ON \
        -DWITH_V4L=ON \
        -DBUILD_TESTS=OFF \
        -DBUILD_PERF_TESTS=OFF \
        -DCMAKE_BUILD_TYPE=RELEASE \
        -DCMAKE_INSTALL_PREFIX=$(python3 -c "import sys; print(sys.prefix)") \
        -DPYTHON_EXECUTABLE=$(which python3) \
		-DPYTHON2_EXECUTABLE=/usr/bin/python2.7 \
        -DPYTHON_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -DPYTHON_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
        .. && \
    make -j $(($(nproc) + 1)) && \
    make install && \
    rm /${OPENCV_VERSION}.zip && \
    rm -r /opencv-${OPENCV_VERSION}

RUN python3 -m pip install numpy && \
    python3 -m pip install opencv-python

CMD ["/bin/bash"]
