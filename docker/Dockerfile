FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04

# Install basics
RUN apt-get update -y \
 && apt-get install -y apt-utils git curl cmake tree htop bmon iotop \
 && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev \
 && rm -rf /var/lib/apt/lists/*

# Install Python 3
RUN apt-get update -y \
 && apt-get install -y python3-dev python3-pip

RUN pip3 install requests ninja yacs cython matplotlib opencv-python tqdm

# Install Pytorch and Torchvision
RUN pip3 install https://download.pytorch.org/whl/cu100/torch-1.1.0-cp36-cp36m-linux_x86_64.whl
RUN pip3 install https://download.pytorch.org/whl/cu100/torchvision-0.3.0-cp36-cp36m-linux_x86_64.whl

# Install pycocotools
RUN git clone https://github.com/cocodataset/cocoapi.git \
 && cd cocoapi/PythonAPI \
 && python3 setup.py build_ext install

# Install PyTorch Detection
ARG FORCE_CUDA="1"
ENV FORCE_CUDA=${FORCE_CUDA}

RUN git clone https://github.com/lucymaurice/maskrcnn-benchmark.git \
 && cd maskrcnn-benchmark \
 && python3 setup.py build develop

WORKDIR /maskrcnn-benchmark

RUN pip3 install jupyter

RUN jupyter notebook --generate-config

COPY "jupyter_notebook_config.py" ${CONFIG_PATH}

ENTRYPOINT ["sh", "-c", "jupyter notebook --allow-root -y --no-browser --ip=0.0.0.0 --config=${CONFIG_PATH}"]

