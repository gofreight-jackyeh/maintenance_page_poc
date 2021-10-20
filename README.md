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

## Flow
1. Redirect requests to maintenance page (pre-upgrade)
2. Stop web service (pre-upgrade)
3. Run DB migration (pre-upgrade)
4. Upgrade helm package
5. Start web service (post-upgrade)
6. Redirect requests to web service (pre-upgrade)

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
  - Goal
      1. Redirect requests to maintenance page
      2. Stop web service
      3. Run DB migration

  - Action
      1. Modify the service selector
      2. Set deployment replicas count to 0
      3. Launch an app pod to run db migration command

- post-upgrade-job.yaml
  - Goal: 
      1. Start web service
      2. Redirect requests to web service

  - Action
      1. Modify the service selector
      2. Set deployment replicas count to N
## Installation
- Command
  ``` helm install <RELEASE NAME> <DIR NAME> ``` 


## Upgrade
- If the app doesn't exist, helm will install it automatically
- If the app exists, helm will upgrade it
- Command
  ```helm upgrade -i <RELEASE NAME> <DIR NAME> --atomic```