---
title: AUTOMATIC1111
source: features/image-generation-and-editing/automatic1111/index.html
word_count: 204
code_blocks: 0
quality_score: 70.0
extracted: 2025-12-14T09:48:30.265386
---

# AUTOMATIC1111

warning

This tutorial is a community contribution and is not supported by the Open WebUI team. It serves only as a demonstration on how to customize Open WebUI for your specific use case. Want to contribute? Check out the contributing tutorial.

Open WebUI supports image generation through the **AUTOMATIC1111** [API](<https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API>). Here are the steps to get started:

### Initial Setup

 1. Ensure that you have [AUTOMATIC1111](<https://github.com/AUTOMATIC1111/stable-diffusion-webui>) installed.
 2. Launch AUTOMATIC1111 with additional flags to enable API access:

[code] 
 /webui.sh --api --listen 

[/code]

 3. For Docker installation of WebUI with the environment variables preset, use the following command:

[code] 
 docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -e AUTOMATIC1111_BASE_URL=http://host.docker.internal:7860/ -e ENABLE_IMAGE_GENERATION=True -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main 

[/code]

### Setting Up Open WebUI with AUTOMATIC1111

 1. In Open WebUI, navigate to the **Admin Panel** > **Settings** > **Images** menu.
 2. Set the `Image Generation Engine` field to `Default (Automatic1111)`.
 3. In the API URL field, enter the address where AUTOMATIC1111's API is accessible:

![Screenshot of the Open WebUI Images settings page with Default \(Automatic1111\) selected and the API URL field highlighted.](/assets/images/automatic1111-settings-3de81295ed8c9764177877ed8fc73439.png)
[code] 
 http://<your_automatic1111_address>:7860/ 

[/code]

If you're running a Docker installation of Open WebUI and AUTOMATIC1111 on the same host, use `http://host.docker.internal:7860/` as your address.