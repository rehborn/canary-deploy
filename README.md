# CanaryCD Deployment Example

clone repo
```shell
git clone https://github.com/rehborn/canary-deploy -b dev
```

create config
```shell
cd canary-deploy
cp .env-example .env
echo "CANARY_SALT=$(openssl rand -base64 32)" > .env
```

start
```shell
source .env ; docker compose up -d
```
