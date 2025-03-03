# common-soft
## Description
* Зависимости для ansible.
* Виртуализация и swarm.
* Kubernetes
* Балансировка
<details><summary> Структура серверов:</summary>

```yaml
control_all:
  children:
    control_main:
      - control01
    control:
      - control02
      - control03

servers:
  children:
    control_main:
    control:
    storage:
      - storage01
    dev:
      - dev01
    prod:
      - prod01

workers:
  children:
    storage:
      - storage01
    dev:
      - dev01
    prod:
      - prod01
```
</details>

## Dependency

* [Образ](https://github.com/FZEN475/terraform-image.git)
* [Library](https://github.com/FZEN475/ansible-library)

<details><summary> .env </summary>

```properties
TERRAFORM_REPO="https://github.com/FZEN475/common-soft.git"
#GIT_EXTRA_PARAM="-btemp_branch"
SECURE_SERVER=""
SECURE_PATH=""
LIBRARY="https://github.com/FZEN475/ansible-library.git"
```
</details>

### Stages
* [Общее](https://github.com/FZEN475/common-soft/blob/dev/playbooks/_0_common/_0_install.yaml).
  * [Расширение диска](https://github.com/FZEN475/ansible-library?tab=readme-ov-file#disk_resize).
  * Установка python и модулей для ansible.
  * Forwarding между подсетями.
* [Swarm](https://github.com/FZEN475/common-soft/blob/dev/playbooks/_1_swarm/_0_install.yaml).
  * [Apparmor](https://github.com/FZEN475/common-soft?tab=readme-ov-file#Troubleshoots).
* [kubeadm](https://github.com/FZEN475/common-soft/blob/main/playbooks/_2_kubeadm/_0_install.yaml)
  * Установка helm cni-plugins и calicoctl.
    * cni-plugins требует перезапуска containerd.
  * [Настройка containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
    * [config.toml](https://github.com/FZEN475/common-soft/blob/main/config/config.toml)
      * Использование systemd в качестве драйвера cgroup для среды выполнения контейнера.
        ```
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
        ...
        SystemdCgroup = true
        ```
* [keepalived](https://github.com/FZEN475/common-soft/blob/main/playbooks/_3_keepalived/_0_install.yaml)

## Results

* servers

| Soft           | Dependency for                 | Comment                                                                                         |
|:---------------|:-------------------------------|:------------------------------------------------------------------------------------------------|
| python3-pip    | ansible                        | Менеджер пакетов python                                                                         |
| pip openshift  | ansible                        | Клиент python для [openshift](https://www.redhat.com/en/technologies/cloud-computing/openshift) | 
| pip pyyaml     | ansible                        | Парсер YAML                                                                                     | 
| pip kubernetes | ansible                        | Клиент python для kubernetes                                                                    | 
| pip docker     | ansible                        | Клиент python для kubernetes docker                                                             | 
| pip jsondiff   | ansible                        | Сравнение двух объектов JSON                                                                    | 
| apparmor       | docker, containerd, kubernetes | Модуль безопасности                                                                             | 
| apparmor-utils | apparmor                       | Наборы политик apparmor                                                                         | 
| containerd.io  | docker, kubernetes             | Система управления контейнерами                                                                 | 
| docker-ce      | docker-ce-cli                  | Docker engin (api между containerd и docker-ce-cli)                                             | 
| docker-ce-cli  | ---                            | Интерфейс командной строки                                                                      | 
| kubelet        | kubernetes                     | Демон мониторинга (controller manager)                                                          | 
| kubeadm        | kubernetes                     | Утилита развертывания кластера                                                                  | 
| kubectl        | kubernetes                     | Интерфейс командной строки                                                                      | 
| socat          | ---                            | Проброс сетевого трафика (для тестов)                                                           | 
| helm           | kubernetes                     | Менеджер чартов                                                                                 | 

* control_all

| Soft       | Зависимость для | Comment                                |
|:-----------|:----------------|:---------------------------------------|
| keepalived | kubernetes      | Виртуальный ip для высокой доступности |



## Troubleshoots

<!DOCTYPE html>
<table>
  <thead>
    <tr>
      <th>Проблема</th>
      <th>Решение</th>
    </tr>
  </thead>
  <tr>
      <td>Бывает, что после установки не запускается apparmor</td>
      <td>

Перезагрузка.
</td>
  </tr>
  <tr>
  </tr>
</table>





