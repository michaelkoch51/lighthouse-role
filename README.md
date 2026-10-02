# lighthouse-role

Роль Ansible для развёртывания [Lighthouse](https://github.com/VKCOM/lighthouse) —
веб-интерфейса для ClickHouse.

## Особенности

Архив Lighthouse копируется с control-node (машины, где запускается Ansible)
через SSH. Это позволяет разворачивать роль на хостах без внешнего
интернета.

## Что делает роль

1. Устанавливает `nginx` и `unzip`.
2. Копирует архив Lighthouse с control-node.
3. Распаковывает его в `/var/www/lighthouse`.
4. Настраивает `nginx` для отдачи статики Lighthouse.
5. Отключает default-сайт nginx.
6. Включает и запускает `nginx`.

## Переменные

| Переменная               | По умолчанию                          | Описание                         |
|--------------------------|---------------------------------------|----------------------------------|
| lighthouse_archive_name  | lighthouse-master.zip                 | Имя архива                       |
| lighthouse_archive_src   | files/{{ lighthouse_archive_name }}   | Путь к архиву на control-node    |
| lighthouse_archive_dest  | /tmp/{{ lighthouse_archive_name }}    | Куда копировать архив на хосте   |
| lighthouse_dir           | /var/www/lighthouse                   | Каталог для распаковки           |
| lighthouse_extracted_dir | /var/www/lighthouse/lighthouse-master | Каталог со статикой (root nginx) |
| nginx_user               | www-data                              | Пользователь nginx               |
| nginx_site_name          | lighthouse                            | Имя конфига сайта nginx          |

## Требования

Файл `files/lighthouse-master.zip` должен находиться рядом с playbook.

## Пример

    - hosts: lighthouse
      become: true
      roles:
        - lighthouse-role
