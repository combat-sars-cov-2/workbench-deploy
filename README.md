# COMBAT-SARS-CoV-2 Workbench

The COMBAT-SARS-CoV-2 Workbench is a all-included environment for SARS-CoV-2 genome data analysis. Users can load data and metadata,
and run analyses including:

1. genome consensus reconstruction
2. variant identification
3. SARS-CoV-2 lineage typing (with pangolin) and clade typing (with nextclade)

Data generated from these analyses can be used downstream or shared with databases like GISAID or NCBI's Genbank.

The workbench uses Docker and docker compose to hide the complexity of the software.

## Up and running

### Prerequisites

- [`docker`](https://docs.docker.com/install/)
- [`docker-compose`](https://docs.docker.com/compose/)

### Specs

This setup was tested on a VM with the following specs.

- 32G RAM
- 4 vCPU
- 250G Disk

### Using docker-compose

**Assumption :**

- You have [`docker`](https://docs.docker.com/install/) and [`docker-compose`](https://docs.docker.com/compose/) installed on destination instance/VM.

>#### NOTE:
>You can use [scripts](scripts) to install docker and deploy this stack.

```sh
ssh USER@REMOTE.SERVER
```

```sh
git clone https://github.com/COMBAT-SARS-COV-2/irida-galaxy-deploy.git ; cd irida-galaxy-deploy
```

```sh
docker-compose -f stack.yml up -d
```

>#### NOTE:
>This will take a couple of minutes.. :watch: :coffee:

Upon completion, point your browser to:

- [REMOTE.SERVER:8080/irida/](http://REMOTE.SERVER) to access the main web interface
- [REMOTE.SERVER:9090](http://REMOTE.SERVER:9090/) to access the Galaxy workflow execution environment

The default administrator **username and password** are:

- **`admin:password1`** for the main (IRIDA-based) web interface
- **`admin:admin`** for Galaxy

## Openstack platform installation

Workbench code is in GitHub - pvanheus/docker-galaxy-stable

### 1. Checkout the galaxy docker repository
```sh
git clone https://github.com/pvanheus/docker-galaxy-stable
cd docker-galaxy-stable/compose
docker-compose -f docker-compose.yml -f docker-compose.singularity.yml -f docker-compose.irida.yml up
```

### 2. Install the workflows & tools

```shell
cd docker-galaxy-stable/compose/galaxy-configurator/templates/irida/plugins
jar tf galaxy-configurator/templates/irida/plugins/sarscov2-artic-illumina-pipeline-plugin-0.1.10.jar
shed-tools install -g http://localhost:90 -a fakekey -t tools.yml
```






