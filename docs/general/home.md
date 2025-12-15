---
title: Open WebUI
source: index.html
word_count: 1111
code_blocks: 0
quality_score: 80.0
extracted: 2025-12-14T09:48:32.471280
---

# Open WebUI

**Open WebUI is an[extensible](<https://docs.openwebui.com/features/plugin/>), feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline.** It supports various LLM runners like **Ollama** and **OpenAI-compatible APIs** , with **built-in inference engine** for RAG, making it a **powerful AI deployment solution**.

Passionate about open-source AI? [Join our team →](<https://careers.openwebui.com/>)

![GitHub stars](https://img.shields.io/github/stars/open-webui/open-webui?style=social) ![GitHub forks](https://img.shields.io/github/forks/open-webui/open-webui?style=social) ![GitHub watchers](https://img.shields.io/github/watchers/open-webui/open-webui?style=social) ![GitHub repo size](https://img.shields.io/github/repo-size/open-webui/open-webui) ![GitHub language count](https://img.shields.io/github/languages/count/open-webui/open-webui) ![GitHub top language](https://img.shields.io/github/languages/top/open-webui/open-webui) ![GitHub last commit](https://img.shields.io/github/last-commit/open-webui/open-webui?color=red) [![Discord](https://img.shields.io/badge/Discord-Open_WebUI-blue?logo=discord&logoColor=white)](<https://discord.gg/5rJgQTnV4s>) [![Image Description](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](<https://github.com/sponsors/tjbck>)

![Open WebUI Demo](/assets/images/demo-f704541c988ae735dde16b8baba17627.png)

tip

**Looking for an[Enterprise Plan](<https://docs.openwebui.com/enterprise>)?** — **[Speak with Our Sales Team Today!](<https://docs.openwebui.com/enterprise>)**

Get **enhanced capabilities** , including **custom theming and branding** , **Service Level Agreement (SLA) support** , **Long-Term Support (LTS) versions** , and **more!**

Sponsored by Open WebUI Inc.

[![Open WebUI Inc.](/sponsors/banners/openwebui-banner.png)![Open WebUI Inc.](/sponsors/banners/openwebui-banner-mobile.png)](<https://docs.openwebui.com/enterprise>)

Upgrade to a licensed plan for enhanced capabilities, including custom theming and branding, and dedicated support.

## Quick Start with Docker 🐳

info

**WebSocket** support is required for Open WebUI to function correctly. Ensure that your network configuration allows WebSocket connections.

**If Ollama is on your computer** , use this command:
[code] 
 docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main 

[/code]

**To run Open WebUI with Nvidia GPU support** , use this command:
[code] 
 docker run -d -p 3000:8080 --gpus all --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:cuda 

[/code]

For environments with limited storage or bandwidth, Open WebUI offers slim image variants that exclude pre-bundled models. These images are significantly smaller but download required models on first use:
[code] 
 docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main-slim 

[/code]

### Open WebUI Bundled with Ollama

This installation method uses a single container image that bundles Open WebUI with Ollama, allowing for a streamlined setup via a single command. Choose the appropriate command based on your hardware setup:

 * **With GPU Support** : Utilize GPU resources by running the following command:
[code] docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama 

[/code]

 * **For CPU Only** : If you're not using a GPU, use this command instead:
[code] docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama 

[/code]

Both commands facilitate a built-in, hassle-free installation of both Open WebUI and Ollama, ensuring that you can get everything up and running swiftly.

After installation, you can access Open WebUI at <http://localhost:3000>. Enjoy! 😄

### Using the Dev Branch 🌙

warning

The `:dev` branch contains the latest unstable features and changes. Use it at your own risk as it may have bugs or incomplete features.

If you want to try out the latest bleeding-edge features and are okay with occasional instability, you can use the `:dev` tag like this:
[code] 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:dev 

[/code]

For the slim variant of the dev branch:
[code] 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:dev-slim 

[/code]

### Updating Open WebUI

To update Open WebUI container easily, follow these steps:

#### Manual Update

Use [Watchtower](<https://containrrr.dev/watchtower>) to update your Docker container manually:
[code] 
 docker run --rm -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once open-webui 

[/code]

#### Automatic Updates

Keep your container updated automatically every 5 minutes:
[code] 
 docker run -d --name watchtower --restart unless-stopped -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --interval 300 open-webui 

[/code]

🔧 **Note** : Replace `open-webui` with your container name if it's different.

## Manual Installation

info

### Platform Compatibility

Open WebUI works on macOS, Linux (x86_64 and ARM64, including Raspberry Pi and other ARM boards like NVIDIA DGX Spark), and Windows.

There are two main ways to install and run Open WebUI: using the `uv` runtime manager or Python's `pip`. While both methods are effective, **we strongly recommend using`uv`** as it simplifies environment management and minimizes potential conflicts.

### Installation with `uv` (Recommended)

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

 * **Windows** :
[code] $env:DATA_DIR="C:\open-webui\data"; uvx --python 3.11 open-webui@latest serve 

[/code]

note

**For PostgreSQL Support:**

The default installation now uses a slimmed-down package. If you need **PostgreSQL support** , install with all optional dependencies:
[code]
 pip install open-webui[all] 

[/code]

### Installation with `pip`

For users installing Open WebUI with Python's package manager `pip`, **it is strongly recommended to use Python runtime managers like`uv` or `conda`**. These tools help manage Python environments effectively and avoid conflicts.

Python 3.11 is the development environment. Python 3.12 seems to work but has not been thoroughly tested. Python 3.13 is entirely untested and some dependencies do not work with Python 3.13 yet—**use at your own risk**.

 1. **Install Open WebUI** :

Open your terminal and run the following command:
[code] pip install open-webui 

[/code]

 2. **Start Open WebUI** :

Once installed, start the server using:
[code] open-webui serve 

[/code]

### Updating Open WebUI

To update to the latest version, simply run:
[code] 
 pip install --upgrade open-webui 

[/code]

This method installs all necessary dependencies and starts Open WebUI, allowing for a simple and efficient setup. After installation, you can access Open WebUI at <http://localhost:8080>. Enjoy! 😄

## Other Installation Methods

We offer various installation alternatives, including non-Docker native installation methods, Docker Compose, Kustomize, and Helm. Visit our [Open WebUI Documentation](<https://docs.openwebui.com/getting-started/>) or join our [Discord community](<https://discord.gg/5rJgQTnV4s>) for comprehensive guidance.

Continue with the full [getting started guide](</getting-started>).

### Desktop App

We also have an **experimental** desktop app, which is actively a **work in progress (WIP)**. While it offers a convenient way to run Open WebUI natively on your system without Docker or manual setup, it is **not yet stable**.

👉 For stability and production use, we strongly recommend installing via **Docker** or **Python (`uv` or `pip`)**.

## Sponsors 🙌

Emerald

* * *

Jade

* * *

[Open WebUI](<https://openwebui.com>)

[![Open WebUI](/sponsors/sponsor.png)On a mission to build the best AI user interface.](<https://openwebui.com>)

We are incredibly grateful for the generous support of our sponsors. Their contributions help us to maintain and improve our project, ensuring we can continue to deliver quality work to our community. Thank you!

## Acknowledgements 🙏

We are deeply grateful for the generous grant support provided by:

[![A16z](/assets/images/a16z-58aeed7bc9f9a7894b5f3e59192d88d3.png)A16z Open Source AI Grant 2025](<https://a16z.com/advancing-open-source-ai-through-benchmarks-and-bold-experimentation/> "A16z Open Source AI Grant 2025")[![Mozilla](/assets/images/mozilla-c6f20205901846103930d13dd5f57ada.png)Mozilla Builders 2024](<https://builders.mozilla.org/> "Mozilla Builders 2024")[![GitHub](/assets/images/github-9561a29eaa5e059e087a198bedf703ce.png)GitHub Accelerator 2024](<https://github.com/accelerator> "GitHub Accelerator 2024")