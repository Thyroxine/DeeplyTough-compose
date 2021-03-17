How to run with DeeplyTough with docker-compose

## Docker

1. Install [Docker Engine](https://docs.docker.com/engine/install/) 19 or later
2. Install [Docker Compose](https://docs.docker.com/compose/install/)
3. Install [NVIDIA Container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)
4. Add your user to docker group
```bash
usermod -aG docker $USER
```
Use pacman and AUR if installing for Arch linux.

Test you installation by
```bash
docker run --rm --gpus all nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04 nvidia-smi
```
You should see all your GPUs in `nvidia-smi`.

## Installing source code
```bash
git clone https://github.com/BenevolentAI/DeeplyTough
```

## Build container
```bash
docker-compose build
```

## Download datasets
```bash
mkdir -p datasets
export STRUCTURE_DATA_DIR=$(pwd)/datasets
./DeeplyTough/datasets_downloader.sh 
```
Wait some time for downloading....

## Create data volumes
Create results folder
```bash
mkdir -p results
```
If you need any of results from repository, just copy them to this folder.
If you need custom network models, create `networks` folder and uncomment the corresponding volume in YAML file.

## Run interactive version
Execute any commands interactively.
```bash
docker-compose run --rm deeplytough
source activate deeplytough
python <your command>
```


## Run batch version
```bash
docker-compose -f docker-compose-batch.yml run --rm deeplytough
```
Edit command in YAML file for you needs.

## Run as a service
You can run program in background, and it will restart automatically if failed.
### Start execution
```bash
docker-compose -f docker-compose-batch.yml up -d
```
### View logs
```bash
docker-compose logs
```
### Stop execution
```bash
docker-compose -f docker-compose-batch.yml down
```

