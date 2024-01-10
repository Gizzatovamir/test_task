FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get update && apt install -y git python3-pip ffmpeg libsm6 libxext6 g++ python3-dev nvidia-container-toolkit && pip install tyro open_clip_torch wandb h5py openai hydra-core && pip install torch==2.1.1 torchvision==0.16.1 torchaudio==2.1.1 --index-url https://download.pytorch.org/whl/cu118

RUN git clone https://github.com/krrish94/chamferdist.git &&  \
    git clone https://github.com/gradslam/gradslam.git &&  \
    git clone https://github.com/IDEA-Research/Grounded-Segment-Anything.git && \
    git clone https://github.com/haotian-liu/LLaVA.git

RUN cd /chamferdist && git checkout 255b7108a70f96d22d6a7e2258d03888fb3c8b89 && pip install . &  \
    cd /gradslam && git checkout conceptfusion && pip install . &&  \
    pip install LLaVa

ENV REPLICA_ROOT=/dataset/
ENV GSA_PATH=/Grounded-Segment-Anything
ENV LLAVA_MODEL_PATH=/models/
ENV CG_FOLDER=/concept-graphs/
ENV REPLICA_CONFIG_PATH=/concept-graphs/conceptgraph/dataset/dataconfigs/replica/replica.yaml
ENV SCENE_NAME=room0
COPY concept-graphs concept-graphs
WORKDIR concept-graphs
RUN pip install .

CMD  python3 conceptgraph/scripts/run_slam_rgb.py \
    --dataset_root $REPLICA_ROOT \
    --dataset_config $REPLICA_CONFIG_PATH \
    --scene_id $SCENE_NAME \
    --image_height 480 \
    --image_width 640 \
    --stride 5