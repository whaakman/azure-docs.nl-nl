---
title: Gegevens kopiëren of verplaatsen naar Azure Storage met AzCopy in Linux | Microsoft Docs
description: De AzCopy voor Linux-hulpprogramma gebruiken om te verplaatsen of kopiëren van gegevens naar of van blob en bestand inhoud. Gegevens kopiëren naar Azure Storage van lokale bestanden en gegevens binnen of tussen opslagaccounts kopiëren. Migreer uw gegevens eenvoudig naar Azure Storage.
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 04/26/2018
ms.author: seguler
ms.component: common
ms.openlocfilehash: 3863c2eeb73997361a6a903f0b382a8c50ad6d4f
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870754"
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Gegevens overdragen met AzCopy voor Linux

AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar/van Microsoft Azure BLOB Storage en File-opslag met behulp van eenvoudige opdrachten die zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

Er zijn twee versies van AzCopy die u kunt downloaden. AzCopy in Linux is bedoeld voor Linux-platforms bieden POSIX-stijl opdrachtregelopties. [AzCopy in Windows](../storage-use-azcopy.md) biedt Windows stijl opdrachtregelopties. In dit artikel bevat informatie over AzCopy in Linux. 

> [!NOTE]  
> Vanaf versie van AzCopy 7.2, zijn de .NET Core-afhankelijkheden verpakt met het AzCopy-pakket. Als u gebruikmaakt van 7,2 versie of hoger, niet meer u .NET Core installeren als een vereiste moet.

## <a name="download-and-install-azcopy"></a>Download en installeer AzCopy

### <a name="installation-on-linux"></a>Installatie op Linux

> [!NOTE]
> Mogelijk moet u .NET Core 2.1-afhankelijkheden die zijn gemarkeerd in deze installeren [.NET Core-vereisten artikel](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x) , afhankelijk van uw distributie. 
>
> Voor distributies RHEL 7, installeert u ICU en libunwind afhankelijkheden: ```yum install -y libunwind icu```

AzCopy in Linux installeren (v7.2 of hoger) is net zo gemakkelijk als een tar-pakket ophalen en het installatiescript wordt uitgevoerd. 

**RHEL-6-distributies**: [downloadkoppeling](https://aka.ms/downloadazcopylinuxrhel6)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinuxrhel6
tar -xf azcopy.tar.gz
sudo ./install.sh
```

**Alle andere Linux-distributies**: [downloadkoppeling](https://aka.ms/downloadazcopylinux64)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinux64
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Als AzCopy in Linux is geïnstalleerd, kunt u de uitgepakte bestanden verwijderen. U kunt ook als u geen supergebruikersbevoegdheden hebt kunt u ook uitvoeren `azcopy` met behulp van de shell-script-azcopy in de uitgepakte map.

### <a name="alternative-installation-on-ubuntu"></a>Alternatieve installatie op Ubuntu

**Ubuntu 14.04**

Apt-bron voor Linux Microsoft product opslagplaats toevoegen en installeert u AzCopy:

```bash
sudo echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod/ trusty main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Apt-bron voor Linux Microsoft product opslagplaats toevoegen en installeert u AzCopy:

```bash
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod/ xenial main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a>Schrijven van uw eerste AzCopy-opdracht
De eenvoudige syntaxis voor de AzCopy-opdrachten is:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

De volgende voorbeelden tonen verschillende scenario's voor het kopiëren van gegevens naar en van Microsoft Azure-Blobs en -bestanden. Raadpleeg de `azcopy --help` menu voor een gedetailleerde uitleg van de parameters die in elk voorbeeld gebruikt.

## <a name="blob-download"></a>BLOB: downloaden
### <a name="download-single-blob"></a>Één blob downloaden

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key> 
```

Als de map `/mnt/myfiles` niet bestaat, AzCopy wordt deze gemaakt en gedownload `abc.txt ` naar de nieuwe map. 

### <a name="download-single-blob-from-secondary-region"></a>Één blob downloaden uit de secundaire regio

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld moet hebben.

### <a name="download-all-blobs"></a>Alle blobs downloaden

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Wordt ervan uitgegaan dat de volgende BLOB's zich bevinden in de opgegeven container:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Nadat het downloaden is uitgevoerd, de map `/mnt/myfiles` bevat de volgende bestanden:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Als u geen optie opgeeft `--recursive`, geen blob worden gedownload.

### <a name="download-blobs-with-specified-prefix"></a>Downloaden van blobs met het opgegeven voorvoegsel

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Wordt ervan uitgegaan dat de volgende BLOB's zich bevinden in de opgegeven container. Alle blobs die beginnen met het voorvoegsel `a` worden gedownload.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Nadat het downloaden is uitgevoerd, de map `/mnt/myfiles` bevat de volgende bestanden:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

Het voorvoegsel is van toepassing op de virtuele map die het eerste deel van de blob-naam. In het voorbeeld hierboven wordt weergegeven, de virtuele map komt niet overeen met het opgegeven voorvoegsel, zodat er geen blob wordt gedownload. Bovendien, als de optie `--recursive` niet is opgegeven, alle bestaande blobs wordt niet gedownload door AzCopy.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd laatste wijziging van de geëxporteerde bestanden moet hetzelfde zijn als de bron-BLOB's instellen

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

U kunt ook blobs uitsluiten van de downloadbewerking op basis van hun tijd laatst gewijzigd. Bijvoorbeeld, als u wilt uitsluiten van blobs waarvan het laatste tijdstip gewijzigd is dezelfde of nieuwer is dan het doelbestand toevoegen de `--exclude-newer` optie:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Of als u wilt uitsluiten van blobs waarvan het laatste tijdstip gewijzigd is dezelfde of ouder zijn dan het doelbestand toevoegen de `--exclude-older` optie:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>BLOB: uploaden
### <a name="upload-single-file"></a>Eén bestand uploaden

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --dest-key <key>
```

Als de opgegeven doelcontainer niet bestaat, wordt deze door AzCopy gemaakt en wordt het bestand erin geüpload.

### <a name="upload-single-file-to-virtual-directory"></a>Eén bestand uploaden naar de virtuele map

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/vd/abc.txt \
    --dest-key <key>
```

Als de opgegeven virtuele map niet bestaat, AzCopy wordt het bestand om op te nemen van de virtuele map in de naam van de blob geüpload (*bijvoorbeeld*, `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="redirect-from-stdin"></a>Omleiden van stdin

```azcopy
gzip myarchive.tar -c | azcopy \
    --destination https://myaccount.blob.core.windows.net/mycontainer/mydir/myarchive.tar.gz \
    --dest-key <key>
```

### <a name="upload-all-files"></a>Alle bestanden uploaden

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Optie op te geven `--recursive` wordt de inhoud van de opgegeven map geüpload naar Blob storage recursief, wat betekent dat alle submappen en de bestanden ook geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Nadat de upload is uitgevoerd bevat de container de volgende bestanden:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Wanneer de optie `--recursive` niet is opgegeven, alleen de volgende drie bestanden worden geüpload:

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

Nadat de upload is uitgevoerd bevat de container de volgende bestanden:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Wanneer de optie `--recursive` niet is opgegeven, AzCopy wordt overgeslagen bestanden in submappen:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geef het MIME-inhoudstype van een bestemmings-blob
AzCopy wordt standaard ingesteld van het type inhoud van een bestemmings-blob naar `application/octet-stream`. Echter, u kunt het type inhoud via de optie expliciet opgeven `--set-content-type [content-type]`. Deze syntaxis wordt het type inhoud voor alle blobs in een uploadbewerking ingesteld.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Als de optie `--set-content-type` is opgegeven zonder een waarde worden AzCopy elke blob of inhoudstype op basis van het de bestandsextensie van het bestand stelt.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

### <a name="customizing-the-mime-content-type-mapping"></a>De toewijzing van MIME-type inhoud aanpassen
AzCopy maakt gebruik van een configuratiebestand met een toewijzing van de bestandsextensie in inhoudstype. U kunt deze toewijzing aanpassen en toevoegen van nieuwe paren indien nodig. De toewijzing bevindt zich in  ```/usr/lib/azcopy/AzCopyConfig.json```

## <a name="blob-copy"></a>BLOB: kopiëren
### <a name="copy-single-blob-within-storage-account"></a>Kopieert één blob in de Storage-account

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key> \
    --dest-key <key>
```

Wanneer u een blob zonder--sync-kopiëren-optie, kopieert een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-single-blob-across-storage-accounts"></a>Één blob kopiëren tussen opslagaccounts

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Wanneer u een blob zonder--sync-kopiëren-optie, kopieert een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Één blob van de secundaire regio kopiëren naar primaire regio

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1/abc.txt \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld moet hebben.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Één blob en de momentopnamen ervan verwijderd tussen opslagaccounts kopiëren

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/ \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Na de kopieerbewerking bevat de doelcontainer de blob en de momentopnamen ervan verwijderd. De container bevat de volgende blob en de momentopnamen ervan verwijderd:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchroon blobs kopiëren tussen opslagaccounts
AzCopy standaard worden gegevens gekopieerd tussen twee opslag eindpunten asynchroon. Daarom wordt de kopie-bewerking wordt uitgevoerd op de achtergrond met behulp van ongebruikte bandbreedtecapaciteit geen SLA in termen van hoe snel een blob heeft gekopieerd. 

De `--sync-copy` optie zorgt ervoor dat de kopieerbewerking consistente snelheid opgehaald. De synchrone kopie uitvoert AzCopy door te downloaden van de blobs te kopiëren uit de opgegeven bron in het lokale geheugen en vervolgens geüpload naar de bestemming van Blob storage.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy` aanvullende uitgaand verkeer kosten in vergelijking met asynchrone kopie genereren. De aanbevolen aanpak is het gebruik van deze optie in een Azure-VM, die zich in dezelfde regio als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaand verkeer.

## <a name="file-download"></a>Bestand: downloaden
### <a name="download-single-file"></a>Eén bestand downloaden

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Als de opgegeven bron een Azure-bestandsshare is, dan moet u de exacte bestandsnaam opgeven (*bijvoorbeeld* `abc.txt`) om te downloaden van een enkel bestand of de optie opgeven `--recursive` om alle bestanden in de share recursief te downloaden. Er wordt geprobeerd om op te geven van een bestandspatroon en de optie `--recursive` samen resulteert in een fout.

### <a name="download-all-files"></a>Alle bestanden downloaden

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Houd er rekening mee dat alle lege mappen niet worden gedownload.

## <a name="file-upload"></a>Bestand: uploaden
### <a name="upload-single-file"></a>Eén bestand uploaden

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
Wanneer u een bestand via bestandsshares kopiëren, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-file-share-to-blob"></a>Kopiëren van de bestandsshare naar de blob

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Wanneer u een bestand van de bestandsshare kopiëren naar de blob, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-blob-to-file-share"></a>Kopiëren van blob naar de bestandsshare

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Wanneer u een bestand van blob naar de bestandsshare kopiëren, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="synchronously-copy-files"></a>Synchroon kopiëren van bestanden
U kunt opgeven de `--sync-copy` optie om gegevens te kopiëren van File Storage met File Storage, File Storage op Blob-opslag en Blob-opslag met File Storage synchroon. AzCopy wordt uitgevoerd met deze bewerking door te downloaden van de brongegevens naar het lokale geheugen, en vervolgens geüpload naar de bestemming. In dit geval standaard uitgaand verkeer kosten van toepassing is.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Bij het kopiëren van File Storage op Blob-opslag, het blobtype is blok-blob, optie kunt u opgeven `--blob-type page` te wijzigen van het blobtype. Beschikbare typen zijn `page | block | append`.

Houd er rekening mee dat `--sync-copy` aanvullende uitgaand verkeer kosten vergelijken met asynchrone kopiëren kunnen genereren. De aanbevolen aanpak is het gebruik van deze optie in een Azure-VM, die zich in dezelfde regio als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaand verkeer.

## <a name="other-azcopy-features"></a>Andere functies van AzCopy
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen kopiëren van gegevens die niet in het doel bestaat.
De `--exclude-older` en `--exclude-newer` parameters kunt u oudere of nieuwere bron resources uitsluiten wordt gekopieerd, respectievelijk. Als u wilt dat alleen bron-resources die niet bestaan in het doel wilt kopiëren, kunt u beide parameters in de AzCopy-opdracht:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a>Gebruik een configuratiebestand om op te geven van opdrachtregelparameters

```azcopy
azcopy --config-file "azcopy-config.ini"
```

U kunt de parameters van AzCopy-opdrachtregelprogramma opnemen in een configuratiebestand. AzCopy verwerkt de parameters in het bestand alsof ze waren is opgegeven op de opdrachtregel voor het uitvoeren van een directe vervangen door de inhoud van het bestand.

Wordt ervan uitgegaan dat een configuratiebestand met de naam `copyoperation`, die de volgende regels bevat. Elke parameter AzCopy kan worden opgegeven op één regel.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

of op afzonderlijke regels:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy mislukt als u de parameter verdeeld over twee regels, zoals hier wordt weergegeven voor de `--source-key` parameter:

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

### <a name="journal-file-folder"></a>Bestandsmap logboek
Telkens wanneer die u een opdracht voor AzCopy gebruikt geven, wordt gecontroleerd of een logboekbestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. Het logboekbestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als een nieuw logboekbestand gegenereerd.

Als het logboekbestand bestaat, AzCopy wordt gecontroleerd of de opdrachtregel die u invoert, overeenkomt met de opdrachtregel in het logboekbestand. Als de twee regels van de opdracht overeenkomen, wordt de niet-voltooide bewerking door AzCopy hervat. Als ze niet overeenkomen, vraagt AzCopy gebruiker ofwel het bestand wilt overschrijven wijzigingslogboek om te starten van een nieuwe bewerking of de huidige bewerking te annuleren.

Als u gebruiken van de standaardlocatie voor het logboekbestand wilt:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Als u de optie weglaat `--resume`, of de optie opgeven `--resume` zonder een pad naar de map, zoals hierboven, AzCopy maakt het logboekbestand op de standaardlocatie bevindt, dat is `~\Microsoft\Azure\AzCopy`. Als het logboekbestand al bestaat, hervat de bewerking op basis van het logboekbestand met AzCopy.

Als u opgeven van een aangepaste locatie voor het logboekbestand wilt:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

In dit voorbeeld wordt het logboekbestand gemaakt als deze niet al bestaat. Als deze bestaat, hervat de bewerking op basis van het logboekbestand met AzCopy.

Als u een AzCopy-bewerking hervatten wilt, herhaalt u de dezelfde opdracht. AzCopy in Linux vervolgens wordt om bevestiging gevraagd:

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Uitgebreide logboeken met uitvoergegevens

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal gelijktijdige bewerkingen om te starten
Optie `--parallel-level` geeft het aantal gelijktijdige kopieerbewerkingen. AzCopy wordt standaard gestart van een bepaald aantal gelijktijdige bewerkingen om de overdracht van gegevensdoorvoer te vergroten. Het aantal gelijktijdige bewerkingen is gelijk acht keer het aantal processors dat u hebt. Als u AzCopy via een netwerk met een lage bandbreedte uitvoert, kunt u een lager getal voor--parallel-niveau om te voorkomen dat is mislukt, concurrentie opgeven.

>[!TIP]
>Bekijk de volledige lijst van AzCopy parameters wilt weergeven, de optie 'azcopy--help-menu.

## <a name="installation-steps-for-azcopy-71-and-earlier-versions"></a>Installatiestappen voor AzCopy 7.1 en eerdere versies

AzCopy in Linux (v7.1 en eerder) is de .NET Core framework vereist. Installatie-instructies zijn beschikbaar op de [.NET Core-installatie](https://www.microsoft.com/net/core#linuxubuntu) pagina.

Bijvoorbeeld: begin met het installeren van .NET Core op Ubuntu 16,10. Voor de meest recente installatiehandleiding, gaat u naar [.NET Core in Linux](https://www.microsoft.com/net/core#linuxubuntu) installatiepagina.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-sdk-2.0.0
```

Als u .NET Core hebt geïnstalleerd, downloaden en installeren van AzCopy.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Als AzCopy in Linux is geïnstalleerd, kunt u de uitgepakte bestanden verwijderen. U kunt ook als u geen supergebruikersbevoegdheden, kunt u ook uitvoeren `azcopy` met behulp van de shell-script-azcopy in de uitgepakte map.

## <a name="known-issues-and-best-practices"></a>Bekende problemen en aanbevolen procedures
### <a name="error-installing-azcopy"></a>Fout bij het installeren van AzCopy
Als u problemen met het AzCopy-installatie, kunt u proberen om uit te voeren met behulp van de bash-script in de uitgepakte AzCopy `azcopy` map.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Gelijktijdige schrijfbewerkingen tijdens het kopiëren van gegevens beperken
Wanneer u blobs of bestanden met AzCopy kopieert, houd er rekening mee dat een andere toepassing kan worden het wijzigen van gegevens terwijl u kopieert. Indien mogelijk, ervoor te zorgen dat de gegevens die u wilt kopiëren niet wordt gewijzigd tijdens de kopieerbewerking. Bijvoorbeeld, bij het kopiëren van een VHD die is gekoppeld aan een virtuele machine van Azure, moet u ervoor dat er geen andere toepassingen ontwikkelt die momenteel op de VHD. Een goede manier om dit te doen is door de overdracht van de resource moet worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en kopieer vervolgens de momentopname.

Als u niet voorkomen andere toepassingen dat bij het schrijven naar blobs of de bestanden terwijl ze worden gekopieerd, klikt u vervolgens Houd er rekening mee dat op het moment dat de taak is voltooid, de gekopieerde resources niet meer mogelijk volledige pariteit met de bron-resources.

### <a name="running-multiple-azcopy-processes"></a>Meerdere AzCopy-processen
U kunt meerdere AzCopy processen uitvoeren op één client bieden dat u verschillende logboek. Met behulp van een map één logboek voor processen die meerdere AzCopy wordt niet ondersteund.

1e proces:
```azcopy
azcopy \
    --source /mnt/myfiles1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles1 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal1"
```

2e proces:
```azcopy
azcopy \
    --source /mnt/myfiles2 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles2 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal2"
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

### <a name="azure-storage-documentation"></a>Documentatie voor Azure Storage:
* [Kennismaking met Azure Storage](../storage-introduction.md)
* [Een opslagaccount maken](../storage-create-storage-account.md)
* [Blobs beheren met Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)
* [De Azure CLI gebruiken met Azure Storage](../storage-azure-cli.md)
* [Het Blob storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Blob Storage gebruiken met Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Python](../blobs/storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage-blogberichten:
* [Aankondiging van AzCopy in Linux-Preview](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Maak kennis met de verplaatsing van Azure Storage-bibliotheek Gegevensvoorbeeld](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introductie van synchrone kopiëren en aangepaste inhoudstype](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Aankondiging van algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 met ondersteuning voor tabel en bestand](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Geoptimaliseerd voor scenario's voor grootschalige kopiëren](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Ondersteuning voor geografisch redundante opslag met leestoegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Gegevens overdragen met modus voor opnieuw starten en SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Kopiëren van de Blob cross-account met behulp](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Uploaden/downloaden van bestanden voor Azure-Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
