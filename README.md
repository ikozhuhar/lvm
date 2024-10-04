### Настройка и управление LVM разделами в Linux

#### <a name='toc'>Содержание</a>

1. [Посмотреть наличие LVM](#availability)
2. [Создание физических LVM разделов](#creating_physical_lvm)
3. [Создание группы разделов LVM](#creating_group_lvm)
4. [Создание логических томов LVM](#creating_logical_lvm)
5. [Изменение размера LVM тома](#resize_lvm)
6. [Удалить LVM раздел](#delete_lvm)

https://losst.pro/sozdanie-i-nastrojka-lvm-linux

![60d48245-d1a6-4f9f-bbed-fa233881a859](https://github.com/user-attachments/assets/ec1dee5e-20df-4dec-98e6-e0aeaaa688b5)


#### 1. [[⬆]](#toc) <a name='availability'>Смотрим наличие LVM</a>

#####  Физические разделы
```
sudo pvs
sudo pvdisplay
```
#####  Группы разделов
```
sudo vgs
sudo vgdispaly
```
#####  Логические тома
```
sudo lvs
sudo lvdispaly
```
#####  Другие варианты
```
sudo lvs -o lv_name,vg_name,lv_attr,seg_pe_ranges
sudo alias llvs='lvs -o lv_name,vg_name,lv_attr,seg_pe_ranges '
```
![image](https://github.com/user-attachments/assets/c79785d9-83f0-4493-8dc5-79cd37ac93d3)


#### 2. [[⬆]](#toc) <a name='creating_physical_lvm'>Создание физических LVM разделов</a>

##### Создание физического раздела
```
sudo pvcreate /dev/sde1 /dev/sdf1 /dev/sdg1
```
![image](https://github.com/user-attachments/assets/99c813ab-8124-4bdb-8740-9e89a94f861c)
![image](https://github.com/user-attachments/assets/12175700-9718-46de-a1f5-923750227507)

##### Смотрим результат

```
sudo pvdisplay
sudo pvs
```
![image](https://github.com/user-attachments/assets/002e813d-e019-4847-881b-c4fc12545333)
![image](https://github.com/user-attachments/assets/a48bf118-271b-4f4b-a008-55e49b12542e)


#### 3. [[⬆]](#toc) <a name='creating_group_lvm'>Создание группы разделов LVM</a>

##### Создание группы
```
sudo vgcreate otus_volume /dev/sde1 /dev/sdf1 /dev/sdg1
```
![image](https://github.com/user-attachments/assets/ed899482-3c27-4792-a210-6e5b3589cb62)

##### Смотрим результат
```
sudo vgs
sudo vgdisplay
```
![image](https://github.com/user-attachments/assets/913322b9-6133-429c-954c-dab47bdda5f0)

#### 4. [[⬆]](#toc) <a name='creating_logical_lvm'>Создание логических томов LVM</a>

##### Создание логических томом
```
sudo lvcreate otus_volume --name lv01 --size 5G
sudo lvcreate otus_volume --name lv02 --size 10G
sudo lvcreate otus_volume --name lv03 --size 8G
sudo lvcreate otus_volume --name lv04 --size 6G
sudo lvcreate otus_volume --name lv05 --size 10G
sudo lvcreate otus_volume --name lv06 --size 10G
sudo lvcreate otus_volume --name lv07 --size 10G
sudo lvcreate otus_volume --name lv08 --size 10G
sudo lvcreate otus_volume --name lv09 --size 10G
sudo lvcreate otus_volume --name lv10 --size 10G
```
![image](https://github.com/user-attachments/assets/49af73c8-2541-40e2-89bb-21163b08836e)

##### Смотрим результат
```
sudo lvs
sudo lvdisplay
lsblk
```
![image](https://github.com/user-attachments/assets/b83a0dd3-5a10-46b5-92b7-9025f0b46232)
![image](https://github.com/user-attachments/assets/e9dc551d-1db1-4d35-bcc0-eeadd2566c15)
![image](https://github.com/user-attachments/assets/2301d74c-e9d4-454d-b1d5-71ae044fb0ac)


















#### 5. [[⬆]](#toc) <a name='Изменение размера LVM тома'>Изменение размера LVM тома</a>

#### 6. [[⬆]](#toc) <a name='delete_lvm'>Удалить LVM раздел</a>






#### Создание LVM

###### Удаление физического тома
```php
$ sudo pvremove /dev/sdb{1,2,3}
```


###### Удаление Волюм Группы
```php
$ sudo vgremove <name_group>  
$ sudo vgdisplay
```

###### 3. Создание/Удаление Логического Тома

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
