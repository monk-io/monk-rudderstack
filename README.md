# Rudderstack meets Monk

This repository contains Monk.io template to deploy Rudderstack system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

- [Rudderstack meets Monk](#rudderstack-meets-monk)
  - [Prerequisites](#prerequisites)
    - [Make sure monkd is running](#make-sure-monkd-is-running)
    - [Clone Repository](#clone-repository)
    - [Load Template](#load-template)
    - [Verify if it's loaded correctly](#verify-if-its-loaded-correctly)
  - [Deploy Stack](#deploy-stack)
    - [Deploy Rudderstack](#deploy-rudderstack)
  - [Variables](#variables)
  - [Stop, remove and clean up workloads and templates](#stop-remove-and-clean-up-workloads-and-templates)
  - [Persistency](#persistency)

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

### Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

### Clone Repository

```bash
git clone git@github.com:monk-io/monk-rudderstack.git
```

### Load Template

```bash
cd monk-rudderstack
monk load MANIFEST
```

### Verify if it's loaded correctly

```bash
$ monk list -l rudderstack
✔ Got the list
Repository  Template                      Version  Type      Tags
local       rudderstack/backend           -        runnable  -
local       rudderstack/d-transformer     -        runnable  -
local       rudderstack/db                -        runnable  -
local       rudderstack/metrics-exporter  -        runnable  -
local       rudderstack/stack             -        group     -
```

## Deploy Stack

### Deploy Rudderstack

```bash
$ monk run rudderstack/stack
✔ Starting the run job: local/rudderstack/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% rudderlabs/rudder-server:latest local
✔ [================================================] 100% postgres:15-alpine local
✔ [================================================] 100% rudderstack/rudder-transformer:latest local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container local-f6bb83efbe9d895a0f15b8d36d-al-rudderstack-db-postgres created DONE
✔ New container local-185cacb6859c7917bd78e1ceae-k-metrics-exporter-metrics created DONE
✔ New container local-a94336ec0e1b5494d9828849d5--d-transformer-transformer created DONE
✔ New container local-049a16aa128038da3300e79c55-udderstack-backend-backend created DONE
✔ Started local/rudderstack/stack
🔩 templates/local/rudderstack/stack
 └─🧊 Peer local
    ├─🔩 templates/local/rudderstack/db
    │  └─📦 local-f6bb83efbe9d895a0f15b8d36d-al-rudderstack-db-postgres running
    │     └─🧩 postgres:15-alpine
    ├─🔩 templates/local/rudderstack/metrics-exporter
    │  └─📦 local-185cacb6859c7917bd78e1ceae-k-metrics-exporter-metrics running
    │     └─🧩 prom/statsd-exporter:v0.22.4
    ├─🔩 templates/local/rudderstack/backend
    │  └─📦 local-049a16aa128038da3300e79c55-udderstack-backend-backend running
    │     ├─🧩 rudderlabs/rudder-server:latest
    │     ├─💾 /var/lib/monkd/volumes/dragonflydb -> /data
    │     └─🔌 open 86.15.92.46:8080 -> 8080
    └─🔩 templates/local/rudderstack/d-transformer
       └─📦 local-a94336ec0e1b5494d9828849d5--d-transformer-transformer running
          └─🧩 rudderstack/rudder-transformer:latest

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/rudderstack/stack - Inspect logs
        monk shell     local/rudderstack/stack - Connect to the container's shell
        monk do        local/rudderstack/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stored in `manifest.yaml` file.
You can quickly setup by editing the values there.

| Variable        | Description                             | Default |
| --------------- | --------------------------------------- | ------- |
| workspace_token | Your token acquired from ruddestack app | N/A     |

## Stop, remove and clean up workloads and templates

```bash
monk purge    rudderstack/stack
monk purge -x rudderstack/stack
```

## Persistency

If you're using any of the clouds available via Monk. You can use volume definition to spin a disk block device to make your Rudderstack instance independent from the node it's running on.
To do simply uncomment the `volume` block in `manifest.yaml`
