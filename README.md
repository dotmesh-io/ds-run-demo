# ds run demo

**NOTE: This demo requires a runner with a GPU**

To run this using `ds run`, try the following:

## Setup

Make a new project in Dotscience:

```bash
ds project create circleci
```

## Create a runner

```bash
ds runner create gpu-runner-x -s 100 -t gpu-nvidia-runtime
```

It should then display setup instructions (just with your own details and Dotscience environment):

```bash
Runner ID:        207142bc-da9f-4ca1-a58e-f06a3c0ad998
Runner API token: [YOUR TOKEN]

To start a runner, run:

docker pull quay.io/dotmesh/dotscience-runner:0.2.1 && \
docker run --name dotscience-runner -d -e TOKEN=[YOUR TOKEN] \
--restart always -v /var/run/docker.sock:/var/run/docker.sock \
-v dotscience-task-spool:/spool \
quay.io/dotmesh/dotscience-runner:0.2.1 ds-runner run --addr cloud.dotscience.net:8800 
```

You can copy/paste the output from `ds runner create` command into a VM that has GPU and Docker installed.

To view available runners, use `ds runner ls` command:

```bash
$ ds runner ls
ID                                     NAME                RUNNING TASKS       STATUS              TYPE                 AGE
207142bc-da9f-4ca1-a58e-f06a3c0ad998   gpu-runner-x        0                   offline             gpu-nvidia-runtime   13 minutes
c6d6f46f-4a32-4a9d-b649-acc28b282bc3   worker-x            0                   online              CPU                  About an hour
```

<!-- ## upload signnames.csv

Using the Dotscience web UI, drag and drop the signnames.csv file from the root of this repo into the project resources in Dotscience. -->

## fetching data

```
ds run --verbose --nvidia . $PROJECT quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 -- bash get-data.sh
```

## train model

```
ds run --verbose --nvidia . $PROJECT quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 -- python train.py
```

## check dotscience

You should see provenance & metrics data in Dotscience!
