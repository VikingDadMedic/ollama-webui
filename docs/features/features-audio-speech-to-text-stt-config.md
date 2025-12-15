---
title: Configuration
source: features/audio/speech-to-text/stt-config/index.html
word_count: 275
code_blocks: 0
quality_score: 70.0
extracted: 2025-12-14T09:48:29.954181
---

# Configuration 

Open Web UI supports both local, browser, and remote speech to text.

![alt text](/assets/images/image-7ca98cf16f412853e0b540cb7429a4f8.png)

![alt text](/assets/images/stt-providers-a5acf0ba4731e4560b69a11743d23b0c.png)

## Cloud / Remote Speech To Text Proivders

The following cloud speech to text providers are currently supported. API keys can be configured as environment variables (OpenAI) or in the admin settings page (both keys).

Service| API Key Required 
---|--- 
OpenAI| ✅ 
DeepGram| ✅ 

WebAPI provides STT via the built-in browser STT provider.

## Configuring Your STT Provider

To configure a speech to text provider:

 * Navigate to the admin settings
 * Choose Audio
 * Provider an API key and choose a model from the dropdown

![alt text](/assets/images/stt-config-0f5dea04425f765dc3ef8e0eed5cb85e.png)

## User-Level Settings

In addition the instance settings provisioned in the admin panel, there are also a couple of user-level settings that can provide additional functionality.

 * **STT Settings:** Contains settings related to Speech-to-Text functionality.
 * **Speech-to-Text Engine:** Determines the engine used for speech recognition (Default or Web API).

![alt text](/assets/images/user-settings-cd00570f71f7332d684741427f632506.png)

## Using STT

Speech to text provides a highly efficient way of "writing" prompts using your voice and it performs robustly from both desktop and mobile devices.

To use STT, simply click on the microphone icon:

![alt text](/assets/images/stt-operation-d64ed9443ee5fac8641cde25f746fc77.png)

A live audio waveform will indicate successful voice capture:

![alt text](/assets/images/stt-in-progress-5eb1cafd9602495cc9fa7ccfe9b72a66.png)

## STT Mode Operation

Once your recording has begun you can:

 * Click on the tick icon to save the recording (if auto send after completion is enabled it will send for completion; otherwise you can manually send)
 * If you wish to abort the recording (for example, you wish to start a fresh recording) you can click on the 'x' icon to scape the recording interface

![alt text](/assets/images/endstt-70fb0b42872d0a7e670b01384004067d.png)