# common-soft
## Описание
<details><summary> Группировка серверов:</summary>

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

### Список программ и утилит по группам

servers

| Soft           | Зависимость для                | Comment                                                                                         |
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

control_all

| Soft       | Зависимость для | Comment                                |
|:-----------|:----------------|:---------------------------------------|
| keepalived | kubernetes      | Виртуальный ip для высокой доступности |


## Dependency
| Дополнительно | Значение                                              | Comment |
|:--------------|:------------------------------------------------------|:--------|
| Образ         | [ansible](https://github.com/FZEN475/ansible-image)   |         |
| Библиотеки    | [Library](https://github.com/FZEN475/ansible-library) |         |

### Stages
* [Общее](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/playbooks/_1_install_soft.yaml#L6-L29)
* 
* [Настройка containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
- [allow forward IPv4](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/playbooks/_3_keepalived/_0_install.yaml#L4-L9)
- [config.toml](https://github.com/FZEN475/common-soft/blob/main/config/config.toml)
  - [SystemdCgroup = true](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/config/config.toml#L139) для использования systemd в качестве драйвера cgroup для среды выполнения контейнера.  

[kubeadm](https://github.com/FZEN475/common-soft/blob/main/playbooks/_2_kubeadm/_0_install.yaml)
- [Установка helm через ссылку.](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/playbooks/_2_kubeadm/_0_install.yaml#L15-L19)

[keepalived](https://github.com/FZEN475/common-soft/blob/main/playbooks/_3_keepalived/_0_install.yaml)
- [allow binding non-local IPv4](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/playbooks/_3_keepalived/_0_install.yaml#L4-L9).

### Troubleshoots

<!DOCTYPE html>
<table>
  <thead>
    <tr>
      <th>Проблема</th>
      <th>Решение</th>
    </tr>
  </thead>
  <tr>
      <td>Бывает, что после установки не монтируются виртуальные диски.</td>
      <td>
[Решение](https://github.com/FZEN475/common-soft/blob/05c58c7e3d0c3a574b7ed18423365080d062e437/playbooks/_1_swarm/_0_install.yaml#L22-L32)
</td>
  </tr>
  <tr>
  </tr>
</table>





