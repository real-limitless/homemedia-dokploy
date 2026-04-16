# Homemedia Dokploy Stack

This repository contains a complete media server stack for deployment on Dokploy using Docker Compose.

## Structure
- `media-stack/docker-compose.yml` - Main compose file with rclone FUSE mount, *arr stack, Jellyfin, Plex, etc.
- `media-stack/.env.example` - Template (copy to `.env` and fill secrets — **never commit real values**)
- `media-stack/rclone/config/rclone.conf` - **Template only** (update with real S3 creds locally; now ignored by `.gitignore`)

**Security note**: A top-level `.gitignore` has been added to prevent committing `.env`, `rclone.conf` credentials, caches, etc.

## One-time Host Preparation
Run on your Dokploy server:
```bash
sudo apt update && sudo apt install fuse3 -y
sudo modprobe fuse
echo "user_allow_other" | sudo tee -a /etc/fuse.conf
sudo mkdir -p /mnt/s3-media
```

## Deployment in Dokploy
1. Create a new **Compose** project named `media-stack`.
2. Set the compose file path to `media-stack/docker-compose.yml` (or upload the contents).
3. Copy `.env.example` → `.env` and fill your values (TZ, VPN creds, etc.).  
   Copy/edit `rclone/config/rclone.conf` with **real** S3 credentials (this file is now gitignored).
4. Deploy the stack in Dokploy.
5. Configure domains for each service in the Dokploy UI (e.g., `jellyfin.yourdomain.com`).

**Important**: Never commit real secrets. The new `.gitignore` protects `.env` and `rclone.conf`. Use Dokploy's built-in secret management for production.

See the full guide in the original plan for details.
