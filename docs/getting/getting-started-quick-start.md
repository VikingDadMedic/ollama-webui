---
title: Quick Start
source: getting-started/quick-start/index.html
word_count: 2699
code_blocks: 0
quality_score: 80.0
extracted: 2025-12-14T09:48:32.345233
---

# Quick Start

Sponsored by Open WebUI Inc.

[![Open WebUI Inc.](/sponsors/banners/openwebui-banner.png)![Open WebUI Inc.](/sponsors/banners/openwebui-banner-mobile.png)](<https://docs.openwebui.com/enterprise>)

Upgrade to a licensed plan for enhanced capabilities, including custom theming and branding, and dedicated support.

info

**Important Note on User Roles and Privacy:**

 * **Admin Creation:** The first account created on Open WebUI gains **Administrator privileges** , controlling user management and system settings.
 * **User Registrations:** Subsequent sign-ups start with **Pending** status, requiring Administrator approval for access.
 * **Privacy and Data Security:** **All your data** , including login details, is **locally stored** on your device. Open WebUI ensures **strict confidentiality** and **no external requests** for enhanced privacy and security. 
 * **All models are private by default.** Models must be explicitly shared via groups or by being made public. If a model is assigned to a group, only members of that group can see it. If a model is made public, anyone on the instance can see it.

Choose your preferred installation method below:

 * **Docker:** **Officially supported and recommended for most users**
 * **Python:** Suitable for low-resource environments or those wanting a manual setup
 * **Kubernetes:** Ideal for enterprise deployments that require scaling and orchestration

 * Docker
 * Python
 * Kubernetes
 * Third Party

 * Docker
 * Docker Compose
 * Extension
 * Podman
 * Kube Play
 * Swarm
 * WSL

## Quick Start with Docker 🐳

Follow these steps to install Open WebUI with Docker.

## Step 1: Pull the Open WebUI Image

Start by pulling the latest Open WebUI Docker image from the GitHub Container Registry.
[code] 
 docker pull ghcr.io/open-webui/open-webui:main 

[/code]

### Slim Image Variant

For environments with limited storage or bandwidth, Open WebUI offers slim image variants that exclude pre-bundled models. These images are significantly smaller but download required models (whisper, embedding models) on first use.
[code] 
 docker pull ghcr.io/open-webui/open-webui:main-slim 

[/code]

## Step 2: Run the Container

Run the container with default settings. This command includes a volume mapping to ensure persistent data storage.
[code] 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main 

[/code]

To use the slim variant instead:
[code] 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main-slim 

[/code]

### Important Flags

 * **Volume Mapping (`-v open-webui:/app/backend/data`)**: Ensures persistent storage of your data. This prevents data loss between container restarts.
 * **Port Mapping (`-p 3000:8080`)**: Exposes the WebUI on port 3000 of your local machine.

### Using GPU Support

For Nvidia GPU support, add `--gpus all` to the `docker run` command:
[code] 
 docker run -d -p 3000:8080 --gpus all -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:cuda 

[/code]

#### Single-User Mode (Disabling Login)

To bypass the login page for a single-user setup, set the `WEBUI_AUTH` environment variable to `False`:
[code] 
 docker run -d -p 3000:8080 -e WEBUI_AUTH=False -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main 

[/code]

warning

You cannot switch between single-user mode and multi-account mode after this change.

#### Advanced Configuration: Connecting to Ollama on a Different Server

To connect Open WebUI to an Ollama server located on another host, add the `OLLAMA_BASE_URL` environment variable:
[code] 
 docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=https://example.com -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main 

[/code]

## Access the WebUI

After the container is running, access Open WebUI at:

<http://localhost:3000>

For detailed help on each Docker flag, see [Docker's documentation](<https://docs.docker.com/engine/reference/commandline/run/>).

## Updating

To update your local Docker installation to the latest version, you can either use **Watchtower** or manually update the container.

### Option 1: Using Watchtower

With [Watchtower](<https://containrrr.dev/watchtower/>), you can automate the update process:
[code] 
 docker run --rm --volume /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once open-webui 

[/code]

_(Replace`open-webui` with your container's name if it's different.)_

### Option 2: Manual Update

 1. Stop and remove the current container:
[code] docker rm -f open-webui 

[/code]

 2. Pull the latest version:
[code] docker pull ghcr.io/open-webui/open-webui:main 

[/code]

 3. Start the container again:
[code] docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main 

[/code]

Both methods will get your Docker instance updated and running with the latest build.

# Docker Compose Setup

Using Docker Compose simplifies the management of multi-container Docker applications.

Docker Compose requires an additional package, `docker-compose-v2`.

warning

**Warning:** Older Docker Compose tutorials may reference version 1 syntax, which uses commands like `docker-compose build`. Ensure you use version 2 syntax, which uses commands like `docker compose build` (note the space instead of a hyphen).

## Example `docker-compose.yml`

Here is an example configuration file for setting up Open WebUI with Docker Compose:
[code] 
 services: 
 openwebui: 
 image: ghcr.io/open-webui/open-webui:main 
 ports: 
 - "3000:8080" 
 volumes: 
 - open-webui:/app/backend/data 
 volumes: 
 open-webui: 

[/code]

### Using Slim Images

For environments with limited storage or bandwidth, you can use the slim image variant that excludes pre-bundled models:
[code] 
 services: 
 openwebui: 
 image: ghcr.io/open-webui/open-webui:main-slim 
 ports: 
 - "3000:8080" 
 volumes: 
 - open-webui:/app/backend/data 
 volumes: 
 open-webui: 

[/code]

note

**Note:** Slim images download required models (whisper, embedding models) on first use, which may result in longer initial startup times but significantly smaller image sizes.

## Starting the Services

To start your services, run the following command:
[code] 
 docker compose up -d 

[/code]

## Helper Script

A useful helper script called `run-compose.sh` is included with the codebase. This script assists in choosing which Docker Compose files to include in your deployment, streamlining the setup process.

* * *

note

**Note:** For Nvidia GPU support, you change the image from `ghcr.io/open-webui/open-webui:main` to `ghcr.io/open-webui/open-webui:cuda` and add the following to your service definition in the `docker-compose.yml` file:
[code] 
 deploy: 
 resources: 
 reservations: 
 devices: 
 - driver: nvidia 
 count: all 
 capabilities: [gpu] 

[/code]

This setup ensures that your application can leverage GPU resources when available.

# Docker Desktop Extension

Docker has released an Open WebUI Docker extension that uses Docker Model Runner for inference. You can read their getting started blog here: [Run Local AI with Open WebUI + Docker Model Runner](<https://www.docker.com/blog/open-webui-docker-desktop-model-runner/>)

You can find troubleshooting steps for the extension in their Github repository: [Open WebUI Docker Extension - Troubleshooting](<https://github.com/rw4lll/open-webui-docker-extension?tab=readme-ov-file#troubleshooting>)

While this is an amazing resource to try out Open WebUI with little friction, it is not an officially supported installation method - you may run into unexpected bugs or behaviors while using it. For example, you are not able to log in as different users in the extension since it is designed to be for a single local user. If you run into issues using the extension, please submit an issue on the extension's [Github repository](<https://github.com/rw4lll/open-webui-docker-extension>).

# Using Podman

Podman is a daemonless container engine for developing, managing, and running OCI Containers.

## Basic Commands

 * **Run a Container:**
[code] podman run -d --name openwebui -p 3000:8080 -v open-webui:/app/backend/data ghcr.io/open-webui/open-webui:main 

[/code]

 * **List Running Containers:**
[code] podman ps 

[/code]

## Networking with Podman

If networking issues arise, use slirp4netns to adjust the pod's network settings to allow the container to access your computer's ports.

Ensure you have [slirp4netns installed](<https://github.com/rootless-containers/slirp4netns?tab=readme-ov-file#install>), remove the previous container if it exists using `podman rm`, and start a new container with
[code] 
 podman run -d --network=slirp4netns:allow_host_loopback=true --name openwebui -p 3000:8080 -v open-webui:/app/backend/data ghcr.io/open-webui/open-webui:main 

[/code]

If you are using Ollama from your computer (not running inside a container),

Once inside open-webui, navigate to Settings > Admin Settings > Connections and create a new Ollama API connection to `http://10.0.2.2:[OLLAMA PORT]`. By default, the Ollama port is 11434.

Refer to the Podman [documentation](<https://podman.io/>) for advanced configurations.

# Podman Kube Play Setup

Podman supports Kubernetes like-syntax for deploying resources such as pods, volumes without having the overhead of a full Kubernetes cluster. [More about Kube Play](<https://docs.podman.io/en/latest/markdown/podman-kube-play.1.html>).

If you don't have Podman installed, check out [Podman's official website](<https://podman.io/docs/installation>).

## Example `play.yaml`

Here is an example of a Podman Kube Play file to deploy:
[code] 
 apiVersion: v1 
 kind: Pod 
 metadata: 
 name: open-webui 
 spec: 
 containers: 
 - name: container 
 image: ghcr.io/open-webui/open-webui:main 
 ports: 
 - name: http 
 containerPort: 8080 
 hostPort: 3000 
 volumeMounts: 
 - mountPath: /app/backend/data 
 name: data 
 volumes: 
 - name: data 
 persistentVolumeClaim: 
 claimName: open-webui-pvc 
 --- 
 apiVersion: v1 
 kind: PersistentVolumeClaim 
 metadata: 
 name: open-webui-pvc 
 spec: 
 accessModes: 
 - ReadWriteOnce 
 resources: 
 requests: 
 storage: 5Gi 

[/code]

## Starting

To start your pod, run the following command:
[code] 
 podman kube play ./play.yaml 

[/code]

## Using GPU Support

For Nvidia GPU support, you need to replace the container image with `ghcr.io/open-webui/open-webui:cuda` and need to specify the device (GPU) required in the pod resources limits as followed:
[code] 
 [...] 
 resources: 
 limits: 
 nvidia.com/gpu=all: 1 
 [...] 

[/code]

important

To successfully have the open-webui container access the GPU(s), you will need to have the Container Device Interface (CDI) for the GPU you wish to access installed in your Podman Machine. You can check [Podman GPU container access](<https://podman-desktop.io/docs/podman/gpu>).

## Docker Swarm

This installation method requires knowledge on Docker Swarms, as it utilizes a stack file to deploy 3 seperate containers as services in a Docker Swarm.

It includes isolated containers of ChromaDB, Ollama, and OpenWebUI. Additionally, there are pre-filled [Environment Variables](<https://docs.openwebui.com/getting-started/env-configuration>) to further illustrate the setup.

Choose the appropriate command based on your hardware setup:

 * **Before Starting** :

Directories for your volumes need to be created on the host, or you can specify a custom location or volume.

The current example utilizes an isolated dir `data`, which is within the same dir as the `docker-stack.yaml`.

 * **For example** :
[code] mkdir -p data/open-webui data/chromadb data/ollama 

[/code]

 * **With GPU Support** :

#### Docker-stack.yaml
[code] 
 version: '3.9' 

 services: 
 openWebUI: 
 image: ghcr.io/open-webui/open-webui:main 
 depends_on: 
 - chromadb 
 - ollama 
 volumes: 
 - ./data/open-webui:/app/backend/data 
 environment: 
 DATA_DIR: /app/backend/data 
 OLLAMA_BASE_URLS: http://ollama:11434 
 CHROMA_HTTP_PORT: 8000 
 CHROMA_HTTP_HOST: chromadb 
 CHROMA_TENANT: default_tenant 
 VECTOR_DB: chroma 
 WEBUI_NAME: Awesome ChatBot 
 CORS_ALLOW_ORIGIN: "*" # This is the current Default, will need to change before going live 
 RAG_EMBEDDING_ENGINE: ollama 
 RAG_EMBEDDING_MODEL: nomic-embed-text-v1.5 
 RAG_EMBEDDING_MODEL_TRUST_REMOTE_CODE: "True" 
 ports: 
 - target: 8080 
 published: 8080 
 mode: overlay 
 deploy: 
 replicas: 1 
 restart_policy: 
 condition: any 
 delay: 5s 
 max_attempts: 3 

 chromadb: 
 hostname: chromadb 
 image: chromadb/chroma:0.5.15 
 volumes: 
 - ./data/chromadb:/chroma/chroma 
 environment: 
 - IS_PERSISTENT=TRUE 
 - ALLOW_RESET=TRUE 
 - PERSIST_DIRECTORY=/chroma/chroma 
 ports: 
 - target: 8000 
 published: 8000 
 mode: overlay 
 deploy: 
 replicas: 1 
 restart_policy: 
 condition: any 
 delay: 5s 
 max_attempts: 3 
 healthcheck: 
 test: ["CMD-SHELL", "curl localhost:8000/api/v1/heartbeat || exit 1"] 
 interval: 10s 
 retries: 2 
 start_period: 5s 
 timeout: 10s 

 ollama: 
 image: ollama/ollama:latest 
 hostname: ollama 
 ports: 
 - target: 11434 
 published: 11434 
 mode: overlay 
 deploy: 
 resources: 
 reservations: 
 generic_resources: 
 - discrete_resource_spec: 
 kind: "NVIDIA-GPU" 
 value: 0 
 replicas: 1 
 restart_policy: 
 condition: any 
 delay: 5s 
 max_attempts: 3 
 volumes: 
 - ./data/ollama:/root/.ollama 

[/code]

 * **Additional Requirements** :

 1. Ensure CUDA is Enabled, follow your OS and GPU instructions for that.
 2. Enable Docker GPU support, see [Nvidia Container Toolkit](<https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html> " on Nvidia's site.")
 3. Follow the [Guide here on configuring Docker Swarm to with with your GPU](<https://gist.github.com/tomlankhorst/33da3c4b9edbde5c83fc1244f010815c#configuring-docker-to-work-with-your-gpus>)
 * Ensure _GPU Resource_ is enabled in `/etc/nvidia-container-runtime/config.toml` and enable GPU resource advertising by uncommenting the `swarm-resource = "DOCKER_RESOURCE_GPU"`. The docker daemon must be restarted after updating these files on each node.
 * **With CPU Support** :

Modify the Ollama Service within `docker-stack.yaml` and remove the lines for `generic_resources:`
[code] ollama: 
 image: ollama/ollama:latest 
 hostname: ollama 
 ports: 
 - target: 11434 
 published: 11434 
 mode: overlay 
 deploy: 
 replicas: 1 
 restart_policy: 
 condition: any 
 delay: 5s 
 max_attempts: 3 
 volumes: 
 - ./data/ollama:/root/.ollama 

[/code]

 * **Deploy Docker Stack** :
[code] docker stack deploy -c docker-stack.yaml -d super-awesome-ai 

[/code]

## Using Docker with WSL (Windows Subsystem for Linux)

This guide provides instructions for setting up Docker and running Open WebUI in a Windows Subsystem for Linux (WSL) environment.

### Step 1: Install WSL

If you haven't already, install WSL by following the official Microsoft documentation:

[Install WSL](<https://learn.microsoft.com/en-us/windows/wsl/install>)

### Step 2: Install Docker Desktop

Docker Desktop is the easiest way to get Docker running in a WSL environment. It handles the integration between Windows and WSL automatically.

 1. **Download Docker Desktop:** <https://www.docker.com/products/docker-desktop/>

 2. **Install Docker Desktop:** Follow the installation instructions, making sure to select the "WSL 2" backend during the setup process.

### Step 3: Configure Docker Desktop for WSL

 1. **Open Docker Desktop:** Start the Docker Desktop application.

 2. **Enable WSL Integration:**

 * Go to **Settings > Resources > WSL Integration**.
 * Make sure the "Enable integration with my default WSL distro" checkbox is selected.
 * If you are using a non-default WSL distribution, select it from the list.

### Step 4: Run Open WebUI

Now you can run Open WebUI by following the standard Docker instructions from within your WSL terminal.
[code] 
 docker pull ghcr.io/open-webui/open-webui:main 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main 

[/code]

### Important Notes

 * **Run Docker Commands in WSL:** Always run `docker` commands from your WSL terminal, not from PowerShell or Command Prompt.

 * **File System Access:** When using volume mounts (`-v`), make sure the paths are accessible from your WSL distribution.

 * uv
 * Conda
 * Venv
 * Development

### Installation with `uv`

The `uv` runtime manager ensures seamless Python environment management for applications like Open WebUI. Follow these steps to get started:

#### 1\. Install `uv`

Pick the appropriate installation command for your operating system:

 * **macOS/Linux** :
[code] curl -LsSf https://astral.sh/uv/install.sh | sh 

[/code]

 * **Windows** :
[code] powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex" 

[/code]

#### 2\. Run Open WebUI

Once `uv` is installed, running Open WebUI is a breeze. Use the command below, ensuring to set the `DATA_DIR` environment variable to avoid data loss. Example paths are provided for each platform:

 * **macOS/Linux** :
[code] DATA_DIR=~/.open-webui uvx --python 3.11 open-webui@latest serve 

[/code]

 * **Windows** (PowerShell):
[code] $env:DATA_DIR="C:\open-webui\data"; uvx --python 3.11 open-webui@latest serve 

[/code]

# Updating with Python

To update your locally installed **Open-WebUI** package to the latest version using `pip`, follow these simple steps:
[code] 
 pip install -U open-webui 

[/code]

The `-U` (or `--upgrade`) flag ensures that `pip` upgrades the package to the latest available version.

That's it! Your **Open-WebUI** package is now updated and ready to use.

# Install with Conda

 1. **Create a Conda Environment:**
[code] conda create -n open-webui python=3.11 

[/code]

 2. **Activate the Environment:**
[code] conda activate open-webui 

[/code]

 3. **Install Open WebUI:**
[code] pip install open-webui 

[/code]

 4. **Start the Server:**
[code] open-webui serve 

[/code]

# Updating with Python

To update your locally installed **Open-WebUI** package to the latest version using `pip`, follow these simple steps:
[code] 
 pip install -U open-webui 

[/code]

The `-U` (or `--upgrade`) flag ensures that `pip` upgrades the package to the latest available version.

That's it! Your **Open-WebUI** package is now updated and ready to use.

# Using Virtual Environments

Create isolated Python environments using `venv`.

## Venv Steps

 1. **Create a Virtual Environment:**
[code] python3 -m venv venv 

[/code]

 2. **Activate the Virtual Environment:**

 * On Linux/macOS:
[code] source venv/bin/activate 

[/code]

 * On Windows:
[code] venv\Scripts\activate 

[/code]

 3. **Install Open WebUI:**
[code] pip install open-webui 

[/code]

 4. **Start the Server:**
[code] open-webui serve 

[/code]

# Updating with Python

To update your locally installed **Open-WebUI** package to the latest version using `pip`, follow these simple steps:
[code] 
 pip install -U open-webui 

[/code]

The `-U` (or `--upgrade`) flag ensures that `pip` upgrades the package to the latest available version.

That's it! Your **Open-WebUI** package is now updated and ready to use.

### Development Setup

For developers who want to contribute, check the Development Guide in [Advanced Topics](</getting-started/advanced-topics>).

 * Helm

# Helm Setup for Kubernetes

Helm helps you manage Kubernetes applications.

## Prerequisites

 * Kubernetes cluster is set up.
 * Helm is installed.

## Helm Steps

 1. **Add Open WebUI Helm Repository:**
[code] helm repo add open-webui https://open-webui.github.io/helm-charts 
 helm repo update 

[/code]

 2. **Install Open WebUI Chart:**
[code] helm install openwebui open-webui/open-webui 

[/code]

 3. **Verify Installation:**
[code] kubectl get pods 

[/code]

warning

If you intend to scale Open WebUI using multiple nodes/pods/workers in a clustered environment, you need to setup a NoSQL key-value database. There are some [environment variables](<https://docs.openwebui.com/getting-started/env-configuration/>) that need to be set to the same value for all service-instances, otherwise consistency problems, faulty sessions and other issues will occur!

## Access the WebUI

Set up port forwarding or load balancing to access Open WebUI from outside the cluster.

 * Pinokio.computer

### Pinokio.computer Installation

For installation via Pinokio.computer, please visit their website:

<https://pinokio.computer/>

Support for this installation method is provided through their website.

### Additional Third-Party Integrations

 _(Add information about third-party integrations as they become available.)_

## Next Steps

After installing, visit:

 * <http://localhost:3000> to access Open WebUI.
 * or <http://localhost:8080/> when using a Python deployment.

You are now ready to start using Open WebUI!

## Using Open WebUI with Ollama

If you're using Open WebUI with Ollama, be sure to check out our [Starting with Ollama Guide](</getting-started/quick-start/starting-with-ollama>) to learn how to manage your Ollama instances with Open WebUI.

## Join the Community

Need help? Have questions? Join our community:

 * [Open WebUI Discord](<https://discord.gg/5rJgQTnV4s>)
 * [GitHub Issues](<https://github.com/open-webui/open-webui/issues>)

Stay updated with the latest features, troubleshooting tips, and announcements!