# CanaryCD Deployment Example

clone repo
```shell
git clone https://github.com/rehborn/canary-deploy && cd canary-deploy
```

create config
```shell
echo "CANARY_HOST=canary.example.com" >> .env
echo "CANARY_SALT=$(openssl rand -base64 32)" >> .env
```

**create `traefik` network**

```shell
docker network create traefik
```

**start**
```shell
docker compose up -d
```
