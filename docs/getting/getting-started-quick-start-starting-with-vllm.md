---
title: Starting With vLLM
source: getting-started/quick-start/starting-with-vllm/index.html
word_count: 167
code_blocks: 0
quality_score: 65.0
extracted: 2025-12-14T09:48:32.394007
---

# Starting With vLLM

## Overview

vLLM provides an OpenAI-compatible API, making it easy to connect to Open WebUI. This guide will show you how to connect your vLLM server.

* * *

## Step 1: Set Up Your vLLM Server

Make sure your vLLM server is running and accessible. The default API base URL is typically:
[code] 
 http://localhost:8000/v1 

[/code]

For remote servers, use the appropriate hostname or IP address.

* * *

## Step 2: Add the API Connection in Open WebUI

 1. Go to ⚙️ **Admin Settings**.
 2. Navigate to **Connections > OpenAI > Manage** (look for the wrench icon).
 3. Click ➕ **Add New Connection**.
 4. Fill in the following: 
 * **API URL** : `http://localhost:8000/v1` (or your vLLM server URL)
 * **API Key** : Leave empty (vLLM typically doesn't require an API key for local connections)
 5. Click **Save**.

* * *

## Step 3: Start Using Models

Select any model that's available on your vLLM server from the Model Selector and start chatting.