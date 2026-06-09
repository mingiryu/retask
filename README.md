# retask
Tasks for setting up and connecting to remote compute instances (AWS EC2, Datacrunch, RunPod).

## Dependencies

- [Taskfile](https://taskfile.dev/)

```sh
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d -b ~/bin
```

## Setup

Copy the appropriate env template for your instance type and fill in the IP address:

| Template | Use case |
|---|---|
| `.env.dist` | Default / root-access instances |
| `.env.aws` | AWS EC2 (ec2-user, Amazon Linux) |
| `.env.ubuntu` | Ubuntu instances |

```sh
cp .env.dist .env
# edit .env with your SSH_IP_ADDRESS and GIT_URI
```

## Common tasks

```sh
task setup        # Full instance setup: apt, clone, aws CLI, taskfile
task ssh          # SSH into the instance
task ssh:add      # Add SSH key to agent (run once per session)
task tunnel       # Forward SSH_TUNNEL_PORT to localhost (e.g. for Jupyter)
```

```sh
task scp -- file.txt          # Upload file to $REMOTE_HOME
task scp:down -- file.txt     # Download file from $REMOTE_HOME
```

```sh
task aws:config   # Copy local ~/.aws credentials to remote
task task:file    # Re-deploy the remote Taskfile
```

## Remote taskfiles

The remote instance gets one of these deployed as `~/Taskfile.yaml` (controlled by `REMOTE_TASK_FILE`):

| File | Use case |
|---|---|
| `taskfiles/Python.yaml` | Poetry + Jupyter Lab |
| `taskfiles/Amazon.yaml` | Amazon Linux — Docker, NVME mount, tools |
| `taskfiles/Ubuntu.yaml` | Ubuntu — Docker, NVME mount, tools |
