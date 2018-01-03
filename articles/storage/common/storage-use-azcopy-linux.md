---
title: "Gegevens kopiëren of verplaatsen naar Azure Storage met AzCopy op Linux | Microsoft Docs"
description: "De AzCopy op Linux-hulpprogramma gebruiken om te verplaatsen of kopiëren van gegevens of naar blob- en -inhoud. Gegevens van lokale bestanden kopiëren naar Azure Storage of kopiëren van gegevens binnen of tussen opslagaccounts. Uw gegevens eenvoudig migreren naar Azure Storage."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: seguler
ms.openlocfilehash: 2fd89684176cd832b656dae8c8f94a6f1ccbbbe8
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Gegevensoverdracht met AzCopy op Linux

AzCopy is een opdrachtregelprogramma dat is ontworpen voor het kopiëren van gegevens uit Microsoft Azure Blob-, bestands- en tabel opslag, met behulp van eenvoudige opdrachten die zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

Er zijn twee versies van AzCopy die u kunt downloaden. AzCopy op Linux is gebouwd met .NET Core Framework dat gericht is op Linux-platforms biedt POSIX-stijl opdrachtregelopties. [AzCopy op Windows](../storage-use-azcopy.md) is gebouwd met .NET Framework en Windows-stijl biedt opdrachtregelopties. In dit artikel bevat informatie over AzCopy op Linux.

## <a name="download-and-install-azcopy"></a>Downloaden en installeren van AzCopy
### <a name="installation-on-linux"></a>Installatie op Linux

Het artikel bevat opdrachten voor verschillende versies van Ubuntu.  Gebruik de `lsb_release -a` opdracht om uw distributierelease en de codenaam te bevestigen. 

AzCopy op Linux vereist .NET Core framework (versie 2.0) op het platform. Zie de installatie-instructies op de [.NET Core](https://www.microsoft.com/net/download/linux) pagina.

Installeer .NET Core op Ubuntu 16.04 als voorbeeld. Voor de meest recente installatiehandleiding, gaat u naar [.NET Core op Linux](https://www.microsoft.com/net/download/linux) op de installatiepagina.


```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
sudo apt-get install dotnet-sdk-2.0.2
```

Als u .NET Core hebt geïnstalleerd, downloaden en installeren van AzCopy.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Nadat AzCopy op Linux is geïnstalleerd, kunt u de uitgepakte bestanden verwijderen. Als u geen supergebruiker bevoegdheden, kunt u ook ook AzCopy met het shellscript 'azcopy' in de uitgepakte map uitvoeren. 


## <a name="writing-your-first-azcopy-command"></a>Schrijven van uw eerste AzCopy-opdracht
De algemene syntaxis voor opdrachten van AzCopy is:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

De volgende voorbeelden worden de verschillende scenario's voor het kopiëren van gegevens naar en van Microsoft Azure Blobs en -bestanden. Raadpleeg de `azcopy --help` menu voor een gedetailleerde beschrijving van de parameters in elk voorbeeld gebruikt.

## <a name="blob-download"></a>BLOB: downloaden
### <a name="download-single-blob"></a>Enkele blobs downloaden

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key> 
```

Als de map `/mnt/myfiles` niet bestaat, AzCopy wordt deze gemaakt en gedownload `abc.txt ` naar de nieuwe map. 

### <a name="download-single-blob-from-secondary-region"></a>Downloaden van één blob van de secundaire regio

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld nodig hebt.

### <a name="download-all-blobs"></a>Alle blobs downloaden

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Stel dat de volgende BLOB's zich bevinden in de opgegeven container:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Na het opnieuw downloaden, de map `/mnt/myfiles` bevat de volgende bestanden:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Als u geen optie opgeeft `--recursive`, geen blob worden gedownload.

### <a name="download-blobs-with-specified-prefix"></a>Blobs met het opgegeven voorvoegsel downloaden

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Stel dat de volgende BLOB's zich bevinden in de opgegeven container. Alle blobs die beginnen met het voorvoegsel `a` worden gedownload.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Na het opnieuw downloaden, de map `/mnt/myfiles` bevat de volgende bestanden:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

Het voorvoegsel is van toepassing op de virtuele map die het eerste deel van de blob-naam. In het bovenstaande voorbeeld de virtuele map komt niet overeen met het opgegeven voorvoegsel, zodat geen blob wordt gedownload. Bovendien, als de optie `--recursive` niet is opgegeven, AzCopy biedt niet alle blobs downloaden.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd laatste wijziging van de geëxporteerde bestanden moet hetzelfde zijn als de bron-blobs instellen

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

U kunt ook BLOB's uitsluiten van de downloadbewerking op basis van hun tijd voor het laatst is gewijzigd. Bijvoorbeeld, als u wilt uitsluiten waarvan laatst gewijzigd om blobs is dezelfde of nieuwer is dan het doelbestand toevoegen de `--exclude-newer` optie:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Of als u wilt uitsluiten van blobs waarvan laatst gewijzigd om de dezelfde of een ouder is dan het doelbestand toevoegen de `--exclude-older` optie:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>BLOB: uploaden
### <a name="upload-single-file"></a>Bestand uploaden

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --dest-key <key>
```

Als de opgegeven bestemming-container niet bestaat, wordt AzCopy maakt en uploadt het bestand in de App.

### <a name="upload-single-file-to-virtual-directory"></a>Bestand uploaden naar virtuele map

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/vd/abc.txt \
    --dest-key <key>
```

Als de opgegeven virtuele map niet bestaat, AzCopy uploadt het bestand voor het opnemen van de virtuele map in de blob-naam (*bijvoorbeeld*, `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="upload-all-files"></a>Alle bestanden uploaden

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Optie `--recursive` wordt de inhoud van de opgegeven map geüpload naar Blob storage recursief, wat betekent dat alle submappen en de bestanden ook worden geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Nadat de uploadbewerking van bevat de container de volgende bestanden:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Wanneer de optie `--recursive` niet is opgegeven, worden alleen de volgende drie bestanden geüpload:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a>Uploaden van bestanden die overeenkomen met opgegeven patroon

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

Wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Nadat de uploadbewerking van bevat de container de volgende bestanden:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Wanneer de optie `--recursive` niet is opgegeven, AzCopy slaat bestanden in submappen:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geef het MIME-inhoudstype van een bestemmings-blob
AzCopy wordt standaard ingesteld van het type inhoud van een bestemmings-blob naar `application/octet-stream`. U kunt echter expliciet opgeven het inhoudstype via de optie `--set-content-type [content-type]`. Deze syntaxis wordt het type inhoud voor alle blobs in een uploadbewerking.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Als de optie `--set-content-type` is opgegeven zonder een waarde worden AzCopy elke blob of inhoudstype volgens de bestandsextensie van het bestand stelt.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a>BLOB: kopiëren
### <a name="copy-single-blob-within-storage-account"></a>Één blob binnen opslagaccount kopiëren

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key> \
    --dest-key <key>
```

Wanneer u een blob zonder--gesynchroniseerde kopie-optie, kopieert een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-single-blob-across-storage-accounts"></a>Één blob kopiëren tussen opslagaccounts

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Wanneer u een blob zonder--gesynchroniseerde kopie-optie, kopieert een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopiëren van één blob van de secundaire regio naar primaire regio

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1/abc.txt \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld nodig hebt.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Één blob en bijbehorende momentopnamen kopiëren tussen opslagaccounts

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/ \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Na de kopieerbewerking omvat de doelcontainer de blob en bijbehorende momentopnamen. De container bevat de volgende blob en bijbehorende momentopnamen:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchroon kopiëren van BLOB's tussen opslagaccounts
AzCopy standaard worden de gegevens tussen de twee eindpunten voor opslag asynchroon gekopieerd. Daarom wordt de kopie-bewerking wordt uitgevoerd op de achtergrond met behulp van ongebruikte bandbreedtecapaciteit die geen SLA in termen van hoe snel een blob heeft gekopieerd. 

De `--sync-copy` optie zorgt ervoor dat de kopieerbewerking consistente snelheid opgehaald. AzCopy voert de synchrone kopie door te downloaden van de blobs kopiëren vanuit de opgegeven bron naar lokaal geheugen en vervolgens uploaden naar de bestemming van Blob-opslag.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy`Als u meer uitgaande kosten vergeleken met de asynchrone exemplaar kunnen worden gegenereerd. De aanbevolen aanpak is het gebruik van deze optie in een Azure VM, die zich in dezelfde regio bevinden als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaande gegevens.

## <a name="file-download"></a>Bestand: downloaden
### <a name="download-single-file"></a>Enkel bestand downloaden

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Als de opgegeven bron een Azure-bestandsshare is en vervolgens moet u de exacte bestandsnaam opgeven (*bijvoorbeeld* `abc.txt`) moeten worden gedownload van één bestand of de optie `--recursive` om alle bestanden in de share recursief te downloaden. Poging een patroon en de optie opgeven `--recursive` samen resulteert in een fout opgetreden.

### <a name="download-all-files"></a>Alle bestanden downloaden

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Alle lege mappen zijn niet gedownload.

## <a name="file-upload"></a>Bestand: uploaden
### <a name="upload-single-file"></a>Bestand uploaden

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.file.core.windows.net/myfileshare/abc.txt \
    --dest-key <key>
```

### <a name="upload-all-files"></a>Alle bestanden uploaden

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Houd er rekening mee dat alle lege mappen niet worden geüpload.

### <a name="upload-files-matching-specified-pattern"></a>Uploaden van bestanden die overeenkomen met opgegeven patroon

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Bestand: kopiëren
### <a name="copy-across-file-shares"></a>Kopiëren via bestandsshares

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Wanneer u een bestand via bestandsshares kopiëren, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-file-share-to-blob"></a>Kopiëren van de bestandsshare naar de blob

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Wanneer u een bestand vanuit de bestandsshare kopiëren naar de blob, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-blob-to-file-share"></a>Kopiëren van blob naar de bestandsshare

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Wanneer u een bestand van blob naar de bestandsshare kopiëren, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="synchronously-copy-files"></a>Synchroon bestanden kopiëren
U kunt opgeven de `--sync-copy` optie om gegevens te kopiëren van File Storage File Storage, uit de opslag van bestanden naar Blob Storage en naar Blob Storage tot bestandsopslag synchroon. Deze bewerking AzCopy uitgevoerd door de brongegevens downloaden naar het lokale geheugen en vervolgens uploaden naar de bestemming. In dit geval de standaard uitgaande kosten van toepassing.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Bij het kopiëren van de opslag van bestanden naar Blob Storage, de standaard blob-type blok-blob is, optie kunt u opgeven `--blob-type page` blob doeltype wijzigen. Beschikbare typen zijn `page | block | append`.

Houd er rekening mee dat `--sync-copy` extra uitgaande kosten vergelijken met het asynchrone kopie kan genereren. De aanbevolen aanpak is het gebruik van deze optie in een Azure VM, die zich in dezelfde regio bevinden als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaande gegevens.

## <a name="other-azcopy-features"></a>Andere functies van AzCopy
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen gegevens die niet in de doel bestaat-gekopieerd
De `--exclude-older` en `--exclude-newer` parameters kunt u oudere of nieuwere bron resources uitsluiten wordt gekopieerd, respectievelijk. Als u wilt kopiëren van de bron-resources die niet bestaan in de doel-, kunt u beide parameters in de AzCopy-opdracht opgeven:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a>Gebruik een configuratiebestand om op te geven van opdrachtregelparameters

```azcopy
azcopy --config-file "azcopy-config.ini"
```

U kunt eventuele opdrachtregelparameters AzCopy opnemen in een configuratiebestand. AzCopy verwerkt de parameters in het bestand alsof ze had is opgegeven op de opdrachtregel voor het uitvoeren van een directe vervanging met de inhoud van het bestand.

Stel een configuratiebestand met de naam `copyoperation`, die de volgende regels bevat. Elke parameter AzCopy kan worden opgegeven op één regel.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

of op afzonderlijke regels:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy mislukt als u de parameter over twee regels verdelen, zoals hier wordt weergegeven voor de `--source-key` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Geef een shared access signature (SAS)

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-sas <SAS1> \
    --dest-sas <SAS2>
```

U kunt ook een SAS voor de container URI opgeven:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

### <a name="journal-file-folder"></a>Map voor logboek-bestand
Telkens wanneer die u een opdracht met AzCopy geven, controleert deze of een journal-bestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. De wijzigingslogboek-bestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als genereert een nieuw journaalbestand.

Als het journaalbestand bestaat, controleert AzCopy of de opdrachtregel die ingevoerde overeenkomt met de opdrachtregel in het logboek-bestand. Als de twee opdrachtregels overeenkomen, hervat AzCopy de bewerking niet voltooid. Als ze niet overeenkomen, wordt AzCopy gebruiker of het journaalbestand te overschrijven naar een nieuwe bewerking starten of de huidige bewerking te annuleren.

Als u de standaardlocatie voor het wijzigingslogboek-bestand gebruiken wilt:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Als u de optie weglaat `--resume`, of geef de optie `--resume` zonder het mappad, zoals hierboven, AzCopy wijzigingslogboek wordt het bestand gemaakt op de standaardlocatie `~\Microsoft\Azure\AzCopy`. Als het wijzigingslogboek bestand al bestaat, hervat de bewerking op basis van het journaalbestand met AzCopy.

Als u een aangepaste locatie voor het wijzigingslogboek-bestand opgeven wilt:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

In dit voorbeeld wordt het journaalbestand als deze niet al bestaat. Als deze bestaat, hervat de bewerking op basis van het journaalbestand met AzCopy.

Als u een bewerking AzCopy hervatten wilt, herhaalt u dezelfde opdracht. AzCopy op Linux vervolgens wordt om bevestiging gevraagd:

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Uitgebreide uitvoer-Logboeken

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal gelijktijdige bewerkingen starten
Optie `--parallel-level` geeft het aantal gelijktijdige te kopiëren. AzCopy wordt standaard een bepaald aantal gelijktijdige bewerkingen te verhogen van de overdracht gegevensdoorvoer gestart. Het aantal gelijktijdige bewerkingen gelijk is 8 maal het aantal processors dat u hebt. Als u via een netwerk met lage bandbreedte AzCopy uitvoert, kunt u een lager getal voor--parallel-niveau om te voorkomen dat mislukt, omdat resource concurrentie opgeven.

>[!TIP]
>Bekijk de volledige lijst van AzCopy parameters wilt weergeven, 'azcopy--help-menu.

## <a name="known-issues-and-best-practices"></a>Bekende problemen en aanbevolen procedures
### <a name="error-net-sdk-20-is-not-found-in-the-system"></a>Fout: De SDK voor .NET 2.0 is niet gevonden in het systeem.
AzCopy is afhankelijk van de .NET SDK 2.0 vanaf de versie van AzCopy 7.0. AzCopy gebruikt voor deze versie .NET Core 1.1. Als er een foutmelding weergegeven dat .NET Core 2.0 is niet geïnstalleerd in het systeem optreden, moet u mogelijk installeren of upgraden met behulp van de [.NET Core-installatie-instructies](https://www.microsoft.com/net/learn/get-started/linuxredhat).

### <a name="error-installing-azcopy"></a>Fout bij het installeren van AzCopy
Als u problemen met de installatie van AzCopy, u kunt proberen om uit te voeren met de bash-script in de uitgepakte AzCopy `azcopy` map.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Limiet voor gelijktijdige schrijfbewerkingen tijdens het kopiëren van gegevens
Als u blobs of bestanden met AzCopy kopieert, houd er rekening mee dat een andere toepassing de gegevens heeft terwijl u kopieert. Indien mogelijk, zorg dat de gegevens die u wilt kopiëren niet tijdens de kopieerbewerking wordt gewijzigd. Bijvoorbeeld bij het kopiëren van een VHD die is gekoppeld aan een virtuele machine van Azure, zorg dat er geen andere toepassingen momenteel voor de VHD schrijft. Een goede manier om dit te doen is door het leasen van de resource te worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en kopieer de momentopname.

Als u niet voorkomen andere toepassingen dat bij het schrijven naar blobs of bestanden, terwijl ze worden gekopieerd, klikt u vervolgens Houd er rekening mee dat op het moment dat de taak is voltooid, de gekopieerde resources niet meer mogelijk volledige pariteit met de bron-resources.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Één AzCopy-sessie uitvoeren op één computer.
AzCopy is ontworpen om u te maximaliseren van uw machinebron de gegevensoverdracht versnellen, is het raadzaam u slechts één exemplaar van AzCopy uitvoeren op één machine en selecteer de optie `--parallel-level` als u meer gelijktijdige bewerkingen nodig. Typ voor meer informatie `AzCopy --help parallel-level` op de opdrachtregel.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

### <a name="azure-storage-documentation"></a>Documentatie bij Azure Storage:
* [Inleiding tot Azure Storage](../storage-introduction.md)
* [Een opslagaccount maken](../storage-create-storage-account.md)
* [BLOB Storage Explorer beheren](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)
* [De Azure CLI 2.0 gebruiken met Azure Storage](../storage-azure-cli.md)
* [Het Blob storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Blob Storage gebruiken met Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Python](../blobs/storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage-blogberichten:
* [AzCopy aangekondigd op Linux-Preview](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Inleiding tot Azure Storage Data Movement bibliotheek-Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introducing synchrone kopiëren en aangepaste type inhoud](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Aangekondigd algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 met de ondersteuning voor de tabel en de bestandsnaam](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Geoptimaliseerd voor grootschalige kopie scenario 's](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Ondersteuning voor geografisch redundante opslag met leestoegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Gegevensoverdracht met modus voor opnieuw starten en SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Cross-account kopiëren Blob met behulp](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Uploaden/downloaden van bestanden voor Azure Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

