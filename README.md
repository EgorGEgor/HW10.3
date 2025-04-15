# 10.3 Резервное копирование

### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

### *Решение:*
```bash
rsync -a --checksum --verbose --delete --progress --exclude '.*' /home/egor/ /tmp/backup
```




### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

### *Решение:*
#### Файл crontab: [crontab]()
### Результат работы утилиты


### Проверка работы скрипта и утилиты
#### Cкрипт: [every_day_backup.sh]()
```bash
#!/bin/bash
copy=$1
paste=$2

if [ ! -d $copy ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Source directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
elif [ ! -d $paste ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Destination directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
else
	rsync -a --checksum --verbose --delete --progress --exclude '.*' $copy $paste >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
  echo -e "\033[32m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Successfully! | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
fi
```
#### Файл crontab
![png]()

### Результат
#### Файл rsync-cron-2025-04-16.log: [rsync-cron-2025-04-16.log]()
![png]()

---# HW10.3
