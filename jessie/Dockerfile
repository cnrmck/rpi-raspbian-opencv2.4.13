ARG RASPBIAN_VERSION=jessie
FROM resin/rpi-raspbian:$RASPBIAN_VERSION

ARG RASPBIAN_VERSION
ARG DEBIAN_FRONTEND=noninteractive

# install opencv2 as detailied in: https://www.pyimagesearch.com/2015/02/23/install-opencv-and-python-on-your-raspberry-pi-2-and-b/

# update apt
RUN apt-get update \
	&& apt-get install -y --no-install-recommends apt-utils \
	# install necessary build tools \
	&& apt-get -qy install build-essential cmake pkg-config unzip wget \
	# install necessary libraries \
	&& apt-get -qy install \
		libjpeg-dev \
		libtiff5-dev \
		libjasper-dev \
		libpng12-dev \
		libavcodec-dev \
		libavformat-dev \
		libswscale-dev \
		libv4l-dev \
		libxvidcore-dev \
		libx264-dev \
	#Had to break the install into chunks as the deps wouldn't resolve.  \
	&& apt-get -qy install \
		libgtk2.0-dev \
		libgtk-3-dev \
		libatlas-base-dev \
		gfortran \
		python2.7-dev \
		python3-dev \
		python-pip \
		python-numpy \
		python3-pip \
		python3-numpy \
		libraspberrypi0 \
		python-setuptools \
		python3-setuptools \
	# cleanup apt. \
	&& apt-get purge -y --auto-remove \
	&& rm -rf /var/lib/apt/lists/*

ARG OPENCV_VERSION=2.4.13
ENV OPENCV_VERSION $OPENCV_VERSION

	# download latest source & contrib
RUN	cd /tmp \
	&& wget -O opencv-$OPENCV_VERSION.zip http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/$OPENCV_VERSION/opencv-$OPENCV_VERSION.zip/download \
	&& unzip opencv-$OPENCV_VERSION.zip \
	&& cd opencv-$OPENCV_VERSION \
	# build opencv \
	&& cd /tmp/opencv-$OPENCV_VERSION \
	&& mkdir build \
	&& cd build \
	&& cmake -v -D CMAKE_BUILD_TYPE=RELEASE \
		# added this option to try to handle fatal error: stdlib.h \
	  -D ENABLE_PRECOMPILED_HEADERS=OFF \
		-D CMAKE_INSTALL_PREFIX=/usr/local \
		-D INSTALL_C_EXAMPLES=ON \
		-D BUILD_PYTHON_SUPPORT=ON \
		-D BUILD_NEW_PYTHON_SUPPORT=ON \
		-D INSTALL_PYTHON_EXAMPLES=ON \
		-D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv_contrib-$OPENCV_VERSION/modules \
		-D BUILD_EXAMPLES=ON .. \
	&& make -j4  \
	&& make \
	&& make install \
	# && ldconfig \
	&& make clean \
	# cleanup source \
	&& cd / \
	&& rm -rf /tmp/* \
	&& pip install imutils picamera \
	&& pip3 install imutils picamera

ARG VCS_REF
ARG BUILD_DATE
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/cnrmck/rpi-raspbian-opencv2.4"

CMD ["/bin/bash"]
