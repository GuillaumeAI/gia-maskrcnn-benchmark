ARG CUDA="10.2"
ARG CUDNN="7"

FROM jgwill/py-cuda-conda:latest
#FROM nvidia/cuda:${CUDA}-cudnn${CUDNN}-devel-ubuntu16.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections


#RUN nvcc --version
#RUN ls /usr/local/cuda/include
#RUN cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
#RUN sleep 3

# install basics
RUN apt-get update -y \
 && apt-get install -y apt-utils git curl ca-certificates bzip2 cmake tree htop bmon iotop g++ \
 && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev






RUN conda install -y ipython
RUN pip install requests ninja yacs cython matplotlib opencv-python tqdm

# Install PyTorch 1.0 Nightly
ARG CUDA
RUN conda install pytorch-nightly -c pytorch \
 && conda clean -ya

# Install TorchVision master
RUN git clone https://github.com/pytorch/vision.git \
 && cd vision \
 && python setup.py install

# install pycocotools
RUN git clone https://github.com/cocodataset/cocoapi.git \
 && cd cocoapi/PythonAPI \
 && python setup.py build_ext install

# install apex
RUN git clone https://github.com/NVIDIA/apex.git \
 && cd apex \
 && python setup.py install --cuda_ext --cpp_ext

# install PyTorch Detection
ARG FORCE_CUDA="1"
ENV FORCE_CUDA=${FORCE_CUDA}
RUN git clone https://github.com/facebookresearch/maskrcnn-benchmark.git \
 && cd maskrcnn-benchmark \
 && python setup.py build develop

WORKDIR /maskrcnn-benchmark
