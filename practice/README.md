# Ansible: Practice

Автопроверка практических заданий(_на данный момент является дополнительной нагрузкой, не учитывается в прогрессе по курсу_)

## Рабочий процесс с использованием автотестов

Для работы с тестированием через pipeline требуется:

- Скачать репозиторий
    ```bash
    git clone https://gitlab.slurm.io/edu/ansible_course.git
    cd ansible_course
    ```

- Создать ветку студента локально
    ```bash
    git checkout -b s061840
    ```

- Добавить ветку студента на удаленный репозиторий
    ```bash
    git push origin s061840:s061840
    ```

- Указать адреса виртуальных машин уже запущенного стенда в _hosts.ini_
    - После создания стенда, аутентификация производится по SSH на adminbox с адресом _sbox.slurm.io_, с помощью логина и пароля, находящихся в настройках профиля или над текстом по кнопке "Доступы"
    - При запуске стенд состоит из 2-х узлов: _node-1.sXXXXXX_, _node-2.sXXXXXX_(где XXXXXX - ваш номер студента). 
    - Далее, с adminbox, подключение к первой ноде ```ssh node-1.sXXXXXX``` и повышение привелегий ```sudo -i ```

- Дополнительно: чтобы не вводить логин и пароль, каждый раз, при отправки изменений, можно закешировать ключи
    ```bash
    git config --global credential.helper 'cache --timeout 7200'
    ``` 

- Каждый отправленный коммит будет инициировать автотест на вкладке _build_ _->_ _pipeline_, по адресу https://gitlab.slurm.io/edu/ansible_course/-/pipelines
    ```bash
    git add .
    git commit -m "bugfix: module: Set value for playbook"
    git push --set-upstream origin s061840
    ```

- После работы удалить локальную ветку можно командами
    ```bash
    git checkout master
    git branch -d s061840
    ```

- А также зачистить удаленную ветку
    ```bash
    git push origin -d s061840
    ```



## Рабочий процесс с использованием vagrant и molecule локально

Для локального тестирования плейбуков через molecule ожидается, что на машине уже установелны _vagrant_ и _molecule_, далее требуется:

- Cкачать репозиторий
    ```bash
    git clone https://gitlab.slurm.io/edu/ansible_course.git
    cd ansible_course/practice/[number_practice]
    ```

- Развернуть виртуальную машину используя Vagrantfile
    ```bash
    vagrat up
    ```

- Указать актуальный адрес виртуальной машины в hosts.ini, после чего создать окружение для molecule
    ```bash
    molecule create
    ```

- Исполнять плейбуки и тестирование используя утилиту molecule
    ```bash
    molecule converge
    molecule verify
    ```

- После работы с окружением зачистить артефакты
    ```bash
    molecule destroy
    vagratn destroy -f
    ```

- Дополнительно: после отладки, можно отвести ветку и отправить исправления в репозиторий на автопроверку по инструкции выше 

## Дополнительно
- Pipeline на проверку запускается при изменении файлов в каталогах practice/3_10 или practice/4_12(напр: для проверки практики по 3-тьему модулю требуются изменения в каталоге practice/3_10)