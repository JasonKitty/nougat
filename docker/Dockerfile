FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04
# replace CUDA version to your CUDA version.
# You can check your CUDA version with below.
# nvcc -V

RUN apt-get update
RUN apt-get install -y python3
RUN apt-get -y install python3-pip git
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
# replace CUDA version to your CUDA version.

RUN mkdir workspace
WORKDIR /workspace

RUN pip3 install fastapi uvicorn[standard] fsspec[http]==2023.1.0
COPY nougat /workspace/nougat

WORKDIR /workspace/nougat
RUN pip install transformers>=4.25.1
RUN pip install timm==0.5.4
RUN pip install orjson opencv-python-headless datasets[vision] lightning==2.0.0 nltk python-Levenshtein sentencepiece sconf>=0.2.3 albumentations>=1.0.0 pypdf>=3.1.0 pypdfium2

RUN python3 setup.py install

RUN apt-get update && \
    apt-get install -y libgl1-mesa-glx

RUN apt-get install -y libglib2.0-0

RUN pip install -U tensorboard

EXPOSE 8503

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8503"]
# Run this using 'docker run -it -d -p <YOUR PORT>:8503 --gpus all <IMAGE NAME>
