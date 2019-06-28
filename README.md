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

## Setting up GitHub authentication to clone repository

Dotscience can use SSH based authentication which is compatible with all source control management solutions. For the sake of simplicity, we will use GitHub.

To generate a new private/public SSH key pair, use `ds secret generate --name [NAME]` command:

```bash
$ ds secret generate --name deployment-key
Public key: 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC67QalGM8aAcynRNqtizyn8ZKDuVE7cue63DRPfdo+NdNArZvAhp0wS+yREpomh6XtDhTXhgZeuzJLbfBPPsHBmLx+kDR0TQh8Y5ISoEjIGQHOuUPwlsIrD2JwVI4AheCikKIVIJU4UIuvZgNErJo/3zeCkMVDgMVrGrCDQVh/Eanxm9VKid5YFY3no4j88Nf3KwCtK0fMo93xNDS35RvezjuKCmxg91hnleDBozKNpck19uPk5Ww957QmwQYNEWKVB3xoa/SKUSxSksJJizhuos1vrooG7EX8b3JoIKyD/i92pbSM6mMeF0FmBuRWEU4EMsgbyFtp4S44u6utxaNF
```

Now, you can copy/paste this key into your Github repository settings under `https://github.com/<username or org name>/<repo name>/settings/keys`.

Once it's added, Dotscience will be able to clone this repository. You can add multiple keys and remove them if they are not needed anymore. Keys can be attached to a specific project or for any project in the account (if you don't set project ID when creating a secret).

## Fetching data

Every `ds run` command will need a project name. If you forgot your project name, type `ds project ls`: 

```bash
$ ds project ls
ID                                     NAME                DOTS                                   COLLABORATORS       AGE
ac6d7708-6988-4b49-b922-f3be7bdeaaf7   circleci            8c35ec84-9382-4263-a7e4-2f98885540e6                       3 hours
```

```bash
ds run --project-name circleci --nvidia --image quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 --repo git@github.com:dotmesh-io/ds-run-demo.git --ref master bash ds-run-demo/get-data.sh
```

> Note that the actual command can be specified either with `-c` flags or just positional arguments `bash ds-run-demo/get-data.sh`. You will need to specify a repository name as Dotscience clones repository into a subdirectory named after the repository. Any changes to this repository directory will be overwritten during next checkout. 

## Train model

```bash
ds run --project-name circleci --nvidia --image quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 --repo git@github.com:dotmesh-io/ds-run-demo.git --ref master bash ds-run-demo/train.sh
```

```bash
ds run --project-name run-demo --nvidia --image quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 --repo git@github.com:dotmesh-io/ds-run-demo.git --ref master python ds-run-demo/train.py
```

```bash
# ds run --project-name circleci --nvidia --image quay.io/dotmesh/dotscience-tensorflow-opencv:19.02-py3 --repo git@github.com:dotmesh-io/ds-run-demo.git --ref master python ds-run-demo/train.py
```

## Check Dotscience

You should see provenance & metrics data in Dotscience!
