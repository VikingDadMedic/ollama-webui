---
title: Updating Open WebUI
source: getting-started/updating/index.html
word_count: 2034
code_blocks: 0
quality_score: 80.0
extracted: 2025-12-14T09:48:32.452619
---

# Updating Open WebUI

## Overview

Keeping Open WebUI updated ensures you have the latest features, security patches, and bug fixes. You can update manually or automate the process using container update tools.

Before Updating

 * **Backup your data** before major version updates
 * **Check release notes** at <https://github.com/open-webui/open-webui/releases> for breaking changes
 * **Clear browser cache** after updating to ensure the latest web interface loads

## Manual Update

Manual updates give you complete control and are recommended for production environments or when you need to review changes before applying them.

### Step 1: Stop and Remove Current Container

This stops the running container and removes it without deleting your data stored in the Docker volume.

Terminal
[code]
 # Replace 'open-webui' with your container name if different 
 # Use 'docker ps' to find your container name if unsure 
 docker rm -f open-webui 

[/code]

Find Your Container Name

If your container isn't named `open-webui`, find it with:
[code]
 docker ps -a | grep open-webui 

[/code]

Look in the "NAMES" column for your actual container name.

### Step 2: Pull Latest Docker Image

Download the newest Open WebUI image from the container registry.

Terminal
[code]
 docker pull ghcr.io/open-webui/open-webui:main 

[/code]

**Expected output:**
[code] 
 main: Pulling from open-webui/open-webui 
 Digest: sha256:abc123... 
 Status: Downloaded newer image for ghcr.io/open-webui/open-webui:main 

[/code]

About Data Persistence

Your chat histories, settings, and uploaded files are stored in a Docker volume named `open-webui`. Pulling a new image does **not** affect this data. The volume persists independently of the container.

### Step 3: Start Container with Updated Image

Recreate the container using the new image while mounting your existing data volume.

 * CPU Only
 * NVIDIA GPU

Terminal - Standard Deployment
[code]
 docker run -d \ 
 -p 3000:8080 \ 
 -v open-webui:/app/backend/data \ 
 --name open-webui \ 
 --restart always \ 
 ghcr.io/open-webui/open-webui:main 

[/code]

Terminal - With NVIDIA GPU Support
[code]
 docker run -d \ 
 -p 3000:8080 \ 
 --gpus all \ 
 -v open-webui:/app/backend/data \ 
 --name open-webui \ 
 --restart always \ 
 ghcr.io/open-webui/open-webui:main 

[/code]

About WEBUI_SECRET_KEY

If you're not setting `WEBUI_SECRET_KEY`, it will be auto-generated each time you recreate the container, **causing you to be logged out after every update**.

See the Persistent Login Sessions section below to fix this.

### Verify Update Success

Check that Open WebUI started successfully:

Terminal - Check Container Logs
[code]
 docker logs open-webui 

 # Watch logs in real-time 
 docker logs -f open-webui 

[/code]

**Successful startup indicators:**
[code] 
 INFO: [db] Database initialization complete 
 INFO: [main] Open WebUI starting on http://0.0.0.0:8080 

[/code]

Then verify in your browser:

 1. Navigate to `http://localhost:3000` (or your configured port)
 2. Clear browser cache (Ctrl+Shift+Delete or Cmd+Shift+Delete)
 3. Hard refresh the page (Ctrl+F5 or Cmd+Shift+R)
 4. Log in and verify your data is intact

## Persistent Login Sessions

To avoid being logged out after every update, you must set a persistent `WEBUI_SECRET_KEY`.

### Generate and Set Secret Key

 * Docker Run
 * Docker Compose

Terminal - Docker Run with Secret Key
[code]
 docker run -d \ 
 -p 3000:8080 \ 
 -v open-webui:/app/backend/data \ 
 --name open-webui \ 
 --restart always \ 
 -e WEBUI_SECRET_KEY="your-secret-key-here" \ 
 ghcr.io/open-webui/open-webui:main 

[/code]

Generate a Secure Key

Generate a cryptographically secure key with:
[code]
 openssl rand -hex 32 

[/code]

Or use Python:
[code]
 python3 -c "import secrets; print(secrets.token_hex(32))" 

[/code]

docker-compose.yml
[code]
 version: '3' 
 services: 
 open-webui: 
 image: ghcr.io/open-webui/open-webui:main 
 ports: 
 - "3000:8080" 
 volumes: 
 - open-webui:/app/backend/data 
 environment: 
 - WEBUI_SECRET_KEY=your-secret-key-here 
 restart: unless-stopped 

 volumes: 
 open-webui: 

[/code]

Store Secret Key Securely

 * **Never commit** your secret key to version control
 * Use environment files (`.env`) or secret management tools
 * Keep the same key across updates to maintain sessions

For complete environment variable documentation, see [Environment Configuration](<https://docs.openwebui.com/getting-started/env-configuration#security-variables>).

## Automated Update Tools

Automated updates can save time but require careful consideration of the trade-offs.

Important Considerations

**Automated updates can break your deployment if:**

 * A new version has breaking changes you haven't reviewed
 * Custom configurations become incompatible
 * Database migrations fail during unattended updates
 * You have plugins or customizations that aren't forward-compatible

**Best practices:**

 * Always review release notes before auto-updating production systems
 * Test updates in a staging environment first
 * Consider notification-only tools rather than automatic updates
 * Have a rollback plan and recent backups

### Option 1: Watchtower (Community Fork)

Watchtower Status

The original `containrrr/watchtower` is **no longer maintained** and **does not work with Docker 29+**. The community has created maintained forks that resolve these issues.

The original Watchtower project hasn't received updates in over two years and fails with Docker version 29.0.0 or newer due to API version incompatibility. Two maintained forks are now available: nickfedor/watchtower and Marrrrrrrrry/watchtower, both compatible with Docker 29+.

**Recommended: nickfedor/watchtower fork**

 * One-Time Update
 * Continuous Monitoring
 * Docker Compose

Run Watchtower once to update all containers, then exit:

Terminal - One-Time Update
[code]
 docker run --rm \ 
 --volume /var/run/docker.sock:/var/run/docker.sock \ 
 nickfedor/watchtower \ 
 --run-once open-webui 

[/code]

Run Watchtower as a persistent container that checks for updates every 6 hours:

Terminal - Continuous Watchtower
[code]
 docker run -d \ 
 --name watchtower \ 
 --restart unless-stopped \ 
 --volume /var/run/docker.sock:/var/run/docker.sock \ 
 nickfedor/watchtower \ 
 --interval 21600 \ 
 open-webui 

[/code]

docker-compose.yml
[code]
 version: '3' 
 services: 
 open-webui: 
 image: ghcr.io/open-webui/open-webui:main 
 ports: 
 - "3000:8080" 
 volumes: 
 - open-webui:/app/backend/data 
 environment: 
 - WEBUI_SECRET_KEY=your-secret-key-here 
 restart: unless-stopped 

 watchtower: 
 image: nickfedor/watchtower:latest 
 volumes: 
 - /var/run/docker.sock:/var/run/docker.sock 
 environment: 
 - WATCHTOWER_CLEANUP=true 
 - WATCHTOWER_INCLUDE_STOPPED=false 
 - WATCHTOWER_SCHEDULE=0 0 2 * * * # 2 AM daily 
 command: open-webui 
 restart: unless-stopped 
 depends_on: 
 - open-webui 

 volumes: 
 open-webui: 

[/code]

**Watchtower configuration options:**

Environment Variable| Description| Default 
---|---|--- 
`WATCHTOWER_CLEANUP`| Remove old images after update| `false` 
`WATCHTOWER_INCLUDE_STOPPED`| Update stopped containers too| `false` 
`WATCHTOWER_SCHEDULE`| Cron expression for update schedule| `0 0 0 * * *` (midnight) 
`WATCHTOWER_MONITOR_ONLY`| Only notify, don't update| `false` 

Monitor-Only Mode

To receive notifications without automatic updates:
[code]
 docker run -d \ 
 --name watchtower \ 
 --volume /var/run/docker.sock:/var/run/docker.sock \ 
 -e WATCHTOWER_MONITOR_ONLY=true \ 
 -e WATCHTOWER_NOTIFICATIONS=email \ 
 -e WATCHTOWER_NOTIFICATION_EMAIL_TO=you@example.com \ 
 nickfedor/watchtower 

[/code]

**Alternative fork:** Marrrrrrrrry/watchtower is another actively maintained fork with updated dependencies and simplified functions.

For complete Watchtower documentation, visit <https://watchtower.nickfedor.com/>

### Option 2: What's Up Docker (WUD)

What's Up Docker (WUD) is a notification-focused alternative that doesn't automatically update containers but instead provides a web UI to monitor updates and trigger them manually.

**Why choose WUD:**

 * ✅ Web UI for visual monitoring
 * ✅ Click-button manual updates
 * ✅ Shows descriptive names and changelogs
 * ✅ Auto-prunes old images
 * ✅ Supports multiple Docker hosts
 * ✅ Extensive notification options
 * ❌ Requires manual intervention (not fully automated)

**Quick start with WUD:**

docker-compose.yml
[code]
 version: '3' 
 services: 
 wud: 
 image: fmartinou/whats-up-docker:latest 
 container_name: wud 
 ports: 
 - "3001:3000" 
 volumes: 
 - /var/run/docker.sock:/var/run/docker.sock 
 environment: 
 # Authentication (optional but recommended) 
 - WUD_AUTH_BASIC_USER=admin 
 - WUD_AUTH_BASIC_HASH=$$apr1$$... # Generate with htpasswd 

 # Enable triggers for updates 
 - WUD_TRIGGER_DOCKERCOMPOSE_WUD_FILE=/docker-compose.yml 

 # Notification examples 
 - WUD_WATCHER_LOCAL_SOCKET=/var/run/docker.sock 
 restart: unless-stopped 

[/code]

After starting WUD, access the web interface at `http://localhost:3001`. You'll see all containers and available updates with click-to-update buttons.

Generate Password Hash
[code]
 htpasswd -nbB admin yourpassword 

[/code]

Copy the part after the colon, and replace each `$` with `$$` in docker-compose.

For complete WUD documentation, visit <https://getwud.github.io/wud/>

### Option 3: Diun (Docker Image Update Notifier)

Diun is a lightweight CLI tool that only sends notifications about available updates without performing any updates. It's ideal if you want complete control and just need alerts.

**Why choose Diun:**

 * ✅ Notification-only (safest approach)
 * ✅ No web UI overhead (lightweight)
 * ✅ Multiple notification providers (email, Slack, Discord, Telegram, etc.)
 * ✅ Fine-grained control over what to monitor
 * ❌ No built-in update mechanism (purely informational)

docker-compose.yml
[code]
 version: '3' 
 services: 
 diun: 
 image: crazymax/diun:latest 
 container_name: diun 
 volumes: 
 - /var/run/docker.sock:/var/run/docker.sock:ro 
 - ./data:/data 
 environment: 
 - TZ=America/New_York 
 - LOG_LEVEL=info 
 - DIUN_WATCH_WORKERS=10 
 - DIUN_WATCH_SCHEDULE=0 */6 * * * # Every 6 hours 
 - DIUN_PROVIDERS_DOCKER=true 
 - DIUN_NOTIF_MAIL_HOST=smtp.gmail.com 
 - DIUN_NOTIF_MAIL_PORT=587 
 - DIUN_NOTIF_MAIL_USERNAME=your-email@gmail.com 
 - DIUN_NOTIF_MAIL_PASSWORD=your-app-password 
 - DIUN_NOTIF_MAIL_FROM=your-email@gmail.com 
 - DIUN_NOTIF_MAIL_TO=your-email@gmail.com 
 restart: unless-stopped 

[/code]

For complete Diun documentation, visit <https://crazymax.dev/diun/>

### Comparison: Which Tool Should You Use?

Feature| Watchtower (Fork)| WUD| Diun 
---|---|---|--- 
**Automatic Updates**| ✅ Yes| ⚠️ Manual via UI| ❌ No 
**Web Interface**| ❌ No| ✅ Yes| ❌ No 
**Notifications**| ✅ Yes| ✅ Yes| ✅ Yes 
**Manual Control**| ⚠️ Limited| ✅ Full control| ✅ Full control 
**Resource Usage**| Low| Medium| Very Low 
**Docker 29+ Support**| ✅ Yes (forks)| ✅ Yes| ✅ Yes 
**Best For**| Set-and-forget homelabs| Visual monitoring + control| Notification-only workflows 

Recommendation

 * **For homelabs/personal use:** nickfedor/watchtower (automated)
 * **For managed environments:** WUD (visual + manual control)
 * **For production/critical systems:** Diun (notifications only) + manual updates

## Troubleshooting Updates

### Container Won't Start After Update

**Check logs for errors:**

Terminal
[code]
 docker logs open-webui 

 # Look for migration errors or startup failures 

[/code]

**Common causes:**

 * Database migration failed
 * Incompatible environment variables
 * Port already in use

**Solution:** Restore previous version and investigate:

Terminal - Rollback to Previous Version
[code]
 # Stop current container 
 docker stop open-webui 
 docker rm open-webui 

 # Pull specific older version (check GitHub releases for version tags) 
 docker pull ghcr.io/open-webui/open-webui:v0.4.0 

 # Start with old image 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data \ 
 --name open-webui ghcr.io/open-webui/open-webui:v0.4.0 

[/code]

### Data Loss or Corruption

**If you suspect data issues:**

Terminal - Inspect Volume
[code]
 # Find volume location 
 docker volume inspect open-webui 

 # Check database file exists 
 docker run --rm -v open-webui:/data alpine ls -lah /data 

[/code]

**Recovery steps:**

 1. Stop Open WebUI: `docker stop open-webui`
 2. Backup volume: `docker run --rm -v open-webui:/data -v $(pwd):/backup alpine tar czf /backup/openwebui-backup.tar.gz /data`
 3. Restore from backup if needed
 4. Check [Manual Migration Guide](</troubleshooting/manual-database-migration>) for database issues

### Watchtower Updates Too Frequently

Configure update schedule with cron expressions:

Terminal - Custom Schedule
[code]
 # Daily at 3 AM 
 -e WATCHTOWER_SCHEDULE="0 0 3 * * *" 

 # Weekly on Sundays at 2 AM 
 -e WATCHTOWER_SCHEDULE="0 0 2 * * 0" 

 # Every 12 hours 
 -e WATCHTOWER_SCHEDULE="0 0 */12 * * *" 

[/code]

### "Logged Out After Update" Despite Setting Secret Key

**Diagnosis:**

Terminal - Check Environment Variables
[code]
 docker inspect open-webui | grep WEBUI_SECRET_KEY 

[/code]

If the key isn't showing, you didn't pass it correctly when recreating the container.

**Fix:**

Terminal - Recreate with Correct Key
[code]
 docker stop open-webui 
 docker rm open-webui 

 # Make sure to include -e WEBUI_SECRET_KEY 
 docker run -d -p 3000:8080 -v open-webui:/app/backend/data \ 
 -e WEBUI_SECRET_KEY="your-persistent-key" \ 
 --name open-webui ghcr.io/open-webui/open-webui:main 

[/code]

## Docker Volume Management

### Locate Your Data

The `open-webui` Docker volume contains all your data (chats, users, uploads, etc.).

Terminal - Inspect Volume
[code]
 docker volume inspect open-webui 

[/code]

**Common volume locations:**

 * Linux: `/var/lib/docker/volumes/open-webui/_data`
 * Windows (WSL2): `\\wsl$\docker-desktop\mnt\docker-desktop-disk\data\docker\volumes\open-webui\_data`
 * macOS: `~/Library/Containers/com.docker.docker/Data/vms/0/data/docker/volumes/open-webui/_data`

Direct Access Risk

Avoid directly modifying files in the volume location. Always interact through the container or Docker commands to prevent corruption.

### Backup Volume

Terminal - Backup Docker Volume
[code]
 # Create timestamped backup 
 docker run --rm \ 
 -v open-webui:/data \ 
 -v $(pwd):/backup \ 
 alpine tar czf /backup/openwebui-$(date +%Y%m%d_%H%M%S).tar.gz /data 

[/code]

### Restore Volume

Terminal - Restore from Backup
[code]
 # Stop container 
 docker stop open-webui 

 # Restore backup 
 docker run --rm \ 
 -v open-webui:/data \ 
 -v $(pwd):/backup \ 
 alpine sh -c "rm -rf /data/* && tar xzf /backup/openwebui-20241201_120000.tar.gz -C /" 

 # Start container 
 docker start open-webui 

[/code]

### Clean Up Old Images

After updating, old images remain on disk. Remove them to free space:

Terminal - Remove Unused Images
[code]
 # Remove dangling images (not tagged or used) 
 docker image prune 

 # Remove all unused images (careful!) 
 docker image prune -a 

 # List all open-webui images 
 docker images | grep open-webui 

[/code]

Watchtower can do this automatically with:
[code] 
 -e WATCHTOWER_CLEANUP=true 

[/code]

## Post-Update Checklist

After updating, verify everything works:

 * Open WebUI starts without errors (`docker logs open-webui`)
 * Can access web interface at `http://localhost:3000`
 * Can log in with existing credentials
 * Chat history is intact
 * Models are still configured correctly
 * Custom settings are preserved
 * No JavaScript console errors (F12 in browser)
 * Clear browser cache if interface looks broken

Browser Cache Issues

If the interface looks broken or old after updating:

 1. Hard refresh: Ctrl+F5 (Windows/Linux) or Cmd+Shift+R (Mac)
 2. Clear site data: Browser Settings > Privacy > Clear browsing data
 3. Try incognito/private window to verify it's a cache issue

## Additional Resources

 * [Open WebUI GitHub Releases](<https://github.com/open-webui/open-webui/releases>) \- Version history and changelogs
 * [Environment Configuration Guide](<https://docs.openwebui.com/getting-started/env-configuration>) \- All environment variables
 * [Manual Database Migration](</troubleshooting/manual-database-migration>) \- Fix database issues after updates
 * [Watchtower Documentation](<https://watchtower.nickfedor.com/>) \- Advanced Watchtower configuration
 * [WUD Documentation](<https://getwud.github.io/wud/>) \- What's Up Docker setup guide