FROM --platform=linux/arm64 ubuntu:20.04@sha256:64162ac111b666daf1305de1888eb67a3033f62000f5ff781fe529aff8f88b09

ENV TZ=Asia/Kolkata

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update

WORKDIR /

# Install v4l2
RUN apt install -y v4l-utils

# Install Gstreamer
RUN apt install -y libgstreamer1.0-dev
RUN apt install -y libgstreamer-plugins-base1.0-dev
RUN apt install -y libgstreamer-plugins-bad1.0-dev
RUN apt install -y gstreamer1.0-plugins-base
RUN apt install -y gstreamer1.0-plugins-good
RUN apt install -y gstreamer1.0-plugins-bad
RUN apt install -y gstreamer1.0-plugins-ugly
RUN apt install -y gstreamer1.0-tools

# Install CV2
RUN apt install -y build-essential
RUN apt install -y cmake
RUN apt install -y git
RUN apt install -y libgtk2.0-dev
RUN apt install -y pkg-config 
RUN apt install -y libavcodec-dev 
RUN apt install -y libavformat-dev 
RUN apt install -y libswscale-dev
# RUN apt install -y python-dev 
# RUN apt install -y python-numpy 
RUN apt install -y python3 
RUN apt install -y python3-pip 
RUN apt install -y wget 
RUN apt install -y python3.8-dev
RUN apt install -y python3-dev
RUN pip3 install numpy
RUN apt autoremove -y python2
RUN mkdir ws 

WORKDIR /ws

RUN git clone https://github.com/opencv/opencv.git

WORKDIR /ws/opencv

RUN python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())" 
RUN python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
RUN python3 -c "import numpy as np;print(np.get_include())"


RUN mkdir build && cd build && \
    cmake -DBUILD_PYTHON3=ON \
    -DWITH_GSTREAMER=ON \
    -DPYTHON3_EXECUTABLE=/usr/bin/python3 \
    -DPYTHON3_INCLUDE_DIR=/usr/include/python3.8/ \
    -DBUILD_NEW_PYTHON_SUPPORT=ON \
    -DBUILD_opencv_python3=ON \
    -DHAVE_opencv_python3=ON \
    -DPYTHON3_DEFAULT_EXECUTABLE=/usr/bin/python3 \
    -DPYTHON3_LIBRARY=/usr/lib/aarch64-linux-gnu/libpython3.8.so \
    -DBUILD_PYTHON3=ON \
    -DBUILD_OPENCV_PYTHON2=OFF \
    -DBUILD_SHARED_LIBS=ON \
    -DPYTHON3_LIBRARIES=/usr/lib/python3/dist-packages \
    -DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/local/lib/python3.8/dist-packages/numpy/core/include \
    -DWITH_FFMPEG=OFF \
    -DWITH_V4L=ON \
    ..


WORKDIR /ws/opencv/build 

RUN make -j6 && make install && ldconfig 

WORKDIR /ws

RUN mkdir images

# WORKDIR /usr/local/lib/python3.8/site-packages/cv2/python-3.8/

RUN wget https://raw.githubusercontent.com/hexagun/DEV/main/testcam/testcam.py

RUN pwd

RUN ls -la

RUN python3 testcam.py 

# docker build --platform linux/arm64 -t  hexagun/motionapp:test .
