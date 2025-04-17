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
![1 1](https://github.com/user-attachments/assets/2943642e-3729-4252-88f1-25183f925e65)
![1 2](https://github.com/user-attachments/assets/194c25a5-ae3c-477a-87a4-bd04660612e7)

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

### *Решение:*
#### Файл crontab: [https://github.com/EgorGEgor/HW10.3/blob/main/crontab]
#### Cкрипт: [https://github.com/EgorGEgor/HW10.3/blob/main/every_day_backup.sh]
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

### Результат

