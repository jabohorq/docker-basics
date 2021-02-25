# LAMMPS WITTIN DOCKER

## Docker with jupyter
### Dockerfile
```
FROM python:3.7.3-slim
LABEL maintainer="JBB"
LABEL version="0.1"
LABEL description="Noteboook and data (.csv file) 

WORKDIR /data

COPY . /data

RUN pip install numpy pandas==0.24.2 seaborn jupyter

EXPOSE 8888

CMD ["jupyter","notebook","--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```
### Buid image
`docker build -t devenv .`

### run and go to jupyter. Also share a volume with the host
`docker container run -ti -v path/2/sharedir:/data -p 8888:8888 devenv`


## Pull LAMMPS image docker https://hub.docker.com/r/costrouc/lammps-cython
`docker pull costrouc/lammps-cython`
### Construct a new container using the previous lammps box
```
FROM costrouc/lammps-cython
LABEL maintainer="JBB"
LABEL version="0.1"
LABEL description="LAMMPS, numpy, pandas, seaborn, jupyter noteboook 

WORKDIR /data

COPY . /data

RUN pip install numpy pandas==0.24.2 seaborn jupyter

EXPOSE 8888

CMD ["jupyter","notebook","--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```
### Build container
`docker build -t pylammp .`
### Run container using jupyter and with access to a local directory 
`docker container run -ti -v /Users/32b/Documents/nuclearDFT-MD/lammps/docker:/data -p 8888:8888 pylammp`
