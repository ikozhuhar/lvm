### Настройка и управление LVM разделами в Linux

![LVM](https://github.com/user-attachments/assets/f25b220b-7585-449b-b6a6-b2cce02bf425)

######  Посмотреть наличие LVM
```php
$ sudo pvs
```

######  Посмотреть Волюм Группы
```php
$ sudo vgs
```

######  Посмотреть список логических томов
```php
$ sudo lvs
```

#### Создание LVM

###### 1. Создание/Удаление физического тома
```php
$ sudo pvcreate /dev/sdb1 /dev/sdb2 /dev/sdb3
$ sudo pvdisplay
```
###### Удаление физического тома
```php
$ sudo pvremove /dev/sdb{1,2,3}
```

###### 2. Создание Волюм Группы
```php
$ sudo vgcreate <name_group> /dev/sdb1 /dev/sdb2 /dev/sdb3
$ sudo vgdisplay
$ sudo pvdisplay
```

###### 3. Создание/Удаление Логического Тома
```php
$ sudo lvcreate <name_group> -n lv01 -L { 100M | 100G } # Первый вариант
$ sudo lvcreate test_group --name lv-01 --size 5G # Второй вариант
$ sudo lvdisplay
$ sudo vgdisplay
```

###### Удаление Логического Тома
```php
$ sudo lvremove test_group lv--02
```

###### Создание нового Тома и перемещение на него 100% свободного места
```php
$ sudo lvcreate <name_group> -n <new_name_logical_tom> -l 100%FREE
$ sudo mkfs.ext4 /dev/mapper/<new_name_logical_tom>
```

###### Вывести Логический Том из группы
```php
$ sudo vgreduce <name_group> /dev/sda1
```

###### Вывести диск из состава LVM
```php
$ sudo pvremove /dev/sda1
```

###### Перенести данные из одного Логического Тома на другой
```php
$ sudo pvmove /dev/sda1 /dev/sda2  
$ sudo pvmove /dev/sda1 # или можно просто освободить нужный диск
```

###### 4. Создание файлововй системы на Логическом Томе
```php
$ sudo mkfs.ext4 /dev/mapper/<name_group>-lv01
```

###### 5. Монтируем
```php
$ sudo mount /dev/mapper/<name_group>-lv01 /mnt/lv-01 
```

###### 6. Добавить места на lv-01
```php
$ sudo lvextend /dev/mapper/<name_group>-lv01 -L +10G  
$ sudo lvextend /dev/mapper/<name_group>-lv01 -L +10G -r # ключ -r запускает resize2fs  
$ sudo resize2fs /dev/mapper/<name_group>-lv01
```


###### Это программа, которая информирует ядро операционной системы об изменениях таблицы разделов, отправляя запрос операционной системе на повторное чтение таблицы разделов
```php
$ sudo partprobe
```

###### Увеличение файловой системы на диске
```php
$ sudo resize2fs /dev/mapper/test_group-lv-01
```

https://www.youtube.com/watch?v=9ttk2-BHMSM
