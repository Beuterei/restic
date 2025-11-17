[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <img src="https://restic.readthedocs.io/en/stable/_static/logo.png" alt="Logo" height="60">

  <h3 align="center">restic</h3>

  <p align="center">
    Docker setup for restic
    <br />
    <br />
    ·
    <a href="https://github.com/Beuterei/restic/issues">Report Bug</a>
    ·
    <a href="https://github.com/Beuterei/restic/issues">Request Feature</a>
  </p>
</p>

<!-- ABOUT THE PROJECT -->

## About The Project

Small docker setup for restic. It will backup all your docker volumes.

<!-- GETTING STARTED -->

## Getting Started Develop

To get a local copy up and running follow these simple steps.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Installation

1. Clone this repository

2. Create a `.env.local` file and configure variables. See [Customization](#customization) section.

```sh
touch .env.local
```

Add `COMPOSE_PROFILES=prune,check` to your `.env.local` file to enable prune and check services by default.

3. Start docker-compose

```sh
docker-compose up --build
```

## Getting Started Production

To get a copy up and running follow these simple steps.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Nginx by me](https://github.com/beuluis/nginx)

### Installation

1. Clone this repository

2. Create a `.env.local` file and configure variables. See [Customization](#customization) section.

```sh
touch .env.local
```

Add `COMPOSE_PROFILES=prune,check` to your `.env.local` file to enable prune and check services by default.

3. Start docker-compose

```sh
docker-compose up --build -d
```

(Set `COMPOSE_PROFILES=` empty or omit it to disable prune and check)

## Customization

Overwrite variables as needed (format: `{variable name}={variable value}`).

| Variable                    | Description                                                                                     | Default value            | Required |
| --------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------ | -------- |
| `APPRISE_NOTIFICATION_URLS` | Apprise notification URLs (see [Apprise Docs](https://github.com/caronc/apprise))               | none                     | true     |
| `AWS_ACCESS_KEY_ID`         | AWS access key ID for S3 repository access                                                      | none                     | true     |
| `AWS_SECRET_ACCESS_KEY`     | AWS secret access key for S3 repository access                                                  | none                     | true     |
| `RESTIC_BACKUP_CRON`        | GoCron schedule to run backup                                                                   | `0 30 3 * * *`           | false    |
| `RESTIC_BACKUP_HOST`        | Host name identifier for backups                                                                | none                     | true     |
| `RESTIC_BACKUP_TAG`         | Tag for backup snapshots                                                                        | `docker-volumes`         | false    |
| `RESTIC_CHECK_ARGS`         | Additional arguments for restic check command                                                   | `--read-data-subset=10%` | false    |
| `RESTIC_CHECK_CRON`         | GoCron schedule to run repository check                                                         | `0 15 5 * * *`           | false    |
| `RESTIC_KEEP_DAILY`         | For the last `n` days which have one or more snapshots, only keep the last one for that day     | `7`                      | false    |
| `RESTIC_KEEP_LAST`          | Never delete the `n` last (most recent) snapshots                                               | `30`                     | false    |
| `RESTIC_KEEP_MONTHLY`       | For the last `n` months which have one or more snapshots, only keep the last one for that month | `12`                     | false    |
| `RESTIC_KEEP_WEEKLY`        | For the last `n` weeks which have one or more snapshots, only keep the last one for that week   | `5`                      | false    |
| `RESTIC_PASSWORD`           | Password for restic repository                                                                  | none                     | true     |
| `RESTIC_PRUNE_CRON`         | GoCron schedule to run repository prune                                                         | `0 0 4 * * *`            | false    |
| `RESTIC_REPOSITORY`         | Repository location (see [Restic Docs](https://restic.readthedocs.io/))                         | none                     | true     |
| `RESTIC_TZ`                 | Timezone for cron schedules                                                                     | `Europe/Berlin`          | false    |

### Single Host vs Multi-Host Configuration

**Single Host Setup:**

On a single host, all services (`restic-backup`, `restic-prune`, and `restic-check`) should **always run**. Simply add `COMPOSE_PROFILES=prune,check` to your `.env.local` file to enable all services.

**Multi-Host Setup:**

When backing up multiple hosts to the same restic repository, Docker Compose profiles can be used to control which host runs which maintenance operations. **Not all hosts need to run prune and check** - you can designate specific hosts for specific tasks.

**Example multi-host setup:**

- **Host 1 (Primary):** All services enabled (`COMPOSE_PROFILES=prune,check`)
  - Backup at 3:30 AM, Prune at 4:00 AM, Check at 5:15 AM
- **Host 2 (Secondary):** Only backup enabled (no profiles)
  - Backup at 4:00 AM

This allows you to:

- **Distribute load:** Run maintenance operations on dedicated hosts
- **Avoid conflicts:** Prevent simultaneous prune/check operations from multiple hosts
- **Optimize resources:** Only run maintenance where needed

Configure the cron schedules (`RESTIC_BACKUP_CRON`, `RESTIC_PRUNE_CRON`, `RESTIC_CHECK_CRON`) appropriately for each host based on their assigned roles.

### Service Control

The `restic-prune` and `restic-check` services are controlled via Docker Compose profiles.

- **Single host:** Always enable both services (see [Single Host vs Multi-Host Configuration](#single-host-vs-multi-host-configuration))
- **Multi-host:** Use profiles to control which host runs which services (see [Single Host vs Multi-Host Configuration](#single-host-vs-multi-host-configuration))

**To enable prune and check services (recommended):**

Add to your `.env.local` file:

```sh
COMPOSE_PROFILES=prune,check
```

Or use the `--profile` flag:

```sh
docker-compose --profile prune --profile check up --build
```

**To disable prune and check services:**

Simply don't set `COMPOSE_PROFILES` or set it to empty:

```sh
# In .env.local, either omit COMPOSE_PROFILES or set:
COMPOSE_PROFILES=

# Or use docker-compose without profiles
docker-compose up --build
```

**To enable only one service:**

```sh
# Only prune
COMPOSE_PROFILES=prune docker-compose up --build

# Only check
COMPOSE_PROFILES=check docker-compose up --build
```

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[contributors-shield]: https://img.shields.io/github/contributors/Beuterei/restic.svg?style=flat-square
[contributors-url]: https://github.com/Beuterei/restic/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Beuterei/restic.svg?style=flat-square
[forks-url]: https://github.com/Beuterei/restic/network/members
[stars-shield]: https://img.shields.io/github/stars/Beuterei/restic.svg?style=flat-square
[stars-url]: https://github.com/Beuterei/restic/stargazers
[issues-shield]: https://img.shields.io/github/issues/Beuterei/restic.svg?style=flat-square
[issues-url]: https://github.com/Beuterei/restic/issues
[license-shield]: https://img.shields.io/github/license/Beuterei/restic.svg?style=flat-square
