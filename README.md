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


## Подготовка
### Требования
| Дополнительно | Значение                                              | Comment |
|:--------------|:------------------------------------------------------|:--------|
| Образ         | [ansible](https://github.com/FZEN475/ansible-image)   |         |
| Библиотеки    | [Library](https://github.com/FZEN475/ansible-library) |         |

## Установка
Запустить docker-compose.yml

### Дополнительно

[Настройка containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
- [allow forward IPv4]()ССЫЛКА!
- [config.toml]() ССЫЛКА!
  - [SystemdCgroup = true]()ССЫЛКА! для использования systemd в качестве драйвера cgroup для среды выполнения контейнера.  

AppArmor
- Бывает, что после установки не монтируются виртуальные диски. [Решение]()ССЫЛКА!  

helm
- [Установка через ссылку.]()ССЫЛКА! Там более новая версия.

keepalived
- [allow binding non-local IPv4]()ССЫЛКА!

### Ошибки

<!DOCTYPE html>
<table>
  <thead>
    <tr>
      <th>Проблема</th>
      <th>Решение</th>
    </tr>
  </thead>
  <tr>
      <td>Из контейнера не определяются имена серверов без домена.</td>
      <td>
На хосте докера:  
/etc/docker/daemon.json

```json
{
  "insecure-registries":["192.168.2.10:5000"],
  "dns": ["192.168.2.1","8.8.8.8"]
}
```
</td>
  </tr>
  <tr>
  </tr>
</table>





