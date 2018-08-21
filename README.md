# AWS Elastic Beanstalk config example

Полная Кастомизация через папку `.ebextensions`
В папке должны храниться файлы с расширением `*.config`

Документация:
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html

Группы комманд в config файлах:
- packages – скачать и инсталировать прораммы (yum, rpm, msi)
- sources  - скачать архив из инета и распаковать (tar, gzip, zip)
- files    – создать файлы (можно скачать используя source)
- users    – создать пользователей, только на Linux
- groups   – создать группы, только на Linux
- commands – запустить системные комманды перед распаковкой zip файла
- container_commands – запустить системные команды после распаковки
- services  – старт, стоп сервисов
- Resources – создание дополнительных ресурсов AWS

Организация последовательности операций в `.ebextensions`:
- `01_create_folders.config`
- `02_copy_apache_config_files.config`
- `03_create_s3bucket.config`
- `04_install_dependencies.config`

Остальные примеры в папке `.ebextensions`
