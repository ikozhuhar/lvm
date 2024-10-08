### Настройка и управление LVM разделами в Linux

#### <a name='toc'>Содержание</a>

1. [Посмотреть наличие LVM](#availability)
2. [Создание физических LVM разделов](#creating_physical_lvm)
3. [Создание группы разделов LVM](#creating_group_lvm)
4. [Создание логических томов LVM](#creating_logical_lvm)
5. [Создание файловой системы на томе](#creating_fs)
6. [Изменение размера LVM тома](#resize_lvm)
7. [Удаление LVM разделов](#delete_lvm)
8. [Автоматизация создания LVM c Ansible](#creating_automated)
9. [Дополнительные источники](#recommended_sources)  

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
![image](https://github.com/user-attachments/assets/29ae64d9-45bc-464f-9a0c-bbd6c8dc1863)
![image](https://github.com/user-attachments/assets/e9dc551d-1db1-4d35-bcc0-eeadd2566c15)
![image](https://github.com/user-attachments/assets/2301d74c-e9d4-454d-b1d5-71ae044fb0ac)

##### 5. [[⬆]](#toc) <a name='creating_fs'>Создадим на LV файловую систему</a>
```
sudo mkfs.ext4 /dev/mapper/otus_volume-lv01
sudo mkfs.ext4 /dev/mapper/otus_volume-lv02
sudo mkfs.ext4 /dev/mapper/otus_volume-lv03
sudo mkfs.ext4 /dev/mapper/otus_volume-lv04
sudo mkfs.ext4 /dev/mapper/otus_volume-lv05
sudo mkfs.ext4 /dev/mapper/otus_volume-lv06
sudo mkfs.ext4 /dev/mapper/otus_volume-lv07
sudo mkfs.ext4 /dev/mapper/otus_volume-lv08
sudo mkfs.ext4 /dev/mapper/otus_volume-lv09
sudo mkfs.ext4 /dev/mapper/otus_volume-lv010
```
![image](https://github.com/user-attachments/assets/a92a90c4-cd83-4c7c-bcf1-a84b81e925ec)

##### Создадим директории и смонтируем
```
sudo mkdir /mnt/lv{01,02,03,04,05,06,07,08,09,10}

sudo mount /dev/mapper/otus_volume-lv01 /mnt/lv01
sudo mount /dev/mapper/otus_volume-lv02 /mnt/lv02
sudo mount /dev/mapper/otus_volume-lv03 /mnt/lv03
sudo mount /dev/mapper/otus_volume-lv04 /mnt/lv04
sudo mount /dev/mapper/otus_volume-lv05 /mnt/lv05
sudo mount /dev/mapper/otus_volume-lv06 /mnt/lv06
sudo mount /dev/mapper/otus_volume-lv07 /mnt/lv07
sudo mount /dev/mapper/otus_volume-lv08 /mnt/lv08
sudo mount /dev/mapper/otus_volume-lv09 /mnt/lv09
sudo mount /dev/mapper/otus_volume-lv10 /mnt/lv10
```
##### Смотрим результат

![image](https://github.com/user-attachments/assets/a39d736e-94d4-4926-af2a-f9a2354cca4e)

#### 6. [[⬆]](#toc) <a name='Изменение размера LVM тома'>Изменение размера LVM тома</a>

##### Допустим, перед нами встала проблема нехватки свободного места в директории //mnt/lv01. Мы можем расширить файловую систему на LV /dev/mapper/otus_volume-lv01 за счет нового блочного устройства.

##### Для начала так же необходимо создать PV:
```
sudo pvcreate /dev/sdh
```

##### Далее необходимо расширить VG добавив в него этот диск
```
sudovgextend otus_volume /dev/sdh
```

##### Убедимся что новый диск присутствует в новой VG:
```
sudo vgdisplay -v otus_volume | grep 'PV Name'
```

##### И что места в VG прибавилось
```
sudo vgs
```
##### Добавить места на lv01
```
$ sudo lvextend /dev/mapper/otus_volume-lv01 -L +10G  
$ sudo lvextend /dev/mapper/otus_volume-lv01 -L +10G -r # ключ -r запускает resize2fs  
$ sudo resize2fs /dev/mapper/otus_volume-lv01
```

#### 7. [[⬆]](#toc) <a name='delete_lvm'>Удаление LVM разделов</a>

##### Удаление физического тома
```
sudo pvremove /dev/sdb{1,2,3}
```

##### Удаление Волюм Группы
```
sudo vgremove otus_volume
sudo vgdisplay
```

##### Удаление Логического Тома
```
sudo lvremove otus_volume lv01
```

##### Создание нового Тома и перемещение на него 100% свободного места
```
sudo lvcreate <name_group> -n <new_name_logical_tom> -l 100%FREE
sudo mkfs.ext4 /dev/mapper/<new_name_logical_tom>
```

##### Вывести Логический Том из группы
```
sudo vgreduce <name_group> /dev/sda1
```

##### Вывести диск из состава LVM
```
sudo pvremove /dev/sda1
```

##### Перенести данные из одного Логического Тома на другой
```
sudo pvmove /dev/sda1 /dev/sda2  
sudo pvmove /dev/sda1 # или можно просто освободить нужный диск
```

##### Это программа, которая информирует ядро операционной системы об изменениях таблицы разделов,  отправляя запрос операционной системе на повторное чтение таблицы разделов
```
sudo partprobe
```

##### Увеличение файловой системы на логическом разделе
```
sudo resize2fs /dev/mapper/volume_group-lv01
```

#### 8. [[⬆]](#toc) <a name='creating_automated'>Автоматизация создания LVM c Ansible</a>

##### Структура
![image](https://github.com/user-attachments/assets/e3bbaec8-ecd9-43f8-9652-4729e5af2f68)

##### Запускаю
```
ansible-playbook playbook_lvm.yml
```


#### 9. [[⬆]](#toc) <a name='recommended_sources'>Дополнительные источники</a>

1. Весь Linux Для тех, кто хочет стать профессионалом, стр.302

> Сделано по книге

##### Создаем физический том (PV)
```
pvcreate /dev/sdb1 /dev/sdb2 /dev/sdb3
```

##### Теперь нужно создать группу томов с помощью команды vgcreate
```
vgcreate my_vg /dev/sdb
```

##### Создаем отдельные логические тома (команда lvcreate)
```
lvcreate -n swap -L8G my_vg
lvcreate -n home -L500G my_vg
lvcreate -n var -LЗOG my_vg
lvcreate -n tmp -LSG my_vg
```

##### Созданные нами разделы будут храниться в папке /dev/my _ vg/.  В этом каталоге вы найдете файлы home, tmp, var (правда, это будут ссылки, а не файлы, но суть от этого не меняется). 

##### Когда созданы логические тома, можно создать на них файловые системы (отформатировать их). Вы можете использовать любую файловую си.стему, я предпочитаю ext4: 
```
mkfs.ext4 -L var /dev/my_vg/var
mkfs.ext4 -L home /dev/my_vg/home
mkfs.ext4 -L tmp /dev/my_vgftmp
mkswap -L swap /dev/my_vg/swap
swapon /dev/my_vg/swap
```

##### Первые три команды создают файловую систему ext4 на устройствах /dev/my_vg/var, /dev/my_vg/home и /dev/my_vg/tmp. Последние две создают раздел подкачки и активируюг его.
```
mkdir /mnt/home
mkdir /mnt/var

mount /dev/my_vg/home /mnt/home
mount /dev/my_vg/var /mnt/var
```

##### Здесь копируется все для переноса папки home и var со старого диска на новый. Подробнее описано в книге Весь Linux Для тех, кто хочет стать профессионалом, стр.302
```
cр -а /home/* /mnt/home
cp -а /var/* /mnt/var

umount /mnt/home
umount /mnt/var
```

##### Почти все. Теперь нужно добавить в /etc/fstab записи, монтирующие файловые системы /home, /var, /tmp, и указать в нем раздел подкачки:
```
/dev/mapper/my_vg-home /home ext4 relatime 1 1
/dev/mapper/my_var /var ext4 relatime 1 1
/dev/m�pper/my_vg-tmp /tmp ext4 noatime 0 2
/dev /mapper /my _ vg-swap none swap SW 0 0
```
