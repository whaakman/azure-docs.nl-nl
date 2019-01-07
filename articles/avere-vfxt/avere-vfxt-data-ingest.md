---
title: Gegevens verplaatsen naar Avere vFXT voor Azure
description: Gegevens toevoegen aan een nieuw opslagvolume voor gebruik met de vFXT Avere voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: a3d6cb745c782d2a7166208f2a8dd1202a330b15
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050486"
---
# <a name="moving-data-to-the-vfxt-cluster---parallel-data-ingest"></a>Om gegevens te verplaatsen naar het cluster vFXT - parallelle gegevens opnemen 

Nadat u een nieuw vFXT cluster hebt gemaakt, wordt uw eerste taak mogelijk om gegevens naar de nieuwe opslagvolume te verplaatsen. Echter, als de gebruikelijke manier om gegevens te verplaatsen van één client een eenvoudige kopieeropdracht verleent, waarschijnlijk ziet u een trage kopieerprestaties. Single-threaded kopiëren is niet een goede optie voor het kopiëren van gegevens naar de Avere vFXT van back-end clusteropslag.

Omdat het Avere vFXT-cluster een schaalbare meerdere clientcache is, is de snelste en meest efficiënte manier om gegevens te kopiëren naar het met meerdere clients. Deze techniek parallelizes opname van de bestanden en -objecten.

![Diagram van verplaatsing van gegevens van meerdere client, met meerdere threads: Bovenaan links, heeft een pictogram voor on-premises hardware opslag meerdere pijlen die afkomstig zijn van deze. De pijlen verwijzen naar vier clientcomputers. Vanaf elke clientcomputer verwijzen drie pijlen naar de vFXT Avere. Uit de vFXT Avere verwijzen meerdere pijlen naar Blob-opslag.](media/avere-vfxt-parallel-ingest.png) 

De ``cp`` of ``copy`` opdrachten die vaak worden gebruikt voor het gebruik voor het overbrengen van gegevens uit één opslagsysteem naar een andere zijn single-threaded processen die slechts één bestand tegelijk kopiëren. Dit betekent dat de server slechts één bestand per keer - dit een verspilling van resources van het cluster is is opnemen.

In dit artikel beschrijft strategieën voor het maken van een systeem om gegevens te verplaatsen naar het Avere vFXT cluster kopiëren meerdere client, met meerdere threads van bestanden. Hierin wordt uitgelegd bestand overdracht concepten en certificaattoewijzing die kunnen worden gebruikt voor het efficiënt gegevens kopiëren met behulp van meerdere clients en eenvoudige kopie-opdrachten.

Hierin wordt ook uitgelegd sommige hulpprogramma's die kunnen helpen. De ``msrsync`` hulpprogramma kan worden gebruikt om het proces van een gegevensset in buckets te delen en het gebruik rsync opdrachten gedeeltelijk te automatiseren. De ``parallelcp`` script wordt een ander hulpprogramma dat wordt gelezen van de bronmap en problemen automatisch opdrachten kopiëren.  

Klik op de koppeling naar een sectie gaan:

* [Handmatig kopiëren](#manual-copy-example) : een uitgebreide uitleg met behulp van de kopie-opdrachten
* [Gedeeltelijk geautomatiseerd (msrsync)-voorbeeld](#use-the-msrsync-utility-to-populate-cloud-volumes) 
* [Voorbeeld van de parallelle kopiëren](#use-the-parallel-copy-script)

## <a name="data-ingestor-vm-template"></a>Gegevens ingestor voor het VM-sjabloon

Resource Manager-sjabloon is beschikbaar op GitHub automatisch een virtuele machine maken met de parallelle gegevensopnamehulpprogramma's die in dit artikel worden vermeld. 

![het diagram bevat meerdere pijlen van blob-opslag, hardware, opslag en bestandsbronnen van de Azure. De pijlen verwijzen naar een 'data ingestor vm' en van daaruit meerdere pijlen verwijzen naar de vFXT Avere](media/avere-vfxt-ingestor-vm.png)

De gegevens ingestor VM maakt deel uit van een zelfstudie waarin de zojuist gemaakte virtuele machine koppelt de Avere vFXT cluster en de bootstrap-script downloaden uit het cluster. Lezen [een ingestor gegevens VM Bootstrap](https://github.com/Azure/Avere/blob/master/docs/data_ingestor.md) voor meer informatie.

## <a name="strategic-planning"></a>Strategische planning

Bij het bouwen van een strategie om gegevens parallel te kopiëren, moet u de nadelen in bestandsgrootte, aantal bestanden en directory diepte begrijpen.

* Wanneer bestanden klein zijn, is de metrische gegevens van belang zijn bestanden per seconde.
* Wanneer bestanden zijn groot (10MiBi of hoger), de metriek van belang is bytes per seconde.

Elke kopieerproces heeft een doorvoersnelheid en een prijs overgebrachte bestanden, die kan worden bepaald door de lengte van de opdracht copy timing en waarbij u rekening houdt de bestandsgrootte en het aantal bestanden. Waarin wordt uitgelegd hoe u de meten valt buiten het bereik van dit document, maar het is belangrijk om te begrijpen of u moet zijn om met een kleine of grote bestanden.

## <a name="manual-copy-example"></a>Handmatig kopiëren 

U kunt handmatig een dankzij de multi-threaded kopie maken op een client met meer dan één kopieeropdracht in één keer op de achtergrond op basis van vooraf gedefinieerde sets van bestanden of paden.

De Linux/UNIX ``cp`` opdracht bevat het argument ``-p`` om eigendom en mtime metagegevens te behouden. Dit argument toe te voegen aan de onderstaande opdrachten is optioneel. (Het toevoegen van het argument verhoogt het aantal aanroepen van bestandssysteem naar de doel-bestandssysteem voor het wijzigen van de metagegevens van de client verzonden.)

Dit eenvoudige voorbeeld worden twee bestanden gelijktijdig gekopieerd:

```bash
cp /mnt/source/file1 /mnt/destination1/ & cp /mnt/source/file2 /mnt/destination1/ &
```

Na uitgifte van deze opdracht wordt de `jobs` opdracht ziet u dat er twee threads worden uitgevoerd.

### <a name="predictable-filename-structure"></a>Voorspelbare filename-structuur 

Als uw bestandsnamen zijn, kunt u expressies gebruiken om parallelle kopie threads te maken. 

Bijvoorbeeld, als uw directory bevat 1000-bestanden die zijn opvolgend genummerd van `0001` naar `1000`, kunt u de volgende expressies tien parallelle threads dat elke 100 bestanden kopieert maken:

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination1/ & \
cp /mnt/source/file2* /mnt/destination1/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination1/ & \
cp /mnt/source/file5* /mnt/destination1/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination1/ & \
cp /mnt/source/file8* /mnt/destination1/ & \
cp /mnt/source/file9* /mnt/destination1/
```

### <a name="unknown-filename-structure"></a>Onbekende filename-structuur

Als de structuur van uw benoemen van bestanden niet voorspelbaar is, kunt u bestanden kunt groeperen door de namen van mappen. 

In dit voorbeeld worden verzameld voor hele mappen om te verzenden naar ``cp`` opdrachten worden uitgevoerd als achtergrondtaken:

```bash
/root
|-/dir1
| |-/dir1a
| |-/dir1b
| |-/dir1c
   |-/dir1c1
|-/dir1d
```

Nadat de bestanden worden verzameld, kunt u parallelle kopie uitvoeren opdrachten voor het recursief de submappen en de bijbehorende inhoud kopiëren:

```bash
cp /mnt/source/* /mnt/destination/
mkdir -p /mnt/destination/dir1 && cp /mnt/source/dir1/* mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ & # this command copies dir1c1 via recursion
cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
```

### <a name="when-to-add-mount-points"></a>Bij het toevoegen van koppelpunten

Nadat u onvoldoende parallelle threads gaat op basis van een koppelpunt van het bestandssysteem één bestemming hebt, wordt er een punt waar meer threads toe te voegen geen hogere doorvoer geeft. (Doorvoer wordt gemeten bestanden per seconde of bytes per seconde, afhankelijk van het type gegevens.) Of nog erger, te veel threading soms toe leiden dat een verslechtering van de doorvoer.  

Als dit gebeurt, kunt u aan de clientzijde koppelpunten toevoegen aan andere vFXT cluster IP-adressen met behulp van de dezelfde extern bestandssysteem koppelpad is:

```bash
10.1.0.100:/nfs on /mnt/sourcetype nfs (rw,vers=3,proto=tcp,addr=10.1.0.100)
10.1.1.101:/nfs on /mnt/destination1type nfs (rw,vers=3,proto=tcp,addr=10.1.1.101)
10.1.1.102:/nfs on /mnt/destination2type nfs (rw,vers=3,proto=tcp,addr=10.1.1.102)
10.1.1.103:/nfs on /mnt/destination3type nfs (rw,vers=3,proto=tcp,addr=10.1.1.103)
```

Toe te voegen aan de clientzijde koppelpunt verwijst kunt u uitschakelen extra kopie-opdrachten voor de aanvullende splitsen `/mnt/destination[1-3]` koppelpunten, verder bereiken van parallelle uitvoering.  

Bijvoorbeeld, als de bestanden zeer grote zijn, kunt u definiëren de kopieeropdrachten voor het gebruik van afzonderlijke doelpaden, verstuurt meer opdrachten parallel van de client te kopiëren.

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination2/ & \
cp /mnt/source/file2* /mnt/destination3/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination2/ & \
cp /mnt/source/file5* /mnt/destination3/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination2/ & \
cp /mnt/source/file8* /mnt/destination3/ & \
```

In het bovenstaande voorbeeld worden alle drie bestemming koppelpunten wordt benaderd door de client-processen voor het kopiëren van bestand.

### <a name="when-to-add-clients"></a>Wanneer clients toevoegen

Ten slotte, als u hebt bereikt van de client-mogelijkheden, kopie threads toevoegt of aanvullende koppelpunten niet resulteert in een extra bestanden per seconde of bytes per seconde wordt verhoogd. In dit geval kunt u een andere client met dezelfde set koppelpunten die wordt uitgevoerd een eigen sets bestand kopiëren processen implementeren. 

Voorbeeld:

```bash
Client1: cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
Client1: cp -R /mnt/source/dir2/dir2a /mnt/destination/dir2/ &
Client1: cp -R /mnt/source/dir3/dir3a /mnt/destination/dir3/ &

Client2: cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
Client2: cp -R /mnt/source/dir2/dir2b /mnt/destination/dir2/ &
Client2: cp -R /mnt/source/dir3/dir3b /mnt/destination/dir3/ &

Client3: cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ &
Client3: cp -R /mnt/source/dir2/dir2c /mnt/destination/dir2/ &
Client3: cp -R /mnt/source/dir3/dir3c /mnt/destination/dir3/ &

Client4: cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
Client4: cp -R /mnt/source/dir2/dir2d /mnt/destination/dir2/ &
Client4: cp -R /mnt/source/dir3/dir3d /mnt/destination/dir3/ &
```

### <a name="create-file-manifests"></a>Manifesten van bestand maken

Na wat de benaderingen boven (meerdere kopie-threads per bestemming, meerdere bestemmingen per client, meerdere clients per netwerk toegankelijk is via bron-bestandssysteem), houd rekening met deze aanbeveling: Bestand manifesten bouwen en deze vervolgens gebruiken met kopieeropdrachten over meerdere clients.

In dit scenario maakt gebruik van de UNIX ``find`` opdracht voor het maken van de manifesten van bestanden of mappen:

```bash
user@build:/mnt/source > find . -mindepth 4 -maxdepth 4 -type d
./atj5b55c53be6-01/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-01/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-01/support/trace/rolling
./atj5b55c53be6-03/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-03/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-03/support/trace/rolling
./atj5b55c53be6-02/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-02/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-02/support/trace/rolling
```

Dit resultaat omleiden naar een bestand: `find . -mindepth 4 -maxdepth 4 -type d > /tmp/foo`

U kunt doorloopt vervolgens het manifest, aantal bestanden en het bepalen van de grootte van de submappen met behulp van BASH-opdrachten:

```bash
ben@xlcycl1:/sps/internal/atj5b5ab44b7f > for i in $(cat /tmp/foo); do echo " `find ${i} |wc -l`    `du -sh ${i}`"; done
244    3.5M    ./atj5b5ab44b7f-02/support/gsi/2018-07-18T00:07:03EDT
9      172K    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.8M    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T01:01:00UTC
131    13M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    6.2M    ./atj5b5ab44b7f-02/support/gsi/2018-07-20T21:59:41UTC
134    12M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
7      16K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:12:19UTC
8      83K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:17:17UTC
575    7.7M    ./atj5b5ab44b7f-02/support/cores/armada_main.2000.1531980253.gsi
33     4.4G    ./atj5b5ab44b7f-02/support/trace/rolling
281    6.6M    ./atj5b5ab44b7f-01/support/gsi/2018-07-18T00:07:03EDT
15     182K    ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-18T05:01:00UTC
244    17M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-19T01:01:01UTC
299    31M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T01:01:00UTC
256    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T21:59:41UTC_partial
889    7.7M    ./atj5b5ab44b7f-01/support/gsi/2018-07-20T21:59:41UTC
262    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
11     248K    ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:12:19UTC
11     88K     ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:17:17UTC
645    11M     ./atj5b5ab44b7f-01/support/cores/armada_main.2019.1531980253.gsi
33     4.0G    ./atj5b5ab44b7f-01/support/trace/rolling
244    2.1M    ./atj5b5ab44b7f-03/support/gsi/2018-07-18T00:07:03EDT
9      158K    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.3M    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T01:01:00UTC
131    12M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    8.4M    ./atj5b5ab44b7f-03/support/gsi/2018-07-20T21:59:41UTC
134    14M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T22:25:58UTC_vfxt_catchup
7      159K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:12:19UTC
7      157K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:17:17UTC
576    12M     ./atj5b5ab44b7f-03/support/cores/armada_main.2013.1531980253.gsi
33     2.8G    ./atj5b5ab44b7f-03/support/trace/rolling
```

Ten slotte moet u het werkelijke bestand kopieeropdrachten op de clients stellen.  

Als u vier clients hebt, kunt u deze opdracht gebruiken:

```bash
for i in 1 2 3 4 ; do sed -n ${i}~4p /tmp/foo > /tmp/client${i}; done
```

Als u vijf clients hebt, gebruikt u er ongeveer als volgt:

```bash
for i in 1 2 3 4 5; do sed -n ${i}~5p /tmp/foo > /tmp/client${i}; done
```

En voor zes... Extrapoleren indien nodig.

```bash
for i in 1 2 3 4 5 6; do sed -n ${i}~6p /tmp/foo > /tmp/client${i}; done
```

U krijgt *N* resulterende bestanden, één voor elk van uw *N* clients waarvoor de padnamen naar het niveau van vier mappen verkregen als onderdeel van de uitvoer van de `find` opdracht. 

Elk bestand gebruiken om te maken van de kopieeropdracht:

```bash
for i in 1 2 3 4 5 6; do for j in $(cat /tmp/client${i}); do echo "cp -p -R /mnt/source/${j} /mnt/destination/${j}" >> /tmp/client${i}_copy_commands ; done; done
```

De bovenstaande krijgt u *N* bestanden, elk met een kopieeropdracht per regel, die kan worden uitgevoerd als een BASH-script op de client. 

Het doel is om meerdere threads van deze scripts gelijktijdig per client parallel uit te voeren op meerdere clients.

## <a name="use-the-msrsync-utility-to-populate-cloud-volumes"></a>Gebruik het hulpprogramma msrsync voor het vullen van cloud-volumes

De ``msrsync`` hulpprogramma ook kan worden gebruikt om gegevens te verplaatsen naar een back-end core filer voor het cluster Avere. Dit hulpprogramma is ontworpen om bandbreedtegebruik optimaliseren door het uitvoeren van meerdere parallelle ``rsync`` processen. Het is beschikbaar vanuit GitHub op https://github.com/jbd/msrsync.

``msrsync`` de bronmap in afzonderlijke 'buckets' breekt en voert vervolgens afzonderlijke ``rsync`` processen op elke bucket.

Voorlopige testen met behulp van een 4-core VM hebt u geleerd maximale efficiency bij het gebruik van 64 processen. Gebruik de ``msrsync`` optie ``-p`` in te stellen van het aantal processen op 64.

Houd er rekening mee dat ``msrsync`` kunnen alleen schrijven naar en van lokale volumes. De bron en bestemming moet toegankelijk als lokale koppelingen in het virtuele netwerk van het cluster.

Voor het gebruik van msrsync voor het vullen van een Azure-cloud-volume met een cluster Avere, volgt u deze instructies:

1. Installeer msrsync en de vereisten (rsync en Python 2.6 of hoger)
1. Bepaal het totale aantal bestanden en mappen worden gekopieerd.

   Bijvoorbeeld, gebruik het hulpprogramma Avere ``prime.py`` met argumenten ```prime.py --directory /path/to/some/directory``` (beschikbaar met het downloaden van url https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py).

   Als geen ``prime.py``, kunt u het aantal items met de Gnu berekenen ``find`` hulpprogramma als volgt:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. Het aantal items door 64 om te bepalen het aantal items per proces delen. Gebruik dit nummer met de ``-f`` optie in te stellen van de grootte van de buckets wanneer u de opdracht uitvoert.

1. Probleem met de opdracht msrsync om bestanden te kopiëren:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Met deze opdracht is bijvoorbeeld ontworpen 11.000 bestanden in 64 processen van /test/source-repository naar /mnt/vfxt/repository verplaatsen:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/vfxt/repository``

## <a name="use-the-parallel-copy-script"></a>Gebruik het script parallelle kopiëren

De ``parallelcp`` script ook kan nuttig zijn voor het verplaatsen van gegevens aan het back-end van uw vFXT-cluster. 

Het onderstaande script wordt het uitvoerbare bestand toevoegen `parallelcp`. (Dit script is ontworpen voor Ubuntu; als u een ander distributiepunt gebruikt, moet u ``parallel`` afzonderlijk.)

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM 
#!/bin/bash

display_usage() { 
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n" 
} 

if [  \$# -le 1 ] ; then 
    display_usage
    exit 1
fi 
 
if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then 
    display_usage
    exit 0
fi 

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

### <a name="parallel-copy-example"></a>Voorbeeld van de parallelle kopiëren

In dit voorbeeld wordt het parallelle kopie-script voor het compileren van ``glibc`` met behulp van de bronbestanden op uit het cluster Avere. 
<!-- xxx what is stored where? what is 'the avere cluster mount point'? xxx -->

De bronbestanden worden opgeslagen op het koppelpunt van Avere cluster en de Objectbestanden worden opgeslagen op de lokale vaste schijf.

Met dit script maakt gebruik van script parallelle kopiëren hierboven. De optie ``-j`` wordt gebruikt met ``parallelcp`` en ``make`` parallellisering krijgen.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/node1 avere
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```

