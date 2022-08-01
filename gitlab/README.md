image — образ докера, который мы хотим использовать на нашем сервере

порты — список портов, которые мы делаем доступными вне контейнера. В нашей конфигурации мы предоставляем порты 80, 443 (сайт)

container_name — имя нашего контейнера

тома — указывает тома, которые используются контейнером. В нашей конфигурации у нас есть общие с нашей системой каталоги и дополнительный том, который разрешает доступ к среде Docker из GitLab runner.

network — определяет виртуальную сеть, в которой будут работать контейнеры. В нашем случае портал www и раннер работают в одной «gitlab-сети»

external_url - по этому адресу будет доступен гитлаб, я ставлю на виртуалке поэтому будет использоваться ip адрес.

```
docker-compose up -d
```

чтобы зайти используем пользователя root и пароль ниже:
```
docker exec -it gitlab-ce grep 'Password:' /etc/gitlab/initial_root_password
```

подробная информация тут:
```
https://sidmid.ru/gitlab-in-docker-compose/
```

регаем раннер после чего правим конфиг:
gitlab/gitlab/gitlab-runner/config.toml

```
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "docker"
  url = "http://gitlab.local"
  token = "KgF9Prs4K8dz5jhTs4LS"
  executor = "docker"
  clone_url = "http://gitlab.local"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "docker:20.10.16"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
    network_mode = "gitlab-network"

```
а именно добавляем 
network_mode = "gitlab-network"
и 
privileged = true


не забываем добавить в .gitlab-ci.yaml в variable

```
variables:
  # By default, the helper image is pulled from Docker Hub. Use gitlab docker regestry instead of dockerhub
  FF_GITLAB_REGISTRY_HELPER_IMAGE: 1
  DOCKER_HOST: tcp://docker:2375
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: ""
  DOCKER_BUILDKIT: 1
  COMPOSE_DOCKER_CLI_BUILD: 1
  DOCKER_CONTAINER_REGISTRY: registry.gitlab.local:5005
  DOCKER_REGISTRY: $DOCKER_CONTAINER_REGISTRY/test/backend
```

а в gitlab-ci.yaml для service добавляем:
```
  services:
   - name: docker:dind
     alias: docker
     entrypoint: ["dockerd-entrypoint.sh", "--tls=false", "--insecure-registry=registry.gitlab.local:5005"]
```




