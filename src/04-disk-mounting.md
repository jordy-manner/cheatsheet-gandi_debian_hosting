# 04 - Montage des disques

## Préambule - Architecture des volumes du serveur

- Le disque système contient l'ensemble des services installés.
- Le disque de données contient l'ensemble des données (sites web, FTP, configuration, journaux etc.), de manière à offrir une meilleure scalabilité et portabilité.
- Le disque de sauvegarde contient une sauvegarde des données sensible des disques de données et système.

## Contrôle des partitions

Contrôle de présence physique :

```bash
sudo cat /proc/partitions
```

*Les disques suivants **xvdb** et **xvdc** devraient respectivement correspondre au disque de données et au disque de sauvegarde*.

```
major minor  #blocks  name

 202        0   20971520 xvda
 202        1   20970479 xvda1
 202       16   52428800 xvdb
 202       32   83886080 xvdc
 202     6400    1048576 xvdz
 202     6401    1038270 xvdz1
 202     6402      10240 xvdz2
```

Contrôle de la taille des volumes :

```bash
sudo fdisk -l
```

Retourne :

```
Disk /dev/xvda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 32768 bytes
I/O size (minimum/optimal): 32768 bytes / 32768 bytes
Disklabel type: gpt
Disk identifier: 29608333-E331-423E-B854-14405F0A931B

Device     Start      End  Sectors Size Type
/dev/xvda1  2048 41943006 41940959  20G Linux filesystem


Disk /dev/xvdb: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 32768 bytes
I/O size (minimum/optimal): 32768 bytes / 32768 bytes


Disk /dev/xvdc: 80 GiB, 85899345920 bytes, 167772160 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 32768 bytes
I/O size (minimum/optimal): 32768 bytes / 32768 bytes


Disk /dev/xvdz: 1 GiB, 1073741824 bytes, 2097152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 32768 bytes
I/O size (minimum/optimal): 32768 bytes / 32768 bytes
Disklabel type: gpt
Disk identifier: 7C6072E5-2512-11EC-B712-AC1F6B0FFF66

Device       Start     End Sectors  Size Type
/dev/xvdz1      64 2076604 2076541 1014M FreeBSD swap
/dev/xvdz2 2076608 2097087   20480   10M Microsoft basic data
```

Contrôle de montage :

```bash
sudo df -h
```

*Les disques suivants **xvdb** et **xvdc** devraient être absents des points de montage*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            2,0G     0  2,0G   0% /dev
tmpfs           400M  6,6M  393M   2% /run
/dev/xvda1       20G  1,6G   18G   9% /
tmpfs           2,0G     0  2,0G   0% /dev/shm
tmpfs           5,0M     0  5,0M   0% /run/lock
tmpfs           2,0G     0  2,0G   0% /sys/fs/cgroup
tmpfs            24K     0   24K   0% /var/gandi
```

Contrôle de formatage :

```bash
sudo blkid
```

*Les disques suivants **xvdb** et **xvdc** devraient être formatés en ext4*.

```bash
/dev/xvdz1: LABEL="swap" UUID="7c558b77-535b-469e-9b34-c9fff9fbde37" TYPE="swap" PARTLABEL="gandiswap" PARTUUID="3c2eceb4-5157-497a-a706-d8d21a2831c8"
/dev/xvda1: LABEL="debian-buster" UUID="b4280e48-4ebd-4b0c-a3e1-d5b040e61364" TYPE="ext4" PARTLABEL="gandiroot" PARTUUID="d6c8e31a-baf3-492e-9cf6-2877b177572a"
/dev/xvdb: LABEL="h01-datas" UUID="daa5bc7e-c5ea-4b92-b59e-ee9e72043a1a" SEC_TYPE="ext2" TYPE="ext3"
/dev/xvdc: LABEL="h01-backup" UUID="acfda76d-da0f-41be-887d-61dce78457a7" SEC_TYPE="ext2" TYPE="ext3"
/dev/xvdz2: PARTLABEL="gandiconfig" PARTUUID="3fd30f26-50b7-4e6f-bb77-3c042f302fab"
```

## Formatage et label des disques

### Disque de données

```
sudo mkfs.ext4 /dev/xvdb -L {{ (h|r)## }}-datas
```

Vérifier et taper ```y``` pour valider.

```bash
mke2fs 1.44.5 (15-Dec-2018)
/dev/xvdb contains a ext3 file system labelled 'h01-datas'
	created on Mon Oct  4 11:41:18 2021
Proceed anyway? (y,N)
```

### Disque de sauvegarde

```
sudo mkfs.ext4 /dev/xvdc -L {{ (h|r)## }}-backup
```

Vérifier et taper ```y``` pour valider.

```bash
mke2fs 1.44.5 (15-Dec-2018)
/dev/xvdc contains a ext3 file system labelled 'h01-backup'
	created on Mon Oct  4 12:09:42 2021
Proceed anyway? (y,N)
```

### Contrôle et récupération des uuids

```bash
sudo blkid
```

*Contrôlez les labels, types de disque et UUIDs. Notez ensuite les UUIDs respectifs des disques **xvdb** et **xvdc***.

```bash
/dev/xvdz1: LABEL="swap" UUID="7c558b77-535b-469e-9b34-c9fff9fbde37" TYPE="swap" PARTLABEL="gandiswap" PARTUUID="3c2eceb4-5157-497a-a706-d8d21a2831c8"
/dev/xvda1: LABEL="debian-buster" UUID="b4280e48-4ebd-4b0c-a3e1-d5b040e61364" TYPE="ext4" PARTLABEL="gandiroot" PARTUUID="d6c8e31a-baf3-492e-9cf6-2877b177572a"
/dev/xvdb: LABEL="h01-datas" UUID="9ff511be-1b33-45ac-89e5-c5323752cd3c" TYPE="ext4"
/dev/xvdc: LABEL="h01-backup" UUID="2e3c7fa1-1883-4e79-b6fd-54d5f498132c" TYPE="ext4"
/dev/xvdz2: PARTLABEL="gandiconfig" PARTUUID="3fd30f26-50b7-4e6f-bb77-3c042f302fab"
```

#### En cas de problème de génération d'UUID identique

```bash
sudo apt-get install uuid-runtime
```

```bash
sudo uuidgen
```

```bash
sudo tune2fs -U {{ new_generated_UUID }} /dev/xvdb
```

## Edition des points de montage automatique des volumes

1. Editer le fichier de configuration de montage automatique des disques.

```bash
sudo vi /etc/fstab
```

```diff
LABEL=debian-buster     /   ext4    rw,noatime,errors=remount-ro    0   1
devpts  /dev/pts    devpts  defaults    0   0
none    /proc   proc rw,nosuid,noexec   0   0

+# Disque de données
+UUID={{ xxxxxxxx-xxxxx-xxx-xxx-xxxxxxxxxx }} /srv/{{ (h|r)## }}-datas ext4 rw,noatime,errors=remount-ro 0 2
+
+# Disque de sauvegarde
+UUID={{ xxxxxxxx-xxxxx-xxx-xxx-xxxxxxxxxx }} /srv/{{ (h|r)## }}-backup ext4 rw,noatime,errors=remount-ro 0 2
```

2. Désactiver le montage automatique des disques de la configuration Gandi.

```bash
sudo vi /etc/default/gandi
```

```diff
# set to 0 to avoid hostname automatic reconfigure
CONFIG_HOSTNAME=1

# set to 0 to avoid nameserver automatic reconfigure
CONFIG_NAMESERVER=1

# allow mounting the data disk to the mount point using the disk label
-CONFIG_ALLOW_MOUNT=1
+CONFIG_ALLOW_MOUNT=0
```

## Vérification de prise en compte

1. Relancer le serveur en ligne de commande ou depuis l'interface d'administration Gandi.

```bash
sudo reboot
```

2. Après le redémarrage, se reconnecter en SSH et vérifier les points de montage.

```bash
sudo df
```

*Les disques **xvdb** et **xvdc** devrait être désormais répertoriés*.

```diff
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             2029496       0   2029496   0% /dev
tmpfs             408856    5288    403568   2% /run
/dev/xvda1      20575804 1567696  18121520   8% /
tmpfs            2044264       0   2044264   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            2044264       0   2044264   0% /sys/fs/cgroup
+/dev/xvdb       51343840   53272  48652744   1% /srv/h01-datas
+/dev/xvdc       82045336   57368  77777280   1% /srv/h01-backup
tmpfs                 24       0        24   0% /var/gandi
```

---
[>> Étape suivante : Installation et configuration des essentiels](05-install-config-essentials.md)