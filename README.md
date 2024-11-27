# Zilla Test

## Description
I have been trying to set Zilla up on my Windows 11 workstation with Docker Compose, and have not been able to make any of the examples run.

The common thread has been an error related to affinity (`affinity mask must specify at least one bit: REST-example.south_kafka_cache_server 0`) when I view the Zilla logs in Docker:

![Error Message](https://raw.githubusercontent.com/clonardo/zilla-test/main/images/error_text.png)

When this happens, the container dies, and will keep dying if restarted.

For the sake of simplicity, I am now focused on the simple [2-file setup for a CRUD API on top of Kafka here](https://docs.aklivity.io/zilla/latest/tutorials/rest/rest-intro.html#setup). The `docker-compose.yaml` + `zilla.yaml` in this repo are identical to what was posted on the Aklivity site as of November 26, 2024.

My docker-compose is working fine for everything else on my machine.

## Environment Details
- Host OS: Windows 11 (fully up to date)
- Docker version 27.3.1 with wsl 2.3.26.0 (latest) backend
- JDK: openjdk 21.0.5 2024-10-15 LTS

## Things I Have Tried
- I have been running `docker-compose up -d` to attempt to bring up the environment. Between tweaking things and trying to make it work, I experimented with clearing + pruning all Docker resources, and still have had no success
- I have tried this from Powershell as well as the wsl Ubuntu shell (with Docker pass-through), and see the same errors every time.
- I have tried working through a number of other Kafka and RedPanda + Zilla examples. Alas, no joy

## To Attempt to Reproduce the Issue
- Clone this repo
- From the repo root, run `docker-compose up -d`
- View the logs of the `zilla` container in Docker

Of note:
- While this zilla.yaml file is exactly as it was shown on the Aklivity site, as I was fiddling with versions of the config, I saw the same `affinity` failure across the http server as well as the Kafka cache server
- Depending on what the state of your config is in, sometimes it won't fail until an http request comes into the Zilla gateway (e.g., GET on `localhost:7114/api/items`)- otherwise it will log the config and then fall over 