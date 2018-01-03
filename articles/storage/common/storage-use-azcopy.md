---
title: "Gegevens kopiëren of verplaatsen naar Azure Storage met AzCopy in Windows | Microsoft Docs"
description: "De AzCopy op Windows-hulpprogramma gebruiken om te verplaatsen of kopiëren van gegevens of naar blob, table en de inhoud van bestand. Gegevens van lokale bestanden kopiëren naar Azure Storage of kopiëren van gegevens binnen of tussen opslagaccounts. Uw gegevens eenvoudig migreren naar Azure Storage."
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
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 3d4a7ceabc8cdb97fc8a0f29756d7648d253fe21
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a>Gegevensoverdracht met het AzCopy in Windows
AzCopy is een opdrachtregelprogramma dat is ontworpen voor het kopiëren van gegevens uit Microsoft Azure Blob-, bestands- en tabel opslag, met behulp van eenvoudige opdrachten die zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

Er zijn twee versies van AzCopy die u kunt downloaden. AzCopy in Windows is gebouwd met .NET Framework en Windows-stijl biedt opdrachtregelopties. [AzCopy op Linux](storage-use-azcopy-linux.md) is gebouwd met .NET Core Framework die gericht is op Linux-platforms biedt POSIX-stijl opdrachtregelopties. In dit artikel bevat informatie over AzCopy in Windows.

## <a name="download-and-install-azcopy-on-windows"></a>Download en installeer AzCopy in Windows

Download de [meest recente versie van AzCopy op Windows](http://aka.ms/downloadazcopy).

Na de installatie van AzCopy op Windows met behulp van het installatieprogramma, open een opdrachtvenster en navigeer naar de installatiemap van AzCopy op uw computer - waar de `AzCopy.exe` uitvoerbare bestand zich bevindt. Indien gewenst, kunt u de installatielocatie van AzCopy toevoegen aan het systeempad. AzCopy is standaard geïnstalleerd te `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` of `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Schrijven van uw eerste AzCopy-opdracht

De algemene syntaxis voor opdrachten van AzCopy is:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

De volgende voorbeelden ziet een aantal scenario's voor het kopiëren van gegevens naar en van Microsoft Azure Blobs, bestanden en tabellen. Raadpleeg de [AzCopy Parameters](#azcopy-parameters) sectie voor een gedetailleerde beschrijving van de parameters in elk voorbeeld gebruikt.

## <a name="download-blobs-from-blob-storage"></a>Blobs downloaden van Blob-opslag

Bekijk op verschillende manieren voor het downloaden van blobs met AzCopy.

### <a name="download-a-single-blob"></a>Één blob downloaden

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Houd er rekening mee dat als de map `C:\myfolder` niet bestaat, AzCopy maakt het en downloaden `abc.txt ` naar de nieuwe map.

### <a name="download-a-single-blob-from-the-secondary-region"></a>Één blob downloaden van de secundaire regio

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld voor toegang tot de secundaire regio nodig hebt.

### <a name="download-all-blobs-in-a-container"></a>Alle blobs in een container te downloaden

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Stel dat de volgende BLOB's zich bevinden in de opgegeven container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Na het opnieuw downloaden, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Als u geen optie opgeeft `/S`, geen blobs worden gedownload.

### <a name="download-blobs-with-a-specific-prefix"></a>BLOB's met een specifieke voorvoegsel downloaden

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Stel dat de volgende BLOB's zich bevinden in de opgegeven container. Alle blobs die beginnen met het voorvoegsel `a` worden gedownload:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Na het opnieuw downloaden, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Het voorvoegsel is van toepassing op de virtuele map die het eerste deel van de blob-naam. In het bovenstaande voorbeeld de virtuele map komt niet overeen met het opgegeven voorvoegsel, zodat deze niet is gedownload. Bovendien, als de optie `\S` niet is opgegeven, AzCopy biedt niet alle blobs downloaden.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd laatste wijziging van de geëxporteerde bestanden moet hetzelfde zijn als de bron-blobs instellen

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

U kunt ook BLOB's uitsluiten van de downloadbewerking op basis van hun tijd voor het laatst is gewijzigd. Bijvoorbeeld, als u wilt uitsluiten waarvan laatst gewijzigd om blobs is dezelfde of nieuwer is dan het doelbestand toevoegen de `/XN` optie:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Als u wilt uitsluiten waarvan laatst gewijzigd om blobs is dezelfde of ouder zijn dan het doelbestand toevoegen de `/XO` optie:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="upload-blobs-to-blob-storage"></a>BLOB's uploaden naar Blob storage

Bekijk op verschillende manieren voor het uploaden van BLOB's met behulp van AzCopy.

### <a name="upload-a-single-blob"></a>Één blob uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Als de opgegeven bestemming-container niet bestaat, wordt AzCopy maakt en uploadt het bestand in de App.

### <a name="upload-a-single-blob-to-a-virtual-directory"></a>Één blob uploaden naar een virtuele map

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Als de opgegeven virtuele map niet bestaat, AzCopy uploadt het bestand in de naam van de virtuele map (*bijvoorbeeld*, `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="upload-all-blobs-in-a-folder"></a>Alle blobs in een map uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Optie `/S` wordt de inhoud van de opgegeven map geüpload naar Blob storage recursief, wat betekent dat alle submappen en de bestanden ook worden geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nadat de uploadbewerking van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy biedt recursief niet uploaden. Nadat de uploadbewerking van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-blobs-matching-a-specific-pattern"></a>Blobs die overeenkomt met een specifiek patroon uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nadat de uploadbewerking van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy uploadt alleen blobs die zich niet in een virtuele map bevinden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geef het MIME-inhoudstype van een bestemmings-blob

AzCopy wordt standaard ingesteld van het type inhoud van een bestemmings-blob naar `application/octet-stream`. Vanaf versie 3.1.0, kunt u expliciet opgeven het inhoudstype via de optie `/SetContentType:[content-type]`. Deze syntaxis wordt het type inhoud voor alle blobs in een uploadbewerking.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Als u opgeeft `/SetContentType` zonder een waarde ingesteld AzCopy elke blob of inhoudstype volgens de bestandsextensie van het bestand.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="copy-blobs-in-blob-storage"></a>Blobs in Blob storage kopiëren

Bekijk blobs vanaf één locatie naar de andere kopiëren op verschillende manieren met behulp van AzCopy.

### <a name="copy-a-single-blob-from-one-container-to-another-within-the-same-storage-account"></a>Één blob kopiëren uit een container binnen hetzelfde opslagaccount 

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Wanneer u een blob binnen een opslagaccount kopieert een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-single-blob-from-one-storage-account-to-another"></a>Één blob uit één opslagaccount naar de andere kopiëren

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Wanneer u een blob tussen opslagaccounts kopieert, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-single-blob-from-the-secondary-region-to-the-primary-region"></a>Één blob kopiëren van de secundaire regio naar de primaire regio

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld voor toegang tot secundaire opslag nodig hebt.

### <a name="copy-a-single-blob-and-its-snapshots-from-one-storage-account-to-another"></a>Één blob en de momentopnamen van een opslagaccount naar de andere kopiëren

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Na de kopieerbewerking omvat de doelcontainer de blob en bijbehorende momentopnamen. Ervan uitgaande dat de blob in het bovenstaande voorbeeld heeft twee momentopnamen, bevat de container de volgende blob en momentopnamen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="copy-all-blobs-in-a-container-to-another-storage-account"></a>Alle blobs in een container kopiëren naar een ander opslagaccount 

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 
/Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /S
```

Optie uploadt /S u de inhoud van de opgegeven container recursief. Zie [uploaden alle blobs in een map](#upload-all-blobs-in-a-folder) voor meer informatie en een voorbeeld.

### <a name="synchronously-copy-blobs-from-one-storage-account-to-another"></a>Synchroon blobs uit één opslagaccount naar de andere kopiëren

AzCopy standaard worden de gegevens tussen de twee eindpunten voor opslag asynchroon gekopieerd. Daarom de kopieerbewerking wordt uitgevoerd in de achtergrond met behulp van ongebruikte bandbreedtecapaciteit die geen SLA in termen van hoe u snel heeft een blob wordt gekopieerd en AzCopy controleert periodiek de status van het exemplaar totdat het kopiëren is voltooid of mislukt.

De `/SyncCopy` optie zorgt ervoor dat de kopieerbewerking consistente snelheid opgehaald. AzCopy voert de synchrone kopie door te downloaden van de blobs kopiëren vanuit de opgegeven bron naar lokaal geheugen en vervolgens uploaden naar de bestemming van Blob-opslag.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`kan extra uitgaande kosten vergeleken met de asynchrone kopiëren, de aanbevolen aanpak is deze optie wilt gebruiken in een Azure-virtuele machine die zich in dezelfde regio bevinden als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaande gegevens.

## <a name="download-files-from-file-storage"></a>Downloaden van bestanden van File storage

Bekijk op verschillende manieren voor het downloaden van bestanden met behulp van AzCopy.

### <a name="download-a-single-file"></a>Een enkel bestand downloaden

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Als de opgegeven bron een Azure-bestandsshare is en vervolgens moet u de exacte bestandsnaam opgeven (*bijvoorbeeld* `abc.txt`) moeten worden gedownload van één bestand of de optie `/S` om alle bestanden in de share recursief te downloaden. Poging een patroon en de optie opgeven `/S` samen resulteert in een fout opgetreden.

### <a name="download-all-files-in-a-directory"></a>Alle bestanden in een map downloaden

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Lege mappen zijn niet gedownload.

## <a name="upload-files-to-an-azure-file-share"></a>Bestanden uploaden naar een Azure-bestandsshare

Bekijk op verschillende manieren voor het uploaden van bestanden met behulp van AzCopy.

### <a name="upload-a-single-file"></a>Één bestand uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files-in-a-folder"></a>Alle bestanden in een map uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Houd er rekening mee dat de lege mappen niet worden geüpload.

### <a name="upload-files-matching-a-specific-pattern"></a>Uploaden van bestanden die overeenkomen met een specifiek patroon

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="copy-files-in-file-storage"></a>Kopieer de bestanden in de opslag van bestanden

Bekijk op verschillende manieren om bestanden te kopiëren in een Azure-bestandsshare met behulp van AzCopy.

### <a name="copy-from-one-file-share-to-another"></a>Vanuit één bestandsshare naar de andere kopiëren

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand via bestandsshares kopiëren, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-an-azure-file-share-to-blob-storage"></a>Kopiëren van een Azure-bestandsshare naar Blob-opslag

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand vanuit de bestandsshare kopiëren naar de blob, een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-blob-from-blob-storage-to-an-azure-file-share"></a>Een blob van Blob-opslag te kopiëren naar een Azure-bestandsshare

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand van een blob naar een bestandsshare kopiëren een [serverversie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="synchronously-copy-files"></a>Synchroon bestanden kopiëren

U kunt opgeven de `/SyncCopy` optie om gegevens te kopiëren van File Storage tot bestandsopslag, uit de opslag van bestanden naar Blob Storage en Blob Storage tot bestandsopslag synchroon, AzCopy doet dit door het downloaden van de brongegevens naar lokaal geheugen en opnieuw uploaden naar de bestemming. Standaard uitgaande kosten worden toegepast.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Bij het kopiëren van de opslag van bestanden naar Blob storage, is de standaard blob-type blok-blob; kan de gebruiker de optie opgeven `/BlobType:page` blob doeltype wijzigen.

Houd er rekening mee dat `/SyncCopy` kosten voor extra uitgaand vergeleken met de asynchrone kopie kan genereren. De aanbevolen aanpak is het gebruik van deze optie in de Azure-virtuele machine die zich in dezelfde regio bevinden als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaande gegevens.

## <a name="export-data-from-table-storage"></a>Gegevens exporteren uit de tabelopslag

Laten we bij het exporteren van gegevens uit Azure Table storage met AzCopy.

### <a name="export-a-table"></a>Een tabel exporteren

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

Een manifestbestand schrijft AzCopy naar de opgegeven doelmap. Het manifestbestand is gebruikt in het importproces voor de gegevens die nodig zijn bestanden vinden en voer gegevensvalidatie. Het manifestbestand maakt standaard gebruik van de volgende naamconventie:

    <account name>_<table name>_<timestamp>.manifest

Gebruiker kan ook de optie opgeven `/Manifest:<manifest file name>` kunt u de naam van het manifestbestand instellen.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-an-export-from-table-storage-into-multiple-files"></a>Exporteren van een uit de opslag van de tabel splitsen in meerdere bestanden

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy gebruikt een *volume index* in de bestandsnamen van de gesplitste gegevens te onderscheiden van meerdere bestanden. De index van het volume bestaat uit twee delen: een *partitie sleutel bereikindex* en een *gesplitste bestandsindex*. Beide indexen zijn gebaseerd op nul.

Het partitioneren van index sleutel bereik is 0 als de gebruiker geen optie geeft `/PKRS`.

Stel bijvoorbeeld twee gegevensbestanden van AzCopy wordt gegenereerd nadat de gebruiker Hiermee kunt u de optie `/SplitSize`. De resulterende bestandsnamen van gegevens is mogelijk:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Let op: de minimale mogelijke waarde voor de optie `/SplitSize` is 32 MB. Als de opgegeven bestemming is Blob-opslag, AzCopy het gegevensbestand splitst zodra de grootte bereikt de blob-groottebeperkingen (200GB), ongeacht of optie `/SplitSize` is opgegeven door de gebruiker.

### <a name="export-a-table-to-json-or-csv-data-file-format"></a>Een tabel exporteren naar JSON- of CSV-bestandsindeling voor gegevens

Standaard wordt met AzCopy tabellen exporteert naar JSON-gegevensbestanden. U kunt opgeven dat de optie `/PayloadFormat:JSON|CSV` te exporteren van de tabellen als JSON- of CSV.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Bij het opgeven van de indeling van de CSV-nettolading AzCopy ook genereert u een schemabestand met extensie `.schema.csv` voor elk gegevensbestand.

### <a name="export-table-entities-concurrently"></a>Tabelentiteiten gelijktijdig exporteren

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy begint gelijktijdige bewerkingen om te exporteren entiteiten wanneer de gebruiker Hiermee kunt u de optie `/PKRS`. Elke bewerking exporteert een partitiesleutelbereik.

Houd er rekening mee dat het aantal gelijktijdige bewerkingen ook wordt beheerd door de optie `/NC`. AzCopy gebruikt het aantal coreprocessors als de standaardwaarde van `/NC` bij het kopiëren van de tabelentiteiten, zelfs als `/NC` is niet opgegeven. Wanneer de gebruiker opgeeft optie `/PKRS`, AzCopy gebruikt de laagste waarde van de twee waarden - partitie sleutelbereiken versus impliciet of expliciet opgegeven gelijktijdige bewerkingen - te bepalen hoeveel gelijktijdige bewerkingen om te starten. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

### <a name="export-a-table-to-blob-storage"></a>Een tabel exporteren naar Blob storage

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy genereert een JSON-gegevensbestand in de blob-container met de volgende naamconventie:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Het gegenereerde bestand van de JSON-gegevens volgt de indeling van nettolading voor minimale metagegevens. Zie voor meer informatie over deze indeling van nettolading [indeling van nettolading voor tabel servicebewerkingen](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Houd er rekening mee dat bij het tabellen exporteren naar blobs, AzCopy downloadt de tabelentiteiten naar lokale tijdelijke gegevensbestanden en uploadt u deze entiteiten naar de blob. Deze tijdelijke gegevensbestanden in de logboekmap-bestand met het standaardpad zijn geplaatst '<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>', kunt u optie/Z: [journaal-map] te wijzigen van het journaal maplocatie bestands- en dus de locatie van tijdelijke bestanden te wijzigen. De grootte van de tijdelijke bestanden wordt bepaald door uw tabelentiteiten grootte en de grootte die u met de optie /SplitSize hebt opgegeven, hoewel het tijdelijke bestand in de lokale schijf wordt onmiddellijk verwijderd zodra deze is geüpload naar de blob, zorg ervoor dat u hebt onvoldoende lokale schijfruimte voor het opslaan van deze tijdelijke gegevensbestanden voordat ze worden verwijderd.

## <a name="import-data-into-table-storage"></a>Gegevens importeren in de Table storage

Laten we bij het importeren van gegevens in Azure Table storage met AzCopy.

### <a name="import-a-table"></a>Een tabel importeren

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

De optie `/EntityOperation` wordt aangegeven hoe entiteiten in de tabel invoegen. Mogelijke waarden zijn:

* `InsertOrSkip`: Hiermee slaat een bestaande entiteit of een nieuwe entiteit ingevoegd als het bestand niet in de tabel bestaat.
* `InsertOrMerge`: Een bestaande entiteit samenvoegingen of een nieuwe entiteit ingevoegd als het bestand niet in de tabel bestaat.
* `InsertOrReplace`: Een bestaande entiteit vervangt of een nieuwe entiteit ingevoegd als het bestand niet in de tabel bestaat.

Opmerking dat kunt u de optie opgeven `/PKRS` in het scenario voor importeren. In tegenstelling tot de export-scenario, waarin u de optie moet opgeven `/PKRS` voor het starten van gelijktijdige bewerkingen AzCopy gelijktijdige bewerkingen wordt standaard gestart bij het importeren van een tabel. Het aantal gelijktijdige bewerkingen gestart is gelijk aan het aantal coreprocessors; u kunt echter een verschillend aantal gelijktijdige met optie opgeven `/NC`. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

Houd er rekening mee dat AzCopy biedt alleen ondersteuning voor JSON, niet-CSV importeren. AzCopy niet ondersteunt tabel invoer van gebruiker gemaakte JSON en manifest bestanden. Beide bestanden moeten afkomstig zijn van het exporteren van een AzCopy-tabel. Om fouten te voorkomen, neem geen wijzigen van de geëxporteerde JSON of manifestbestand.

### <a name="import-entities-into-a-table-from-blob-storage"></a>Importeren van entiteiten in een tabel van Blob-opslag

Stel een Blob-container bevat het volgende: A JSON-bestand voor een Azure-tabel en het bijbehorende manifest-bestand.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

U kunt de volgende opdracht voor het importeren van entiteiten in een tabel met behulp van het manifestbestand in de blobcontainer uitvoeren:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Andere functies van AzCopy

Laten we op sommige andere functies van AzCopy.

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen gegevens die niet in de doel bestaat-gekopieerd

De `/XO` en `/XN` parameters kunt u oudere of nieuwere bron resources uitsluiten wordt gekopieerd, respectievelijk. Als u wilt kopiëren van de bron-resources die niet bestaan in de doel-, kunt u beide parameters in de AzCopy-opdracht opgeven:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Houd er rekening mee dat dit wordt niet ondersteund wanneer de bron- of doelserver een tabel wordt.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Gebruik een responsbestand opdrachtregelparameters opgeven

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

U kunt eventuele opdrachtregelparameters AzCopy opnemen in een responsbestand. AzCopy verwerkt de parameters in het bestand alsof ze had is opgegeven op de opdrachtregel voor het uitvoeren van een directe vervanging met de inhoud van het bestand.

Wordt ervan uitgegaan dat een antwoordbestand met de naam `copyoperation.txt`, die de volgende regels bevat. Elke parameter AzCopy kan op een enkele regel worden opgegeven

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

of op afzonderlijke regels:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy mislukt als u de parameter over twee regels verdelen, zoals hier wordt weergegeven voor de `/sourcekey` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Meerdere antwoordbestanden gebruiken om op te geven van opdrachtregelparameters

Wordt ervan uitgegaan dat een antwoordbestand met de naam `source.txt` die aangeeft dat een Broncontainer:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

En een antwoordbestand met de naam `dest.txt` waarmee een doelmap in het bestandssysteem:

    /Dest:C:\myfolder

En een antwoordbestand met de naam `options.txt` Hiermee worden de opties voor AzCopy:

    /S /Y

AzCopy aanroepen met deze antwoordbestanden, die zich bevinden in een map `C:\responsefiles`, gebruikt u deze opdracht:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy verwerkt met deze opdracht net zoals als u alle van de afzonderlijke parameters op de opdrachtregel:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Geef een shared access signature (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

U kunt ook een SAS voor de container URI opgeven:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Map voor logboek-bestand

Telkens wanneer die u een opdracht met AzCopy geven, controleert deze of een journal-bestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. De wijzigingslogboek-bestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als genereert een nieuw journaalbestand.

Als het journaalbestand bestaat, controleert AzCopy of de opdrachtregel die ingevoerde overeenkomt met de opdrachtregel in het logboek-bestand. Als de twee opdrachtregels overeenkomen, hervat AzCopy de bewerking niet voltooid. Als ze niet overeenkomen, wordt u gevraagd of het journaalbestand te overschrijven naar een nieuwe bewerking starten of de huidige bewerking te annuleren.

Als u de standaardlocatie voor het wijzigingslogboek-bestand gebruiken wilt:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Als u de optie weglaat `/Z`, of geef de optie `/Z` zonder het mappad, zoals hierboven, AzCopy wijzigingslogboek wordt het bestand gemaakt op de standaardlocatie `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Als het wijzigingslogboek bestand al bestaat, hervat de bewerking op basis van het journaalbestand met AzCopy.

Als u een aangepaste locatie voor het wijzigingslogboek-bestand opgeven wilt:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

In dit voorbeeld wordt het journaalbestand als deze niet al bestaat. Als deze bestaat, hervat de bewerking op basis van het journaalbestand met AzCopy.

Als u een bewerking AzCopy hervatten wilt:

```azcopy
AzCopy /Z:C:\journalfolder\
```

In dit voorbeeld hervat de laatste bewerking, die mogelijk niet voltooien.

### <a name="generate-a-log-file"></a>Genereren van een logboekbestand

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Als u de optie opgeven `/V` zonder op te geven van een pad naar het uitgebreide logboek, AzCopy maakt vervolgens het logboekbestand op de standaardlocatie `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Anders kunt u een logboekbestand maken in een aangepaste locatie:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Houd er rekening mee dat als u een relatief pad na optie opgeeft `/V`, zoals `/V:test/azcopy1.log`, wordt het uitgebreide logboek in de huidige werkmap in een submap met de naam gemaakt `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal gelijktijdige bewerkingen starten

Optie `/NC` geeft het aantal gelijktijdige te kopiëren. AzCopy wordt standaard een bepaald aantal gelijktijdige bewerkingen te verhogen van de overdracht gegevensdoorvoer gestart. Voor bewerkingen van de tabel is het aantal gelijktijdige bewerkingen gelijk aan het aantal processors dat u hebt. Voor Blob- en bewerkingen, het aantal gelijktijdige bewerkingen is gelijk aan 8 keer het aantal processors dat u hebt. Als u via een netwerk met lage bandbreedte AzCopy uitvoert, kunt u een lager getal voor /NC om te voorkomen dat mislukt, omdat resource concurrentie opgeven.

### <a name="run-azcopy-against-the-azure-storage-emulator"></a>AzCopy uitvoeren op de Azure-Opslagemulator

U kunt uitvoeren AzCopy tegen de [Azure-Opslagemulator](storage-use-emulator.md) voor blobs:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

U kunt het ook voor tabellen uitvoeren:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>AzCopy-Parameters

Parameters voor AzCopy worden hieronder beschreven. U kunt ook een van de volgende opdrachten uit vanaf de opdrachtregel voor hulp bij het gebruik van AzCopy typen:

* Voor gedetailleerde help voor AzCopy:`AzCopy /?`
* Voor gedetailleerde hulp bij elke parameter AzCopy:`AzCopy /?:SourceKey`
* Voor voorbeelden van opdrachtregels:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Source: 'bron'

Hiermee geeft u de brongegevens waaruit u wilt kopiëren. De bron kan een bestandssysteemmap, een blob-container, een blob virtuele map, een bestandsshare voor opslag, een map voor opslag of een Azure-tabel zijn.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="destdestination"></a>/ Dest: "doelmap"

Hiermee geeft u het doel om naar te kopiëren. Het doel mag een bestandssysteemmap, een blob-container, een blob virtuele map, een bestandsshare voor opslag, een map voor opslag of een Azure-tabel.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="patternfile-pattern"></a>/ Patroon: "bestandspatroon"

Hiermee geeft u een patroon waarmee wordt aangegeven welke bestanden worden gekopieerd. Het gedrag van de parameter /Pattern wordt bepaald door de locatie van de brongegevens en de aanwezigheid van de optie recursieve modus. Recursieve modus wordt opgegeven via de optie/s.

De opgegeven bron is een map in het bestandssysteem, waarna standaard jokertekens zijn als het opgegeven bestandspatroon wordt vergeleken met de bestanden in de map. Als de optie/s is opgegeven, klikt u vervolgens AzCopy, overeenkomt met het opgegeven patroon tegen alle bestanden in alle submappen onder de map.

Als de opgegeven bron een blob-container of de virtuele map is, worden klikt u vervolgens jokertekens niet toegepast. Als het opgegeven bestandspatroon optie die /s is opgegeven, klikt u vervolgens AzCopy geïnterpreteerd als een voorvoegsel van de blob. Als de optie/s niet is opgegeven, klikt u vervolgens AzCopy komt overeen met het bestandspatroon tegen exacte blob-namen.

Als de opgegeven bron een Azure-bestandsshare is, wordt u moet de exacte bestandsnaam (bijvoorbeeld abc.txt) als u wilt kopiëren van één bestand opgeven, of de optie/s alle bestanden in de share recursief te kopiëren. Probeert op te geven van beide een bestand patroon en de optie/s samen met resultaten in een fout.

AzCopy gebruikt hoofdlettergevoelig overeenkomende wanneer de/Source een blob-container of blob virtuele map is en maakt gebruik van niet-hoofdlettergevoelige overeenkomende in alle andere gevallen.

Bestand standaardpatroon gebruikt wanneer er geen bestandspatroon is opgegeven is *.* voor een locatie in het bestandssysteem of een leeg voorvoegsel voor een Azure-opslaglocatie. Meerdere bestand patronen opgeven wordt niet ondersteund.

**Van toepassing op:** Blobs, bestanden

### <a name="destkeystorage-key"></a>/ DestKey: 'opslagsleutel'

Hiermee geeft u de toegangssleutel voor de doelbron.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="destsassas-token"></a>/ DestSAS: 'sas-token'

Hiermee geeft u een Shared Access Signature (SAS) met machtigingen voor lezen en schrijven voor de bestemming (indien van toepassing). De SAS met dubbele aanhalingstekens rond omdat deze mogelijk speciale opdrachtregelprogramma tekens bevat.

Als de doel-resource een blob-container, een bestandsshare of een tabel is, kunt u deze optie gevolgd door de SAS-token opgeven of u kunt de SAS opgeven als onderdeel van de doel-blob-container, bestandsshare of de URI van de tabel, zonder deze optie.

Als de bron- en doelserver beide blobs zijn, moet de bestemmings-blob zich binnen hetzelfde opslagaccount als de bron-blob.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: 'opslagsleutel'

Hiermee geeft u de opslagaccountsleutel voor de bronresource.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcesassas-token"></a>/ SourceSAS: 'sas-token'

Hiermee geeft u een Shared Access Signature met lees- en machtigingen voor de bron (indien van toepassing). De SAS met dubbele aanhalingstekens rond omdat deze mogelijk speciale opdrachtregelprogramma tekens bevat.

Als de bron-bron een blob-container is en een sleutel noch een SAS is opgegeven, wordt de blob-container gelezen via anonieme toegang.

Als de bron een bestandsshare is of een sleutel of een SAS tabel moet worden opgegeven.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="s"></a>/ S

Hiermee geeft u een recursieve modus voor het kopiëren van. In de modus voor recursieve kopieert AzCopy alle blobs of bestanden die overeenkomen met het opgegeven bestandspatroon, inclusief verbindingen in submappen.

**Van toepassing op:** Blobs, bestanden

### <a name="blobtypeblock--page--append"></a>/ BlobType: 'block' | "page" | "toevoegen"

Hiermee geeft u op of de bestemmings-blob een blok-blob, een pagina-blob of een toevoeg-blob is. Deze optie is alleen toepasbaar wanneer u een blob uploadt. Anders wordt er een fout gegenereerd. Als de bestemming een blob is en deze optie niet is opgegeven, standaard maakt AzCopy een blok-blob.

**Van toepassing op:** Blobs

### <a name="checkmd5"></a>/ CheckMD5

Een MD5-hash voor de gedownloade gegevens berekend en controleert of de MD5-hash wordt opgeslagen in de blob of Content-MD5-eigenschap van bestand overeenkomt met de berekende hash. De MD5-controle is standaard uitgeschakeld, zodat u deze optie om de MD5-controle uitvoeren bij het downloaden van gegevens moet opgeven.

Houd er rekening mee dat Azure Storage garandeert niet dat de MD5-hash die is opgeslagen voor de blob of het bestand bijgewerkt is. Het is de verantwoordelijkheid van de client de MD5 bijwerken wanneer de blob of het bestand is gewijzigd.

AzCopy stelt altijd de Content-MD5-eigenschap voor een Azure-blob of het bestand nadat het uploaden naar de service.  

**Van toepassing op:** Blobs, bestanden

### <a name="snapshot"></a>/ Momentopname

Geeft aan of om over te dragen van momentopnamen. Deze optie is alleen geldig wanneer de bron een blob is.

De overgebrachte blob-momentopnamen worden gewijzigd in deze indeling: .extensie blob-naam (momentopname-tijd)

Momentopnamen worden standaard niet gekopieerd.

**Van toepassing op:** Blobs

### <a name="vverbose-log-file"></a>/ V: [uitgebreide-log-bestand]

Uitvoer uitgebreide statusberichten in een logboekbestand.

Het logboekbestand heet standaard AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Als u een bestaande bestandslocatie voor deze optie opgeeft, wordt het uitgebreide logboek wordt toegevoegd aan of het bestand.  

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="zjournal-file-folder"></a>/ Z: [journaal-map]

Hiermee geeft u een map journaal-bestand voor het hervatten van een bewerking.

AzCopy ondersteunt altijd wordt hervat als een bewerking is onderbroken.

Als deze optie niet opgegeven is of deze is opgegeven zonder het pad naar een map, maakt AzCopy de journaalbestand in de standaardlocatie bevindt, % LocalAppData%\Microsoft\Azure\AzCopy is.

Telkens wanneer die u een opdracht met AzCopy geven, controleert deze of een journal-bestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. De wijzigingslogboek-bestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als genereert een nieuw journaalbestand.

Als het journaalbestand bestaat, controleert AzCopy of de opdrachtregel die ingevoerde overeenkomt met de opdrachtregel in het logboek-bestand. Als de twee opdrachtregels overeenkomen, hervat AzCopy de bewerking niet voltooid. Als ze niet overeenkomen, wordt u gevraagd of het journaalbestand te overschrijven naar een nieuwe bewerking starten of de huidige bewerking te annuleren.

De wijzigingslogboek-bestand wordt verwijderd bij de bewerking is voltooid.

Houd er rekening mee dat het hervatten van een bewerking van een journal-bestand gemaakt met een eerdere versie van AzCopy niet wordt ondersteund.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="parameter-file"></a>/@:"parameter-File'

Hiermee geeft u een bestand met parameters. AzCopy verwerkt de parameters in het bestand alsof ze op de opdrachtregel is doorgegeven.

In een responsbestand kunt u meerdere parameters opgeven op één regel of geef elke parameter in op een aparte regel. Houd er rekening mee dat een afzonderlijke parameter kan niet worden gebruikt voor het bereik van meerdere regels.

Antwoordbestanden kunnen opmerkingen regels die met het symbool beginnen # opnemen.

U kunt meerdere antwoordbestanden opgeven. Houd er echter rekening mee AzCopy biedt geen ondersteuning voor geneste antwoordbestanden.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="y"></a>/Y

Alle AzCopy bevestiging vragen onderdrukt.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="l"></a>/ L

Hiermee geeft u een bewerking in de lijst. Er zijn geen gegevens worden gekopieerd.

AzCopy interpreteert de met behulp van deze optie als een simulatie voor het werken met de opdrachtregel zonder deze optie/l en tellingen hoeveel objecten worden gekopieerd, kunt u de optie /V op hetzelfde moment om te controleren welke objecten worden gekopieerd in het uitgebreide logboekbestand opgeven.

Het gedrag van deze optie is ook afhankelijk van de locatie van de brongegevens en de aanwezigheid van de recursieve modus/s en de bestandsnaam patroon optie /Pattern.

AzCopy vereist lijst weergeven en lezen van de bronlocatie van deze wanneer u deze optie.

**Van toepassing op:** Blobs, bestanden

### <a name="mt"></a>/MT

Hiermee stelt u het gedownloade bestand tijd laatste wijziging moet dezelfde zijn als de bron-blob of van het bestand.

**Van toepassing op:** Blobs, bestanden

### <a name="xn"></a>/XN

Sluit een nieuwere bronresource. De resource niet als de tijd voor laatst gewijzigd van de bron dezelfde wordt gekopieerd of nieuwer zijn dan het doel.

**Van toepassing op:** Blobs, bestanden

### <a name="xo"></a>/XO
Sluit een oudere bronserver resource. De resource niet als de tijd voor laatst gewijzigd van de bron dezelfde wordt gekopieerd of ouder zijn dan het doel.

**Van toepassing op:** Blobs, bestanden

### <a name="a"></a>/A

Alleen bestanden die het kenmerk Archief hebt geüpload.

**Van toepassing op:** Blobs, bestanden

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Alleen bestanden die een van de opgegeven kenmerken set hebt geüpload.

Beschikbare kenmerken zijn onder andere:

* R = alleen-lezen bestanden
* Een = bestanden klaar voor archivering
* S = systeembestanden
* H = Verborgen bestanden
* C = gecomprimeerde bestanden
* N = normale bestanden
* E = versleutelde bestanden
* T = tijdelijke bestanden
* O = offlinebestanden
* Ik = niet-geïndexeerde bestanden

**Van toepassing op:** Blobs, bestanden

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Sluit bestanden die een of meer van de opgegeven kenmerken is ingesteld.

Beschikbare kenmerken zijn onder andere:

* R = alleen-lezen bestanden
* Een = bestanden klaar voor archivering
* S = systeembestanden
* H = Verborgen bestanden
* C = gecomprimeerde bestanden
* N = normale bestanden
* E = versleutelde bestanden
* T = tijdelijke bestanden
* O = offlinebestanden
* Ik = niet-geïndexeerde bestanden

**Van toepassing op:** Blobs, bestanden

### <a name="delimiterdelimiter"></a>/ Scheidingsteken: "scheidingsteken"

Hiermee geeft u het scheidingsteken dat wordt gebruikt voor het scheiden van virtuele mappen in een blob-naam.

Standaard maakt gebruik van AzCopy / als scheidingsteken. AzCopy ondersteunt echter met behulp van een algemene teken (zoals @, # of %) als scheidingsteken. Als u een van deze speciale tekens op de opdrachtregel opnemen moet, plaatst u de bestandsnaam tussen dubbele aanhalingstekens.

Deze optie is alleen van toepassing voor het downloaden van blobs.

**Van toepassing op:** Blobs

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: 'getal-van-gelijktijdige-operations'

Hiermee geeft u het aantal gelijktijdige bewerkingen.

AzCopy standaard wordt een bepaald aantal gelijktijdige bewerkingen te verhogen van de overdracht gegevensdoorvoer gestart. Houd er rekening mee dat groot aantal gelijktijdige bewerkingen in een omgeving met lage bandbreedte mogelijk de netwerkverbinding overbelast en te voorkomen dat de bewerkingen volledig is voltooid. Vertraging gelijktijdige bewerkingen op basis van de werkelijke beschikbare netwerkbandbreedte.

De maximale limiet voor gelijktijdige bewerkingen is 512.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | 'Tabel'

Hiermee wordt aangegeven dat de `source` bron is een blob beschikbaar zijn in de lokale ontwikkelomgeving wordt uitgevoerd in de opslagemulator.

**Van toepassing op:** Blobs, tabellen

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | 'Tabel'

Hiermee wordt aangegeven dat de `destination` bron is een blob beschikbaar zijn in de lokale ontwikkelomgeving wordt uitgevoerd in de opslagemulator.

**Van toepassing op:** Blobs, tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: "key&#1;key2 key&#3;..."

Splitst de partitiesleutelbereik zodat de tabelgegevens parallel de snelheid van de exportbewerking verhoogt te exporteren.

Als deze optie niet is opgegeven, gebruikt AzCopy één thread tabelentiteiten exporteren. Bijvoorbeeld, als de gebruiker opgeeft /PKRS: "aa #bb" en vervolgens AzCopy begint drie gelijktijdige bewerkingen.

Elke bewerking exporteert een van drie partitie sleutel bereiken, zoals hieronder wordt weergegeven:

  [eerste partitiesleutel, aa)

  [aa, bb)

  [bb, laatste partitiesleutel]

**Van toepassing op:** tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: 'bestandsgrootte'

Hiermee geeft u het geëxporteerde bestand met grootte in MB, de minimaal toegestane waarde splitsen is 32.

Als deze optie niet is opgegeven, wordt met AzCopy tabelgegevens exporteert naar één bestand.

Als de tabelgegevens worden geëxporteerd naar een blob en de grootte van het geëxporteerde bestand met de limiet van 200 GB voor de Blobgrootte van de bereikt, klikt u vervolgens splitst AzCopy het geëxporteerde bestand, zelfs als deze optie is niet opgegeven.

**Van toepassing op:** tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: 'InsertOrSkip' | 'InsertOrMerge' | 'InsertOrReplace'

Hiermee geeft u het gedrag van tabel gegevens importeren.

* InsertOrSkip - slaat een bestaande entiteit of een nieuwe entiteit ingevoegd als het bestand bestaat niet in de tabel.
* InsertOrMerge - samenvoegingen van een bestaande entiteit of een nieuwe entiteit ingevoegd als het bestand bestaat niet in de tabel.
* InsertOrReplace - wordt vervangen door een bestaande entiteit of een nieuwe entiteit ingevoegd als het bestand bestaat niet in de tabel.

**Van toepassing op:** tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: 'manifest file'

Hiermee geeft u het manifestbestand voor de tabel exporteren en importeren.

Deze optie is optioneel tijdens de exportbewerking, AzCopy genereert een manifestbestand met vooraf gedefinieerde naam als deze optie niet is opgegeven.

Deze optie is vereist tijdens de importbewerking voor het zoeken van de gegevensbestanden.

**Van toepassing op:** tabellen

### <a name="synccopy"></a>/ SyncCopy

Geeft aan of synchroon kopiëren van bestanden tussen de twee eindpunten voor Azure Storage of BLOB's.

AzCopy standaard gebruikt serverzijde asynchrone kopie. Geef deze optie voor het uitvoeren van een synchrone kopie blobs of bestanden gedownload naar het lokale geheugen en vervolgens geüpload naar Azure Storage.

U kunt deze optie gebruiken bij het kopiëren van bestanden in Blob storage, binnen de opslag van bestanden of van Blob-opslag naar de opslag van bestanden of vice versa.

**Van toepassing op:** Blobs, bestanden

### <a name="setcontenttypecontent-type"></a>/ SetContentType: 'content-type'

Hiermee geeft u het MIME-inhoudstype voor bestemming blobs of bestanden.

AzCopy wordt het type inhoud voor een blob of het bestand is ingesteld op application/octet-stream standaard. U kunt het type inhoud voor alle blobs of bestanden instellen door een waarde op voor deze optie expliciet op te geven.

Als u deze optie geen waarde opgeeft, stelt AzCopy elke blob of inhoudstype volgens de bestandsextensie van het bestand.

**Van toepassing op:** Blobs, bestanden

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: 'JSON' | 'CSV'

Hiermee geeft u de indeling van het bestand met geëxporteerde gegevens.

Als deze optie niet is opgegeven, standaard exporteert AzCopy gegevensbestand van de tabel in JSON-indeling.

**Van toepassing op:** tabellen

## <a name="known-issues-and-best-practices"></a>Bekende problemen en aanbevolen procedures

U gaat nu een overzicht van enkele van de bekende problemen en aanbevolen procedures.

### <a name="limit-concurrent-writes-while-copying-data"></a>Limiet voor gelijktijdige schrijfbewerkingen tijdens het kopiëren van gegevens

Als u blobs of bestanden met AzCopy kopieert, houd er rekening mee dat een andere toepassing de gegevens heeft terwijl u kopieert. Indien mogelijk, zorg dat de gegevens die u wilt kopiëren niet tijdens de kopieerbewerking wordt gewijzigd. Bijvoorbeeld bij het kopiëren van een VHD die is gekoppeld aan een virtuele machine van Azure, zorg dat er geen andere toepassingen momenteel voor de VHD schrijft. Een goede manier om dit te doen is door het leasen van de resource te worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en kopieer de momentopname.

Als u niet voorkomen andere toepassingen dat bij het schrijven naar blobs of bestanden, terwijl ze worden gekopieerd, klikt u vervolgens Houd er rekening mee dat op het moment dat de taak is voltooid, de gekopieerde resources niet meer mogelijk volledige pariteit met de bron-resources.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Één AzCopy-sessie uitvoeren op één computer.

AzCopy is ontworpen om u te maximaliseren van uw machinebron de gegevensoverdracht versnellen, is het raadzaam u slechts één exemplaar van AzCopy uitvoeren op één machine en selecteer de optie `/NC` als u meer gelijktijdige bewerkingen nodig. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS-compatibele MD5-algoritmen voor AzCopy inschakelen wanneer u "Gebruik compatibele FIPS-algoritmen voor versleuteling, hashing en ondertekening."

AzCopy standaard .NET MD5-implementatie gebruikt voor het berekenen van de MD5 bij het kopiëren van objecten, maar er zijn bepaalde beveiligingsvereisten voor de die AzCopy inschakelen FIPS-compatibele MD5 instelling nodig.

U kunt een app.config-bestand maken `AzCopy.exe.config` met de eigenschap `AzureStorageUseV1MD5` en reserveer met AzCopy.exe plaatsen.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Voor de eigenschap 'AzureStorageUseV1MD5':

* De standaardwaarde AzCopy gebruikt waar - .NET MD5-implementatie.
* ONWAAR – FIPS-compatibele MD5-algoritme maakt gebruik van AzCopy.

Compatibele FIPS-algoritmen zijn standaard uitgeschakeld in Windows. U kunt deze instelling wijzigen op uw computer. Typ in het venster uitvoeren (Windows + R) secpol.msc openen de **lokaal beveiligingsbeleid** venster. In de **beveiligingsinstellingen** venster, gaat u naar **beveiligingsinstellingen** > **lokaal beleid** > **beveiligingsopties**. Zoek de **Systeemcryptografie: gebruik compatibele FIPS-algoritmen voor versleuteling, hashing en ondertekening** beleid. Dubbelklik op het beleid voor een overzicht van de waarde weergegeven in de **beveiligingsinstelling** kolom.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

### <a name="azure-storage-documentation"></a>Documentatie bij Azure Storage:
* [Inleiding tot Azure Storage](../storage-introduction.md)
* [Het Blob storage gebruiken met .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Hoe File storage gebruiken met .NET](../storage-dotnet-how-to-use-files.md)
* [Hoe Table storage gebruiken met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Het maken, beheren of een opslagaccount verwijderen](../storage-create-storage-account.md)
* [Gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage-blogberichten:
* [Inleiding tot Azure Storage Data Movement bibliotheek-Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introducing synchrone kopiëren en aangepaste type inhoud](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Aangekondigd algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 met de ondersteuning voor de tabel en de bestandsnaam](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Geoptimaliseerd voor grootschalige kopie scenario 's](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Ondersteuning voor geografisch redundante opslag met leestoegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Gegevensoverdracht met modus voor opnieuw starten en SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Cross-account kopiëren Blob met behulp](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Uploaden/downloaden van bestanden voor Azure Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)