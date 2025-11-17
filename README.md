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

2. Create a `.env` file and configure variables. See [Customization](#customization) section.

```sh
touch .env
```

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

2. Create a `.env.prod` file and configure variables. See [Customization](#customization) section.

```sh
touch .env.prod
```

3. Start docker-compose

```sh
docker-compose --env-file ./.env.prod up --build -d
```

## Customization

Create a `.env` file (for development) or `.env.prod` file (for production) and overwrite variables as needed (format: `{variable name}={variable value}`).

| Variable                | Description                                                                                     | Default value            | Required |
| ----------------------- | ----------------------------------------------------------------------------------------------- | ------------------------ | -------- |
| `AWS_ACCESS_KEY_ID`     | AWS access key ID for S3 repository access                                                      | none                     | true     |
| `AWS_SECRET_ACCESS_KEY` | AWS secret access key for S3 repository access                                                  | none                     | true     |
| `BACKUP_CRON`           | GoCron schedule to run backup                                                                   | `0 30 3 * * *`           | false    |
| `CHECK_CRON`            | GoCron schedule to run repository check                                                         | `0 15 5 * * *`           | false    |
| `NOTIFY_PASSWORD`       | Email password for notifications                                                                | none                     | true     |
| `NOTIFY_SERVER`         | Email server hostname for notifications                                                         | none                     | true     |
| `NOTIFY_TO`             | Email address to receive notifications                                                          | none                     | true     |
| `NOTIFY_USER`           | Email username for notifications                                                                | none                     | true     |
| `PRUNE_CRON`            | GoCron schedule to run repository prune                                                         | `0 0 4 * * *`            | false    |
| `RESTIC_BACKUP_HOST`    | Host name identifier for backups                                                                | none                     | true     |
| `RESTIC_BACKUP_TAG`     | Tag for backup snapshots                                                                        | `docker-volumes`         | false    |
| `RESTIC_CHECK_ARGS`     | Additional arguments for restic check command                                                   | `--read-data-subset=10%` | false    |
| `RESTIC_KEEP_DAILY`     | For the last `n` days which have one or more snapshots, only keep the last one for that day     | `7`                      | false    |
| `RESTIC_KEEP_LAST`      | Never delete the `n` last (most recent) snapshots                                               | `30`                     | false    |
| `RESTIC_KEEP_MONTHLY`   | For the last `n` months which have one or more snapshots, only keep the last one for that month | `12`                     | false    |
| `RESTIC_KEEP_WEEKLY`    | For the last `n` weeks which have one or more snapshots, only keep the last one for that week   | `5`                      | false    |
| `RESTIC_PASSWORD`       | Password for restic repository                                                                  | none                     | true     |
| `RESTIC_REPOSITORY`     | Repository location (see [Restic Docs](https://restic.readthedocs.io/))                         | none                     | true     |
| `RESTIC_TZ`             | Timezone for cron schedules                                                                     | `Europe/Berlin`          | false    |

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
