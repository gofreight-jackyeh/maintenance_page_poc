# PoC for Maintenance page

## Directory architecture
```
.
└── nginx
    ├── Chart.lock
    ├── Chart.yaml
    ├── README.md
    ├── templates
    │   ├── hook-iam.yaml
    │   ├── post-upgrade-job.yaml
    │   ├── pre-upgrade-job.yaml
    │   └── ...
    └── ...
```

## Description
- Execution order
  1. hook-iam.yaml
  2. pre-upgrade-job.yaml
  3. post-upgrade-job.yaml

- hook-iam.yaml
  - Create a new role
  - Create a role binding for role and service account
  - Create a new service account for hooks

- pre-upgrade-job.yaml
  - Goal: redirect requests to maintenance page
  - Method: modify the service selector

- post-upgrade-job.yaml
  - Goal: redirect requests to web service
  - Method: modify the service selector

## Installation
- Command
  ``` helm install <RELEASE NAME> <DIR NAME> ``` 


## Upgrade
- If the app doesn't exist, helm will install it automatically
- If the app exists, helm will upgrade it
- Command
  ```helm upgrade -i <RELEASE NAME> <DIR NAME> --atomic```