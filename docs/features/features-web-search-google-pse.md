---
title: Google PSE
source: features/web-search/google-pse/index.html
word_count: 193
code_blocks: 0
quality_score: 80.0
extracted: 2025-12-14T09:48:31.523042
---

# Google PSE

warning

This tutorial is a community contribution and is not supported by the Open WebUI team. It serves only as a demonstration on how to customize Open WebUI for your specific use case. Want to contribute? Check out the contributing tutorial.

## Google PSE API

### Setup

 1. Go to Google Developers, use [Programmable Search Engine](<https://developers.google.com/custom-search>), and log on or create account.
 2. Go to [control panel](<https://programmablesearchengine.google.com/controlpanel/all>) and click `Add` button
 3. Enter a search engine name, set the other properties to suit your needs, verify you're not a robot and click `Create` button.
 4. Generate `API key` and get the `Search engine ID`. (Available after the engine is created)
 5. With `API key` and `Search engine ID`, open `Open WebUI Admin panel` and click `Settings` tab, and then click `Web Search`
 6. Enable `Web search` and Set `Web Search Engine` to `google_pse`
 7. Fill `Google PSE API Key` with the `API key` and `Google PSE Engine Id` (# 4)
 8. Click `Save`

![Open WebUI Admin panel](/assets/images/tutorial_google_pse1-94ef7e085e91377d88e015463a786162.png)

#### Note

You have to enable `Web search` in the prompt field, using plus (`+`) button. Search the web ;-)

![enable Web search](/assets/images/tutorial_google_pse2-141dd0c9eb65b65ed2d05b5dfb18b1ca.png)