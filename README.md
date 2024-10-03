### Настройка и управление LVM разделами в Linux

#### <a name='toc'>Содержание</a>

1. [Посмотреть наличие LVM](#availability)
2. [Создание физических LVM разделов](#creating_physical_lvm)
3. [Создание группы разделов LVM](#creating_group_lvm)
4. [Создание логических томов LVM](#creating_logical_lvm)
5. [Изменение размера LVM тома](#resize_lvm)
6. [Удалить LVM раздел](#delete_lvm)

![60d48245-d1a6-4f9f-bbed-fa233881a859](https://github.com/user-attachments/assets/ec1dee5e-20df-4dec-98e6-e0aeaaa688b5)


#### 1. [[⬆]](#toc) <a name='availability'>Посмотреть наличие LVM</a>

##### &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
![image](https://github.com/user-attachments/assets/c2d27d37-a182-4ef0-8105-b2d43056856f)

##### &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
![image](https://github.com/user-attachments/assets/072f5c4b-0d9b-46f3-abbf-3b68c9ed2b39)

#### 2. [[⬆]](#toc) <a name='creating_physical_lvm'>Создание физических LVM разделов</a>

#### 3. [[⬆]](#toc) <a name='creating_group_lvm'>Создание группы разделов LVM</a>

#### 4. [[⬆]](#toc) <a name='creating_logical_lvm'>Создание логических томов LVM</a>

#### 5. [[⬆]](#toc) <a name='Изменение размера LVM тома'>Изменение размера LVM тома</a>

#### 6. [[⬆]](#toc) <a name='delete_lvm'>Удалить LVM раздел</a>













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

###### Удаление Волюм Группы
```php
$ sudo vgremove <name_group>  
$ sudo vgdisplay
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


###### Это программа, которая информирует ядро операционной системы об изменениях таблицы разделов,  отправляя запрос операционной системе на повторное чтение таблицы разделов
```php
$ sudo partprobe
```

###### Увеличение файловой системы на диске
```php
$ sudo resize2fs /dev/mapper/test_group-lv-01
```

https://www.youtube.com/watch?v=9ttk2-BHMSM
