# CanaryCD Deployment Example

### Clone example repository
```shell
git clone https://github.com/rehborn/canary-deploy /srv/canary
```

### create config
```shell
cd /srv/canary
cat <<EOF>.env
CANARY_HOST=canary.example.com
CANARY_SALT=$(openssl rand -base64 32) 
EOF
```

### create network
```shell
docker network create traefik
```

### start service
```shell
docker compose up -d
```

### grep API key
```shell
docker compose logs|grep API_KEY
```

### logging
```shell
docker compose logs -f
```

### verify setup

```shell
ss -tlnp|grep -E ':80|:443'
```

## Deploy a Static Page

### create page
```shell
$ canary-cli page create example.com
```

### get deploy token
```shell
$ canary-cli page refresh-token example.com
```

### set host and token
```shell
$ gh variable set CANARY_HOST --body canary.example.com
$ gh secret set CANARY_TOKEN --body BR6N23AZAPoLzpX..
```

### Example Workflow
```yaml
name: Deploy Static Page

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v5

      - name: Deploy
        run: |
          tar cf payload.tar -C public/ .
          curl -X POST -T payload.tar https://${{ vars.CANARY_HOST }}/webhook/page/${{ secrets.CANARY_TOKEN }}
```

