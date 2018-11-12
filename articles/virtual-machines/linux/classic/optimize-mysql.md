---
title: Prestaties van MySQL op Linux | Microsoft Docs
description: Informatie over het optimaliseren van MySQL uitgevoerd op een Azure-machine (VM) waarop Linux wordt uitgevoerd.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 0ba85e82824bc257869d9801f342bd6dbb0402d2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247446"
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Prestaties van MySQL op Azure Linux VM 's
Er zijn talloze factoren die invloed hebben op prestaties van MySQL in Azure, zowel in de selectie van de virtuele hardware en softwareconfiguratie. In dit artikel richt zich op de prestaties optimaliseren door opslag-, systeem- en databaseconfiguraties.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) en klassiek. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over optimalisatie van de Linux-VM met het Resource Manager-model [optimaliseren van uw Linux-VM op Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>RAID op een virtuele machine van Azure gebruiken
Opslag is de belangrijkste factoren die van invloed op prestaties van de database in cloudomgevingen. In vergelijking met één schijf, krijgt RAID u een snellere toegang via gelijktijdigheid. Zie voor meer informatie, [Standard RAID-niveaus](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

-I/o-schijfdoorvoer en reactietijd van i/o's in Azure kunnen worden verbeterd via RAID. Onze tests lab laten zien dat-i/o-schijfdoorvoer kan worden verdubbeld en i/o-reactietijd kan worden teruggebracht door halve gemiddeld wanneer het aantal schijven RAID verdubbeld (van twee tot vier, vier en acht, enz.). Zie [bijlage A](#AppendixA) voor meer informatie.  

Naast de schijf-i/o verbetert de MySQL-prestaties als u de RAID-niveau verhoogt.  Zie [bijlage B](#AppendixB) voor meer informatie.  

U kunt ook rekening houden met de chunkgrootte. In het algemeen als u een grotere chunkgrootte hebt, krijgt u lagere overhead, met name voor grote schrijfbewerkingen. Wanneer de chunkgrootte te groot is is, kan het echter extra overhead waarmee wordt voorkomen dat u profiteren van een RAID toevoegen. De huidige grootte van de standaardwaarde is 512 KB, dat is een bewezen oplossing naar optimaal is voor de meest algemene productie-omgevingen. Zie [bijlage C](#AppendixC) voor meer informatie.   

Er gelden limieten voor het aantal schijven, kunt u voor andere virtuele machine-typen toevoegen. Deze limieten zijn beschreven in [grootten voor virtuele machine en cloud-service voor Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx). U moet vier gekoppelde gegevensschijven te volgen in het voorbeeld RAID in dit artikel, maar u kunt het instellen van RAID met minder schijven.  

In dit artikel wordt ervan uitgegaan dat u al een virtuele Linux-machine hebt gemaakt en hebben MYSQL geïnstalleerd en geconfigureerd. Zie voor meer informatie over aan de slag, over het installeren van MySQL op Azure.  

### <a name="set-up-raid-on-azure"></a>Instellen van RAID op Azure
De volgende stappen laten zien hoe RAID op Azure maken met behulp van de Azure-portal. U kunt ook RAID instellen met behulp van Windows PowerShell-scripts.
In dit voorbeeld configureren we RAID 0 met vier schijven.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Een gegevensschijf toevoegen aan uw virtuele machine
In de Azure-portal, gaat u naar het dashboard en selecteer de virtuele machine die u wilt een gegevensschijf toevoegen. In dit voorbeeld is de virtuele machine mysqlnode1.  

<!--![Virtual machines][1]-->

Klik op **schijven** en klik vervolgens op **Attach New**.

![Virtuele machines toevoegen schijf](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Maak een nieuwe schijf van 500 GB. Zorg ervoor dat **Host Cache Preference** is ingesteld op **geen**.  Wanneer u klaar bent, klikt u op **OK**.

![Lege schijf koppelen](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Hiermee wordt een lege schijf van uw virtuele machine toegevoegd. Herhaal deze stap drie keer zodat er vier gegevensschijven voor RAID.  

U kunt de toegevoegde schijven in de virtuele machine bekijken door te kijken naar de kernel berichtenlogboek. Bijvoorbeeld, als u wilt zien dit op Ubuntu, gebruik de volgende opdracht:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>RAID maken met de extra schijven
De volgende stappen wordt beschreven hoe u [software-RAID op Linux configureren](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Als u van het bestandssysteem XFS gebruikmaakt, voer de volgende stappen uit nadat u RAID hebt gemaakt.
>
>

Als u wilt installeren XFS op Debian of Ubuntu Linux pure, gebruik de volgende opdracht:  

    apt-get -y install xfsprogs  

Als u wilt installeren XFS op Fedora of CentOS RHEL, gebruik de volgende opdracht:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Een nieuw opslagpad instellen
Gebruik de volgende opdracht voor het instellen van een nieuwe opslagpad:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>De oorspronkelijke gegevens kopiëren naar het nieuwe opslagpad
Gebruik de volgende opdracht uit om gegevens te kopiëren naar de nieuwe opslagpad:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Machtigingen wijzigen zodat MySQL toegang krijgt tot (lezen en schrijven) de gegevensschijf
Gebruik de volgende opdracht uit om machtigingen te wijzigen:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Het algoritme van schijf-i/o-planning aanpassen
Linux implementeert vier verschillende i/o algoritmen plannen:  

* Nooperation-algoritme (geen bewerking)
* Deadline-algoritme (Deadline)
* Volledig geoorloofd Queuing-algoritme (CFQ)
* Budget periode-algoritme (Anticipatory)  

U kunt verschillende i/o-planners in verschillende scenario's om prestaties te optimaliseren. In een omgeving volledig willekeurige toegang is er een belangrijk verschil tussen de algoritmen CFQ en de Deadline voor de prestaties. U wordt aangeraden dat u de MySQL-database-omgeving op de Deadline voor de stabiliteit instellen. Als er een groot aantal opeenvolgende i/o, mogelijk CFQ schijf-i/o-prestaties verminderen.   

Voor SSD- en andere apparaten kunt Nooperation of Deadline betere prestaties dan de standaard-scheduler bereiken.   

Voorafgaand aan de kernel 2.5 is het standaardalgoritme voor het plannen van i/o Deadline. CFQ beginnen met de kernel 2.6.18, is het standaardalgoritme voor het plannen van i/o geworden.  U kunt deze instelling bij het opstarten van de kernel opgeven of u kunt deze instelling dynamisch wijzigen wanneer het systeem wordt uitgevoerd.  

Het volgende voorbeeld laat zien hoe u om te controleren en de scheduler standaard ingesteld op de NOOP-algoritme in de familie Debian distributie.  

### <a name="view-the-current-io-scheduler"></a>De huidige i/o-planner weergeven
Om weer te geven de scheduler voert u de volgende opdracht uit:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

U ziet de volgende uitvoer, waarmee wordt aangegeven met de huidige scheduler:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>Wijzigen van het huidige apparaat (/ dev/sda) van de planning i/o-algoritme
Voer de volgende opdrachten om te wijzigen van het huidige apparaat:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Deze instelling voor /dev/sda alleen is niet nuttig. Het moet worden ingesteld op alle gegevensschijven waarin de database zich bevindt.  
>
>

U ziet de volgende uitvoer, die aangeeft dat grub.cfg met succes is opnieuw opgebouwd en dat de scheduler standaard is bijgewerkt naar Nooperation:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Voor de distributie-familie van Red Hat moet u de volgende opdracht uit:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Configureer de instellingen van operations-bestand
Een aanbevolen procedure is om uit te schakelen de *atime* functie voor logboekregistratie op het bestandssysteem. Atime is de tijd van laatste toegang bestand. Wanneer een bestand wordt geopend, registreert het bestandssysteem de tijdstempel in het logboek. Deze informatie is echter zelden gebruikt. U kunt deze uitschakelen als u niet nodig hebt, waarmee tijd van totale schijf toegang wordt beperkt.  

Atime als logboekregistratie wilt uitschakelen, moet u het fstab-bestand /etc/ bevinden van bestand system-configuratie wijzigen en toevoegen de **noatime** optie.  

Bijvoorbeeld, bewerk het vim/etc/fstab-bestand, toe te voegen de noatime zoals wordt weergegeven in het volgende voorbeeld:  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Vervolgens koppelen het bestandssysteem met de volgende opdracht:  

    mount -o remount /RAID0

Test het gewijzigde resultaat. Wanneer u de test-bestand wijzigt, wordt de tijd van de toegang niet bijgewerkt. De volgende voorbeelden ziet hoe de code eruitziet voor en na de wijziging.

Voor:        

![Code voor wijziging van toegang tot][5]

Na:

![Code na wijziging van toegang tot][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Het maximum aantal ingangen voor gelijktijdigheid van hoge systeem vergroten
MySQL is een hoge gelijktijdigheid-database. Het aantal gelijktijdige ingangen is 1024 voor Linux, die is niet altijd voldoende. Gebruik de volgende stappen uit om te verhogen van het maximum aantal gelijktijdige verwerkt van het systeem voor de ondersteuning van hoge gelijktijdigheid van MySQL.

### <a name="modify-the-limitsconf-file"></a>Het bestand limits.conf wijzigen
Als u wilt vergroten de maximale toegestane gelijktijdige ingangen, toevoegen de volgende vier regels in het bestand /etc/security/limits.conf. Houd er rekening mee dat 65536 is het maximum aantal dat het systeem kan worden ondersteund.   

    * voorlopig nofile 65536
    * vaste nofile 65536
    * voorlopig nproc 65536
    * vaste nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Bijwerken van het systeem voor de nieuwe limieten
Voer de volgende opdrachten voor het bijwerken van het systeem:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Zorg ervoor dat de limieten bij het opstarten worden bijgewerkt
De volgende opstartopdrachten in het bestand /etc/rc.local plaatsen, zodat u gaat de wijziging bij het opstarten.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>Optimalisatie van de MySQL-database
Voor het configureren van MySQL op Azure, kunt u de dezelfde prestaties afstemmen strategie u op een on-premises machine.  

De belangrijkste regels van de i/o-optimalisatie zijn:   

* Verhoog de cachegrootte.
* I/o-reactietijd verminderen.  

U kunt de my.cnf-bestand, dat het configuratiebestand voor server en client-computers bijwerken voor het optimaliseren van de MySQL-serverinstellingen.  

De volgende configuratie-items zijn de belangrijkste factoren die invloed hebben op de MySQL-prestaties:  

* **innodb_buffer_pool_size**: de buffergroep gebufferde gegevens en de index bevat. Dit is meestal is ingesteld op 70 procent van het fysieke geheugen.
* **innodb_log_file_size**: dit is de grootte van het opnieuw. U opnieuw Logboeken gebruiken om ervoor te zorgen dat schrijfbewerkingen snel, betrouwbaar en hersteld na een crash zijn. Dit is ingesteld op 512 MB, waarmee u voldoende ruimte voor het vastleggen van schrijfbewerkingen.
* **max_connections**: soms toepassingen niet sluiten verbindingen naar behoren. Een grotere waarde krijgt meer tijd voor het recyclen van verbindingen inactief sinds de server. Het maximum aantal verbindingen is 10.000, maar de aanbevolen maximaal 5.000 is.
* **Innodb_file_per_table**: deze instelling Hiermee schakelt u of de mogelijkheid van InnoDB voor het opslaan van tabellen in afzonderlijke bestanden. Schakel de optie om ervoor te zorgen dat de verschillende bewerkingen van de geavanceerde beheer efficiënt kunnen worden toegepast. Vanuit het oogpunt van prestaties kan het versnellen van de overdracht van de ruimte tabel en de restanten management prestaties te optimaliseren. De aanbevolen instelling voor deze optie is ingesteld op ON.</br></br>
De standaardinstelling is ingeschakeld, van MySQL 5.6, dus er is geen actie vereist. Voor eerdere versies is de standaardinstelling uitgeschakeld. De instelling moet worden gewijzigd voordat gegevens zijn geladen, omdat alleen de zojuist gemaakte tabellen worden getroffen.
* **innodb_flush_log_at_trx_commit**: de standaardwaarde is 1, waarbij het bereik ingesteld op 0 ~ 2. De standaardwaarde is de meest geschikte optie voor zelfstandige MySQL DB. De instelling van 2 kan de meeste gegevensintegriteit en is geschikt voor het model in de MySQL-Cluster. De instelling van 0 kan gegevens verloren gaan, die invloed kan zijn op de betrouwbaarheid (in sommige gevallen met betere prestaties) en is geschikt voor ondergeschikte server in de MySQL-Cluster.
* **Innodb_log_buffer_size**: het logboekbuffer kan transacties uitvoeren zonder het logboek leegmaken naar schijf voordat u de transacties doorvoeren. Als er grote binaire objecten of tekstveld, de cache wordt snel verbruikt en i/o regelmatig wordt geactiveerd. Het is beter de buffergrootte verhogen als Innodb_log_waits status variabele niet is 0.
* **query_cache_size**: de beste optie is uitgeschakeld vanaf het begin. Query_cache_size ingesteld op 0 (dit is de standaardinstelling in MySQL 5.6) en andere methoden gebruiken om query's te versnellen.  

Zie [bijlage D](#AppendixD) voor een vergelijking van de prestaties vóór en na de optimalisatie.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Het logboek MySQL langzame query's voor het analyseren van het prestatieknelpunt inschakelen
Het MySQL-logboek voor langzame query's kunt u de trage query's identificeren voor MySQL. Nadat het MySQL-logboek voor langzame query's is ingeschakeld, kunt u MySQL-hulpprogramma's zoals **mysqldumpslow** voor het identificeren van het prestatieknelpunt.  

Standaard, is dit niet ingeschakeld. Inschakelen van het logboek voor langzame query's, kan sommige CPU-resources verbruiken. U wordt aangeraden dat u dit tijdelijk inschakelen voor het oplossen van knelpunten. Om in te schakelen op het logboek voor langzame query's:

1. Het bestand my.cnf wijzigen door de volgende regels toe te voegen aan het einde:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. De MySQL-server opnieuw opstarten.

        service  mysql  restart

3. Controleer of de instelling van kracht duurt met behulp van de **weergeven** opdracht.

![Browserpagina's met trage query log ON][7]   

![Resultaten van de langzame-query-logboek][8]

In dit voorbeeld ziet u dat de functie voor langzame query's is ingeschakeld. Vervolgens kunt u de **mysqldumpslow** hulpprogramma voor het bepalen van knelpunten en Optimaliseer de prestaties, zoals het toevoegen van indexen.

## <a name="appendices"></a>Bijlagen
Hieronder vindt u voorbeeld test prestatiegegevens in een testomgeving gerichte geproduceerd. Ze bieden algemene achtergrond op de trend van de prestaties van gegevens met verschillende benaderingen afstemmen van de prestaties. De resultaten verschillen onder verschillende versies van de omgeving of product.

### <a name="AppendixA"></a>Bijlage A  
**Prestaties (IOPS) van de schijf met verschillende RAID-niveaus**

![Schijf-IOPS met verschillende RAID-niveaus][9]

**Test-opdrachten**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> De werkbelasting van deze test maakt gebruik van 64 threads, probeert te bereiken van de bovengrens van RAID.
>
>

### <a name="AppendixB"></a>Bijlage B  
**Vergelijking van de MySQL-prestaties (doorvoer) met verschillende RAID-niveaus**   
(XFS bestandssysteem)

![Vergelijking van de MySQL-prestaties met verschillende RAID-niveaus][10]  
![Vergelijking van de MySQL-prestaties met verschillende RAID-niveaus][11]

**Test-opdrachten**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Vergelijking van de MySQL-prestaties (OLTP) met verschillende RAID-niveaus**  
![Vergelijking van de MySQL-prestaties (OLTP) met verschillende RAID-niveaus][12]

**Test-opdrachten**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Bijlage C   
**Vergelijking van de prestaties (IOPS) schijven voor de verschillende chunk-grootten**  
(XFS bestandssysteem)

![][13]

**Test-opdrachten**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

De bestandsgrootte die wordt gebruikt voor deze test zijn respectievelijk 30 GB en 1 GB, met RAID 0 (4 schijven) XFS bestandssysteem.

### <a name="AppendixD"></a>Bijlage D  
**Vergelijking van MySQL-prestaties (doorvoer) voor en na optimalisatie**  
(XFS File System)

![Vergelijking van MySQL-prestaties (doorvoer) voor en na optimalisatie][14]

**Test-opdrachten**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**De configuratie-instelling voor standaard- en optimalisatie is als volgt:**

| Parameters | Standaard | Optimalisatie |
| --- | --- | --- |
| **innodb_buffer_pool_size** |Geen |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Voor meer gedetailleerde [optimalisatie configuratieparameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), raadpleegt u de [MySQL officiële instructies](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Testomgeving**  

| Hardware | Details |
| --- | --- |
| CPU |AMD Opteron (TM)-Processor 4171 hij / 4 kernen |
| Geheugen |14 GB |
| Schijf |10 GB/schijf |
| OS |14.04.1 Ubuntu LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

