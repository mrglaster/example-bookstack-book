## Настройка деплоя воркера на виртуальной машине

### Аренда виртуальной машины 

Воркер и его окружение не являются требовательным к ресурсам. Достаточной является виртуальная машина со следующими характеристиками 

![img](https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=ksnip_20241202-144339.png)

В качестве операционной системы **рекомендуется** использовать Ubuntu Server или AlmaLinux 8

![img](https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=images.png)  ![img](https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=ksnip_20241202-144550.png)

### Настройка виртуальной машины 

1) Необходимо установить **wget**, ели он ещё не установлен. Делается это при помощи команды ```sudo apt install wget```, если вы используете Ubuntu. Если вы используете **AlmaLinux**, воспользуйтесь командой ```dnf install wget``` или ```yum install wget```

2) Теперь необходимо скачать скрипт для настройки. Если вы используете Ubuntu, воспользуйте команду 

```
wget https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=configure_runner_ubuntu.sh
```

Если вы используете AlmaLinux используйте команду 

```
https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=configure_runner_alma.sh
```


3) Предоставьте скрипту права на исполнение 

```
sudo chmod +x configure_runner_ubuntu.sh
```
или

```
sudo chmod +x configure_runner_alma.sh
```

4) Запустите скрипт командой 

```
./configure_runner_ubuntu.sh GITLAB_RUNNER_TOKEN
```

Информацию о том, где получить токен для GitLab Runner можно  найти здесь:  ![GitLab Runner Configuration Guide](https://git.iszf.irk.ru/demo-runner-group/gitlab-ci-demos/gitlab-runner-configuration-guide)


5) Добавьте следующую информацию в **.gitlab-ci.yml**

```
deploy-production-worker-new:
    stage: deploy
    tags:
        - RUNNER_NAME
    image: docker/compose:1.25.1
    when: manual
    script:
        - docker stop worker || true
        - docker rm worker|| true
        - docker-compose stop
        - docker-compose up -d --build
        - docker container ls

```

Где ***RUNNER NAME*** - имя раннера, которое вы задали на моменте создания 

6) Можно проводить деплой и использовать воркер 

![img](https://cloud.iszf.irk.ru/index.php/s/cmiZF0NtaHRq5v8/download?path=%2F&files=ksnip_20241202-152214.png)

