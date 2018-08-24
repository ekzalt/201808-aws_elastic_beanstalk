# Deploy flow

## AWS Elastic Beanstalk config example

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

## AWS - Хранение Секретов - System Manager Parameter Store

Example of Parameter Store Role

```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ssm:GetParameters",
      "Resource": "*"
    }
  ]
}

```

### Export variables from AWS Parameter Store to Application env

`aws ssm get-parameters --name <ParameterStoreName:DBendpoint> --region <AWSregion:us-east-1> --output <OutputFormat:text> --query <DataFromObject:Parameters[].Value>`

Example of Parameter Store DB url data:

```json

{
  "InvalidParameters": [],
  "Parameters": [
    {
      "Version": 2,
      "Type": "String",
      "Name": "DBendpoint",
      "Value": "mysql-server.1234567890xxx.us-east-1.rds.amazonaws.com"
    }
  ]
}

```

Examples of exporting variables to env:

```bash

export DB_URL=`aws ssm get-parameters --name DBendpoint --region us-east-1 --output text --query Parameters[].Value`
export DB_USER=`aws ssm get-parameters --name DBusername --region us-east-1 --output text --query Parameters[].Value`
export DB_PASS=`aws ssm get-parameters --name DBpassword --region us-east-1 --with-decryption --output text --query Parameters[].Value`

```
