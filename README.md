# AdGuard Home Sync

Docker service for synchronizing configuration and settings between multiple AdGuard Home instances using [adguardhome-sync](https://github.com/bakito/adguardhome-sync).

## Overview

This service automatically synchronizes settings from a primary (origin) AdGuard Home instance to one or more replica instances. It ensures your AdGuard Home configurations stay consistent across multiple servers.

## Features

The sync includes the following features:
- General settings
- Query log configuration
- Statistics configuration
- Client settings
- Services (blocked services)
- Filters (blocklists and allowlists)
- DHCP server configuration
- DHCP static leases
- DNS server configuration
- DNS access lists
- DNS rewrites
- UI theme

## Prerequisites

- Docker and Docker Compose installed
- Multiple AdGuard Home instances running and accessible
- Admin credentials for each AdGuard Home instance

## Configuration

### Current Setup

- **Origin Server**: Primary AdGuard Home instance at `192.168.1.178:8080`
- **Replica 1**: Secondary instance at `192.168.1.91:8080`
- **Sync Schedule**: Every minute (`* * * * *`)
- **Web UI**: Accessible on port `8090`

### Environment Variables

Key configuration options in `docker-compose.yaml`:

| Variable | Description | Default |
|----------|-------------|---------|
| `LOG_LEVEL` | Logging verbosity | `info` |
| `ORIGIN_URL` | URL of the primary AdGuard Home instance | Required |
| `ORIGIN_USERNAME` | Admin username for origin | Required |
| `ORIGIN_PASSWORD` | Admin password for origin | Required |
| `REPLICA1_URL` | URL of the first replica | Required |
| `REPLICA1_USERNAME` | Admin username for replica 1 | Required |
| `REPLICA1_PASSWORD` | Admin password for replica 1 | Required |
| `CRON` | Sync schedule (cron format) | `0 */2 * * *` |
| `RUN_ON_START` | Run sync immediately on container start | `true` |
| `API_PORT` | Web UI port (inside container) | `8080` |

## Deployment

### Using Docker Compose

1. Clone this repository:
```bash
git clone <your-repo-url>
cd adguard-sync
```

2. Create your environment configuration:
```bash
cp .env.example .env
```

3. Edit the `.env` file and update with your actual values:
   - Update IP addresses to match your AdGuard Home instances
   - Set appropriate credentials for origin and replica instances
   - Adjust the CRON schedule if needed
   - Configure any optional features

4. Start the service:
```bash
docker compose up -d
```

5. Check logs:
```bash
docker compose logs -f adguardhome-sync
```

### Using Komodo

This repository is configured for deployment with Komodo:

1. Add this repository to your Komodo instance
2. Configure environment variables in Komodo (based on `.env.example`)
3. Komodo will automatically deploy using the `docker-compose.yaml` configuration
4. Monitor the deployment through the Komodo dashboard

**Note**: When using Komodo, you can either:
- Push your `.env` file to the repository (if private and secure)
- Configure environment variables directly in Komodo's interface

## Web Interface

The sync service includes a web interface for monitoring and manual synchronization:

- **URL**: `http://<host-ip>:8090`
- Access the dashboard to view sync status and trigger manual syncs

## Adding Additional Replicas

To add more replica instances, uncomment and configure additional replica sections in `docker-compose.yaml`:

```yaml
REPLICA2_URL: "http://192.168.1.x:8080"
REPLICA2_USERNAME: "username"
REPLICA2_PASSWORD: "password"
```

You can add up to multiple replicas (REPLICA2, REPLICA3, etc.).

## Security Considerations

� **Important**: This configuration uses environment variables to manage sensitive credentials.

**Best Practices:**
1. The `.env` file contains sensitive credentials and is excluded from version control via `.gitignore`
2. Never commit the `.env` file to your repository
3. Use `.env.example` as a template for new deployments
4. Restrict network access to the sync service
5. Consider using HTTPS for AdGuard Home instances
6. Regularly rotate credentials
7. When deploying with Komodo, configure environment variables through Komodo's interface

## Troubleshooting

### Check container status
```bash
docker compose ps
```

### View logs
```bash
docker compose logs -f adguardhome-sync
```

### Restart service
```bash
docker compose restart
```

### Force sync
Access the web UI at `http://<host-ip>:8090` and click the sync button.

## Sync Schedule

The current configuration runs every minute. To change the schedule, modify the `CRON` variable:

- Every 2 hours: `0 */2 * * *`
- Every 30 minutes: `*/30 * * * *`
- Daily at 2 AM: `0 2 * * *`

## Links

- [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome)
- [AdGuard Home Sync Project](https://github.com/bakito/adguardhome-sync)

## License

This configuration is for personal use. Refer to the upstream project licenses for the actual software.
