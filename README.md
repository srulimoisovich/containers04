# Лабораторная работа №4

**Тема:** запуск Apache HTTP Server внутри контейнера Ubuntu  
**Студент:** Bordeniuc Ivan  
**Группа:** I2402  

## Задача работы

Нужно запустить контейнер Ubuntu, установить в нем Apache, опубликовать порт контейнера локально и заменить стартовую страницу на `Hello, World!`.

## Выполнение

Контейнер запускается в интерактивном режиме:

```text
docker run -ti -p 8000:80 --name containers04 ubuntu bash
```

Параметр `-p 8000:80` означает, что порт 80 внутри контейнера доступен на порту 8000 хоста.

В контейнере выполняются команды:

```text
apt update
apt install apache2 -y
service apache2 start
```

Команда `apt update` обновляет список пакетов, `apt install apache2 -y` устанавливает веб-сервер, а `service apache2 start` запускает службу Apache.

После этого в браузере открывается:

```text
http://localhost:8000
```

На первом этапе видна стандартная страница Apache.

## Изменение страницы

Содержимое каталога сайта можно проверить так:

```text
ls -l /var/www/html/
```

Ожидаемый вывод:

```text
total 12
-rw-r--r-- 1 root root 10671 Apr 18 12:02 index.html
```

Затем стартовый файл заменяется:

```text
echo '<h1>Hello, World!</h1>' > /var/www/html/index.html
```

После обновления страницы браузер показывает:

```text
Hello, World!
```

## Конфигурация Apache

Файл виртуального хоста открывается командой:

```text
cd /etc/apache2/sites-enabled/
cat 000-default.conf
```

Основные строки:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

`DocumentRoot /var/www/html` показывает, из какой папки Apache отдает HTML-файлы.

## Завершение работы

После выхода из контейнера:

```text
exit
```

можно посмотреть все контейнеры и удалить учебный:

```text
docker ps -a
docker rm containers04
containers04
```

## Ответы на вопросы

**Что видно при первом открытии страницы?**  
Открывается стандартная страница Apache, которая создается после установки пакета.

**Что изменилось после команды `echo`?**  
Стандартный файл был перезаписан, и вместо него браузер показал заголовок `Hello, World!`.

**Для чего нужен файл `000-default.conf`?**  
Он описывает виртуальный хост Apache по умолчанию: корневую папку сайта и файлы для ошибок и доступа.

## Заключение

В работе был разобран простой сценарий: контейнер Ubuntu используется как временная Linux-среда, в нем устанавливается Apache и публикуется веб-страница на локальный порт.

## Источники

- Docker Documentation: `docker run`, port publishing.
- Ubuntu/Debian manuals: `apt`, `apache2`.
