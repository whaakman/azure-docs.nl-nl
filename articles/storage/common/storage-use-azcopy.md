---
title: Gegevens kopiëren of verplaatsen naar Azure Storage met AzCopy voor Windows | Microsoft Docs
description: De AzCopy op Windows-hulpprogramma gebruiken om te verplaatsen of kopiëren van gegevens naar of van blob-, tabel en bestandsinhoud. Gegevens kopiëren naar Azure Storage van lokale bestanden en gegevens binnen of tussen opslagaccounts kopiëren. Migreer uw gegevens eenvoudig naar Azure Storage.
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 08/13/2018
ms.author: seguler
ms.component: common
ms.openlocfilehash: a0e2400163433ba5290525d0fe807ede624a31ed
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42059435"
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a>Gegevens overdragen met het AzCopy voor Windows
AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar/van Microsoft Azure Blob-, bestands- en Table-opslag met behulp van eenvoudige opdrachten die zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

Er zijn twee versies van AzCopy die u kunt downloaden. AzCopy in Windows biedt Windows stijl opdrachtregelopties. [AzCopy in Linux](storage-use-azcopy-linux.md) is gericht op Linux-platforms bieden POSIX-stijl opdrachtregelopties. In dit artikel bevat informatie over AzCopy in Windows.

## <a name="download-and-install-azcopy-on-windows"></a>Download en installeer AzCopy op Windows

### <a name="latest-version-v81"></a>Meest recente versie (v8.1)
Download de [meest recente versie van AzCopy in Windows](https://aka.ms/downloadazcopy).

#### <a name="azcopy-on-windows-81-release-notes"></a>AzCopy in Windows 8.1 opmerkingen bij de Release
- Table-service wordt niet meer ondersteund in de meest recente versie. Als u de exportfunctie tabel, download u AzCopy 7.3-versie.
- Gebouwd met .NET Core 2.1 en alle afhankelijkheden van .NET Core worden nu geleverd bij de installatie.
- Toegevoegde ondersteuning voor OAuth-verificatie. Gebruik ```azcopy login``` om aan te melden met behulp van Azure Active Directory.

### <a name="azcopy-with-table-support-v73"></a>Azcopy met ondersteuning voor tabel (v7.3)
Download de [AzCopy 7.3 met ondersteuning voor tabel](https://aka.ms/downloadazcopynet).

### <a name="post-installation-step"></a>Stap na de installatie

Na de installatie van AzCopy in Windows met behulp van het installatieprogramma, open een opdrachtvenster en navigeer naar de installatiemap van AzCopy op uw computer - waar de `AzCopy.exe` uitvoerbare bestand zich bevindt. Indien gewenst, kunt u de locatie van de AzCopy-installatie toevoegen aan het systeempad staat. AzCopy wordt standaard geïnstalleerd in `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` of `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Schrijven van uw eerste AzCopy-opdracht

De eenvoudige syntaxis voor de AzCopy-opdrachten is:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

De volgende voorbeelden ziet een verscheidenheid aan scenario's voor het kopiëren van gegevens naar en van Microsoft Azure-Blobs, bestanden en tabellen. Raadpleeg de [AzCopy Parameters](#azcopy-parameters) sectie voor een gedetailleerde uitleg van de parameters die in elk voorbeeld gebruikt.

## <a name="download-blobs-from-blob-storage"></a>Downloaden van blobs van Blob-opslag

Laten we kijken op verschillende manieren om te downloaden van blobs met behulp van AzCopy.

### <a name="download-a-single-blob"></a>Één blob downloaden

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Houd er rekening mee dat als de map `C:\myfolder` niet bestaat, AzCopy gemaakt en gedownload `abc.txt ` naar de nieuwe map.

### <a name="download-a-single-blob-from-the-secondary-region"></a>Een enkele blob downloaden uit de secundaire regio

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld voor toegang tot de secundaire regio moet hebben.

### <a name="download-all-blobs-in-a-container"></a>Alle blobs in een container downloaden

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Wordt ervan uitgegaan dat de volgende BLOB's zich bevinden in de opgegeven container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Nadat het downloaden is uitgevoerd, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Als u geen optie opgeeft `/S`, geen blobs worden gedownload.

### <a name="download-blobs-with-a-specific-prefix"></a>Downloaden van blobs voor een bepaald voorvoegsel

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Wordt ervan uitgegaan dat de volgende BLOB's zich bevinden in de opgegeven container. Alle blobs die beginnen met het voorvoegsel `a` worden gedownload:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Nadat het downloaden is uitgevoerd, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Het voorvoegsel is van toepassing op de virtuele map die het eerste deel van de blob-naam. In het voorbeeld hierboven wordt weergegeven, de virtuele map komt niet overeen met het opgegeven voorvoegsel, zodat deze niet is gedownload. Bovendien, als de optie `/S` niet is opgegeven, alle bestaande blobs wordt niet gedownload door AzCopy.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd laatste wijziging van de geëxporteerde bestanden moet hetzelfde zijn als de bron-BLOB's instellen

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

U kunt ook blobs uitsluiten van de downloadbewerking op basis van hun tijd laatst gewijzigd. Bijvoorbeeld, als u wilt uitsluiten van blobs waarvan het laatste tijdstip gewijzigd is dezelfde of nieuwer is dan het doelbestand toevoegen de `/XN` optie:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Als u wilt uitsluiten van blobs waarvan het laatste tijdstip gewijzigd is dezelfde of ouder zijn dan het doelbestand toevoegen de `/XO` optie:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="upload-blobs-to-blob-storage"></a>Blobs uploaden naar Blob-opslag

Laten we kijken op verschillende manieren om te uploaden met AzCopy blobs.

### <a name="upload-a-single-blob"></a>Één blob uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Als de opgegeven doelcontainer niet bestaat, wordt deze door AzCopy gemaakt en wordt het bestand erin geüpload.

### <a name="upload-a-single-blob-to-a-virtual-directory"></a>Één blob uploaden naar een virtuele map

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Als de opgegeven virtuele map niet bestaat, AzCopy uploadt het bestand om op te nemen van de virtuele map in de naam ervan (*bijvoorbeeld*, `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="upload-all-blobs-in-a-folder"></a>Alle blobs in een map uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Optie op te geven `/S` wordt de inhoud van de opgegeven map geüpload naar Blob storage recursief, wat betekent dat alle submappen en de bestanden ook geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nadat de upload is uitgevoerd bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy wordt recursief niet uploaden. Nadat de upload is uitgevoerd bevat de container de volgende bestanden:

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

Nadat de upload is uitgevoerd bevat de container de volgende bestanden:

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

AzCopy wordt standaard ingesteld van het type inhoud van een bestemmings-blob naar `application/octet-stream`. Vanaf versie 3.1.0, u kunt expliciet aangeven het type inhoud via de optie `/SetContentType:[content-type]`. Deze syntaxis wordt het type inhoud voor alle blobs in een uploadbewerking ingesteld.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Als u opgeeft `/SetContentType` zonder een waarde ingesteld AzCopy elke blob of inhoudstype op basis van het de bestandsextensie van het bestand.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="copy-blobs-in-blob-storage"></a>Blobs in Blob-opslag kopiëren

Laten we kijken verschillende manieren om te kopiëren van blobs van de ene locatie naar de andere met behulp van AzCopy.

### <a name="copy-a-single-blob-from-one-container-to-another-within-the-same-storage-account"></a>Één blob van één container kopiëren naar een andere binnen hetzelfde opslagaccount 

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Wanneer u een blob in een opslagaccount kopieert een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-single-blob-from-one-storage-account-to-another"></a>Één blob van één opslagaccount naar de andere kopiëren

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Wanneer u een blob tussen opslagaccounts kopiëren, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-single-blob-from-the-secondary-region-to-the-primary-region"></a>Één blob van de secundaire regio kopiëren naar de primaire regio

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Houd er rekening mee dat u geografisch redundante opslag met leestoegang ingeschakeld voor toegang tot secundaire opslag nodig hebt.

### <a name="copy-a-single-blob-and-its-snapshots-from-one-storage-account-to-another"></a>Één blob en de momentopnamen ervan verwijderd van een opslagaccount kopiëren naar een andere

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Na de kopieerbewerking bevat de doelcontainer de blob en de momentopnamen ervan verwijderd. Ervan uitgaande dat de blob in het bovenstaande voorbeeld heeft twee momentopnamen, bevat de container de volgende blob en momentopnamen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="copy-all-blobs-in-a-container-to-another-storage-account"></a>Alle blobs in een container te kopiëren naar een ander opslagaccount 

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 
/Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /S
```

/S optie op te geven wordt de inhoud van de opgegeven container recursief geüpload. Zie [alle blobs in een map uploaden](#upload-all-blobs-in-a-folder) voor meer informatie en een voorbeeld.

### <a name="synchronously-copy-blobs-from-one-storage-account-to-another"></a>Synchroon blobs van één opslagaccount naar de andere kopiëren

AzCopy standaard worden gegevens gekopieerd tussen twee opslag eindpunten asynchroon. Daarom de kopieerbewerking wordt uitgevoerd in de achtergrond met behulp van ongebruikte bandbreedtecapaciteit geen SLA in termen van hoe u snel heeft een blob wordt gekopieerd en AzCopy controleert periodiek de kopiestatus totdat het kopiëren is voltooid of mislukt.

De `/SyncCopy` optie zorgt ervoor dat de kopieerbewerking consistente snelheid opgehaald. De synchrone kopie uitvoert AzCopy door te downloaden van de blobs te kopiëren uit de opgegeven bron in het lokale geheugen en vervolgens geüpload naar de bestemming van Blob storage.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy` aanvullende uitgaand verkeer kosten in vergelijking met asynchrone kopiëren kan genereren, de aanbevolen aanpak is het gebruik van deze optie in een Azure-virtuele machine die zich in dezelfde regio als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaand verkeer.

## <a name="download-files-from-file-storage"></a>Bestanden downloaden van File storage

Laten we kijken op verschillende manieren om te downloaden van bestanden met behulp van AzCopy.

### <a name="download-a-single-file"></a>Een enkel bestand downloaden

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Als de opgegeven bron een Azure-bestandsshare is, dan moet u de exacte bestandsnaam opgeven (*bijvoorbeeld* `abc.txt`) om te downloaden van een enkel bestand of de optie opgeven `/S` om alle bestanden in de share recursief te downloaden. Er wordt geprobeerd om op te geven van een bestandspatroon en de optie `/S` samen resulteert in een fout.

### <a name="download-all-files-in-a-directory"></a>Alle bestanden in een map downloaden

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Houd er rekening mee dat lege mappen niet worden gedownload.

## <a name="upload-files-to-an-azure-file-share"></a>Bestanden uploaden naar een Azure-bestandsshare

Laten we kijken op verschillende manieren voor het uploaden van bestanden met behulp van AzCopy.

### <a name="upload-a-single-file"></a>Een enkel bestand uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files-in-a-folder"></a>Alle bestanden in een map uploaden

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Houd er rekening mee dat lege mappen niet worden geüpload.

### <a name="upload-files-matching-a-specific-pattern"></a>Uploaden van bestanden die overeenkomen met een specifiek-patroon

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="copy-files-in-file-storage"></a>Bestanden in File storage kopiëren

Laten we kijken op verschillende manieren om te kopiëren van bestanden in een Azure-bestandsshare met behulp van AzCopy.

### <a name="copy-from-one-file-share-to-another"></a>Kopiëren van een bestandsshare naar een andere

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand via bestandsshares kopiëren, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-from-an-azure-file-share-to-blob-storage"></a>Kopiëren van een Azure-bestandsshare naar Blob-opslag

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand van de bestandsshare kopiëren naar de blob, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="copy-a-blob-from-blob-storage-to-an-azure-file-share"></a>Een blob kopiëren van Blob storage naar een Azure-bestandsshare

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Wanneer u een bestand van een blob naar een bestandsshare kopiëren, een [serverzijde kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) bewerking wordt uitgevoerd.

### <a name="synchronously-copy-files"></a>Synchroon kopiëren van bestanden

U kunt opgeven de `/SyncCopy` optie om gegevens te kopiëren van File Storage met File Storage, File Storage op Blob-opslag en van Blob-opslag met File Storage synchroon, AzCopy doet dit door te downloaden van de brongegevens naar lokaal geheugen en opnieuw aan bij uploaden bestemming. Standaard uitgaand verkeer in rekening gebracht.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Bij het kopiëren van File storage op Blob-opslag, is het blobtype blok-blob; kan de gebruiker de optie opgeven `/BlobType:page` te wijzigen van het blobtype.

Houd er rekening mee dat `/SyncCopy` genereren kosten voor aanvullende uitgaand in vergelijking met asynchrone kopiëren. De aanbevolen aanpak is het gebruik van deze optie in de Azure-virtuele machine die zich in dezelfde regio als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaand verkeer.

## <a name="export-data-from-table-storage"></a>Gegevens exporteren uit Table storage

Laten we eens exporteren van gegevens uit Azure Table storage met behulp van AzCopy.

### <a name="export-a-table"></a>Een tabel exporteren

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

Een manifestbestand schrijft AzCopy naar de opgegeven doelmap. Het manifestbestand wordt gebruikt in het importproces te zoeken naar de bestanden van de benodigde gegevens gegevensvalidatie uitvoeren. Het manifestbestand maakt standaard gebruik van de volgende naamconventie gebruikt:

    <account name>_<table name>_<timestamp>.manifest

Gebruiker kan ook de optie opgeven `/Manifest:<manifest file name>` om in te stellen van de naam van de manifest-bestand.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-an-export-from-table-storage-into-multiple-files"></a>Exporteren van een van de Table storage splitsen in meerdere bestanden

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy gebruikt een *volume index* in de bestandsnamen van de gesplitste gegevens te onderscheiden van meerdere bestanden. De index van het volume bestaat uit twee delen: een *partitie sleutelbereik index* en een *gesplitste bestandsindex*. Beide indexen zijn gebaseerd op nul.

De index van de belangrijkste bereik partitie is 0 als de gebruiker geen optie geeft `/PKRS`.

Stel bijvoorbeeld twee bestanden van AzCopy wordt gegenereerd nadat de gebruiker Hiermee geeft u de optie `/SplitSize`. De namen van de resulterende gegevens kunnen zijn:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Let op: de minimale mogelijke waarde voor de optie `/SplitSize` is 32 MB. Als de opgegeven bestemming is Blob storage, AzCopy het gegevensbestand splitst zodra de grootte bereikt de maximale grootte van blob (200GB), ongeacht of de optie `/SplitSize` is opgegeven door de gebruiker.

### <a name="export-a-table-to-json-or-csv-data-file-format"></a>Een tabel exporteren naar JSON- of CSV-bestandsindeling voor gegevens

Standaard exporteert u AzCopy tabellen naar JSON-gegevensbestanden. Kunt u de optie `/PayloadFormat:JSON|CSV` voor het exporteren van de tabellen als JSON- of CSV.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Wanneer u de indeling van de CSV-nettolading opgeeft, genereert AzCopy ook een schema-bestand met de bestandsextensie `.schema.csv` voor elk gegevensbestand.

### <a name="export-table-entities-concurrently"></a>Tabelitems gelijktijdig exporteren

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy wordt gestart gelijktijdige bewerkingen voor het exporteren van entiteiten als de gebruiker Hiermee geeft u de optie `/PKRS`. Elke bewerking wordt een partitiesleutelbereik geëxporteerd.

Houd er rekening mee dat het aantal gelijktijdige bewerkingen ook wordt bepaald door de optie `/NC`. AzCopy wordt het aantal coreprocessors gebruikt als de standaardwaarde van `/NC` bij het kopiëren van de tabelentiteiten, zelfs als `/NC` is niet opgegeven. Wanneer de gebruiker bevat de optie `/PKRS`, AzCopy maakt gebruik van de kleinste hoeveelheid van de twee waarden - partitie sleutelbereiken versus impliciet of expliciet opgegeven gelijktijdige bewerkingen - om te bepalen hoeveel gelijktijdige bewerkingen om te starten. Typ voor meer informatie, `AzCopy /?:NC` op de opdrachtregel.

### <a name="export-a-table-to-blob-storage"></a>Een tabel exporteren naar Blob-opslag

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy wordt een JSON-gegevens-bestand gegenereerd in de blob-container met de volgende naamconventie gebruikt:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Het gegenereerde JSON-gegevensbestand volgt de indeling van de nettolading voor minimale metagegevens. Zie voor meer informatie over deze indeling nettolading [indeling nettolading voor servicebewerkingen tabel](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Houd er rekening mee dat bij het exporteren van tabellen tot blobs, AzCopy de tabelitems naar lokale tijdelijke bestanden gedownload en uploadt vervolgens deze entiteiten naar de blob. Deze tijdelijke bestanden worden geplaatst in de map voor het logboek met het standaardpad '<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>', kunt u optie/Z: [logboek-bestanden en mappen] om te wijzigen van het logboek map bestandslocatie en dus de locatie van de tijdelijke gegevens-bestanden te wijzigen. De tijdelijke gegevens bestanden grootte wordt bepaald door uw tabelitems grootte en de grootte die u hebt opgegeven met de optie /SplitSize, hoewel het tijdelijke bestand in de lokale schijf wordt onmiddellijk verwijderd zodra deze is geüpload naar de blob, zorg ervoor dat u hebt onvoldoende lokale schijfruimte voor het opslaan van deze tijdelijke bestanden voordat ze worden verwijderd.

## <a name="import-data-into-table-storage"></a>Gegevens importeren in Table storage

Laten we eens importeren van gegevens in Azure Table storage met behulp van AzCopy.

### <a name="import-a-table"></a>Een tabel importeren

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

De optie `/EntityOperation` geeft aan hoe u entiteiten in de tabel invoegen. Mogelijke waarden zijn:

* `InsertOrSkip`: Hiermee slaat een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat.
* `InsertOrMerge`: Hiermee voegt een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat.
* `InsertOrReplace`: Vervangt een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat.

Houd er rekening mee dat kunt u de optie opgeven `/PKRS` in het scenario voor importeren. In tegenstelling tot de export-scenario, waarin u de optie moet opgeven `/PKRS` voor het starten van gelijktijdige bewerkingen, AzCopy gelijktijdige bewerkingen wordt standaard gestart bij het importeren van een tabel. Het aantal gelijktijdige bewerkingen gestart is gelijk aan het aantal coreprocessors; u kunt echter een ander aantal gelijktijdige met optie opgeven `/NC`. Typ voor meer informatie, `AzCopy /?:NC` op de opdrachtregel.

Houd er rekening mee dat AzCopy biedt alleen ondersteuning voor JSON, niet-CSV importeren. AzCopy niet ondersteunen tabel invoer van gebruiker gemaakte JSON en manifest van de bestanden. Beide van deze bestanden moeten afkomstig zijn van het exporteren van een AzCopy-tabel. Om fouten te voorkomen, kunt wijzigen van de geëxporteerde JSON of niet-manifestbestand.

### <a name="import-entities-into-a-table-from-blob-storage"></a>Entiteiten importeren in een tabel van Blob-opslag

Wordt ervan uitgegaan dat een Blob-container bevat het volgende: een JSON-bestand dat een Azure-tabel en het bijbehorende manifest-bestand.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

U kunt de volgende opdracht uit om te importeren van entiteiten in een tabel met behulp van het manifestbestand in deze blobcontainer uitvoeren:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Andere functies van AzCopy

Laten we eens enkele andere functies van AzCopy.

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen kopiëren van gegevens die niet in het doel bestaat.

De `/XO` en `/XN` parameters kunt u oudere of nieuwere bron resources uitsluiten wordt gekopieerd, respectievelijk. Als u wilt dat alleen bron-resources die niet bestaan in het doel wilt kopiëren, kunt u beide parameters in de AzCopy-opdracht:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Houd er rekening mee dat dit niet wordt ondersteund als de bron- of doelservers een tabel is.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Een antwoordbestand gebruiken om op te geven van opdrachtregelparameters

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

U kunt opdrachtregelparameters AzCopy opnemen in een antwoordbestand. AzCopy verwerkt de parameters in het bestand alsof ze waren is opgegeven op de opdrachtregel voor het uitvoeren van een directe vervangen door de inhoud van het bestand.

Wordt ervan uitgegaan dat een antwoordbestand met de naam `copyoperation.txt`, die de volgende regels bevat. Elke parameter AzCopy kan op één regel worden opgegeven

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

of op afzonderlijke regels:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy mislukt als u de parameter verdeeld over twee regels, zoals hier wordt weergegeven voor de `/sourcekey` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Meerdere antwoord-bestanden gebruiken om op te geven van opdrachtregelparameters

Wordt ervan uitgegaan dat een antwoordbestand met de naam `source.txt` die Hiermee geeft u een Broncontainer:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

En een antwoordbestand met de naam `dest.txt` die Hiermee geeft u een doelmap in het bestandssysteem:

    /Dest:C:\myfolder

En een antwoordbestand met de naam `options.txt` die Hiermee geeft u opties voor AzCopy:

    /S /Y

Om aan te roepen AzCopy met deze reactie-bestanden, die allemaal bevinden zich in een map `C:\responsefiles`, gebruikt u deze opdracht:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy verwerkt met deze opdracht net zoals deze zou doen als u alle van de afzonderlijke parameters op de opdrachtregel:

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

### <a name="journal-file-folder"></a>Bestandsmap logboek

Telkens wanneer die u een opdracht voor AzCopy gebruikt geven, wordt gecontroleerd of een logboekbestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. Het logboekbestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als een nieuw logboekbestand gegenereerd.

Als het logboekbestand bestaat, AzCopy wordt gecontroleerd of de opdrachtregel die u invoert, overeenkomt met de opdrachtregel in het logboekbestand. Als de twee regels van de opdracht overeenkomen, wordt de niet-voltooide bewerking door AzCopy hervat. Als ze niet overeenkomen, wordt u gevraagd om te overschrijven ofwel het logboekbestand te starten van een nieuwe bewerking of de huidige bewerking te annuleren.

Als u gebruiken van de standaardlocatie voor het logboekbestand wilt:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Als u de optie weglaat `/Z`, of de optie opgeven `/Z` zonder een pad naar de map, zoals hierboven, AzCopy maakt het logboekbestand op de standaardlocatie bevindt, dat is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Als het logboekbestand al bestaat, hervat de bewerking op basis van het logboekbestand met AzCopy.

Als u opgeven van een aangepaste locatie voor het logboekbestand wilt:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

In dit voorbeeld wordt het logboekbestand gemaakt als deze niet al bestaat. Als deze bestaat, hervat de bewerking op basis van het logboekbestand met AzCopy.

Als u een AzCopy-bewerking hervatten wilt:

```azcopy
AzCopy /Z:C:\journalfolder\
```

In dit voorbeeld hervat de laatste bewerking, die mogelijk niet voltooien.

### <a name="generate-a-log-file"></a>Genereren van een logboekbestand

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Als u de optie opgeven `/V` zonder op te geven van een pad naar het uitgebreide logboek, AzCopy maakt vervolgens het logboekbestand op de standaardlocatie bevindt, dat is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

U kunt anders een logboekbestand maken in een aangepaste locatie:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Houd er rekening mee dat als u een relatief pad volgen optie opgeeft `/V`, zoals `/V:test/azcopy1.log`, wordt het uitgebreide logboek in de huidige werkmap in een submap met de naam gemaakt `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal gelijktijdige bewerkingen om te starten

Optie `/NC` geeft het aantal gelijktijdige kopieerbewerkingen. AzCopy wordt standaard gestart van een bepaald aantal gelijktijdige bewerkingen om de overdracht van gegevensdoorvoer te vergroten. Voor bewerkingen van de tabel is het aantal gelijktijdige bewerkingen gelijk aan het aantal processors dat u hebt. Voor de Blob en bestand bewerkingen, het aantal gelijktijdige bewerkingen is gelijk aan 8 keer het aantal processors dat u hebt. Als u AzCopy via een netwerk met een lage bandbreedte uitvoert, kunt u een lager getal voor /NC om te voorkomen dat is mislukt, concurrentie opgeven.

### <a name="run-azcopy-against-the-azure-storage-emulator"></a>AzCopy uitvoeren in de Azure-Opslagemulator

U kunt uitvoeren AzCopy op basis van de [Azure-Opslagemulator](storage-use-emulator.md) voor blobs:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

U kunt ook het uitvoeren van tabellen:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

### <a name="automatically-determine-content-type-of-a-blob"></a>Type inhoud van een Blob automatisch te bepalen

AzCopy bepaalt type inhoud van een blob op basis van een JSON-bestand met de extensie bestandstoewijzing inhoudstype. Deze JSON-bestand met de naam AzCopyConfig.json en bevindt zich in de AzCopy-map. Als u een bestandstype dat zich niet in de lijst hebt, kunt u de toewijzing kunt toevoegen aan het JSON-bestand:

```
{
  "MIMETypeMapping": {
    ".myext": "text/mycustomtype",
    .
    .
  }
}
```     

## <a name="azcopy-parameters"></a>AzCopy-Parameters

Parameters voor AzCopy worden hieronder beschreven. U kunt ook een van de volgende opdrachten uit vanaf de opdrachtregel voor meer informatie over typen in met behulp van AzCopy:

* Voor gedetailleerde help voor AzCopy: `AzCopy /?`
* Voor gedetailleerde hulp met een parameter AzCopy: `AzCopy /?:SourceKey`
* Voor voorbeelden van de opdrachtregel: `AzCopy /?:Sample`

### <a name="sourcesource"></a>/ Source: 'bron'

Hiermee geeft u de brongegevens waaruit u wilt kopiëren. De bron kan zijn voor de systeemmap van een bestand, een blob-container, een virtuele map van de blob, een bestandsshare voor opslag, een map voor opslag of een Azure-tabel.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="destdestination"></a>/ Dest: 'doel'

Hiermee geeft u de bestemming om naar te kopiëren. Het doel mag een bestandssysteemmap, een blob-container, een virtuele map van de blob, een bestandsshare voor opslag, een map voor opslag of een Azure-tabel.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="patternfile-pattern"></a>/ Patroon: "bestandspatroon"

Hiermee geeft u een bestandspatroon dat wordt aangegeven welke bestanden te kopiëren. Het gedrag van de parameter /Pattern wordt bepaald door de locatie van de brongegevens en de aanwezigheid van de optie voor recursieve modus. Recursieve modus is opgegeven via de optie/s.

Als de opgegeven bron een map in het bestandssysteem is wordt standaard jokertekens van kracht worden en het opgegeven bestandspatroon wordt vergeleken met bestanden in de map. Als de optie die /s is opgegeven, klikt u vervolgens AzCopy ook komt overeen met het opgegeven patroon op basis van alle bestanden in submappen onder de map.

Als de opgegeven bron een blob-container of de virtuele map is, worden klikt u vervolgens jokertekens niet toegepast. Als het opgegeven bestandspatroon optie die /s is opgegeven, klikt u vervolgens AzCopy geïnterpreteerd als een blob-voorvoegsel. Als de optie die /s niet is opgegeven, klikt u vervolgens AzCopy komt overeen met het bestandspatroon tegen exacte blob-namen.

Als de opgegeven bron een Azure-bestandsshare is, wordt u moet de exacte bestandsnaam opgeven, (bijvoorbeeld abc.txt) als u wilt kopiëren van een enkel bestand of de optie opgeven /S om te kopiëren van alle bestanden in de share recursief. Er wordt geprobeerd om op te geven van beide een bestand patroon en de optie /S samen met resultaten in een fout.

AzCopy maakt gebruik van hoofdlettergevoelige overeenkomende wanneer de/Source een blob-container of blob virtuele map is en maakt gebruik van niet-hoofdlettergevoelige overeenkomende in alle andere gevallen.

Het patroon van de standaard-bestand gebruikt wanneer er geen bestandspatroon is opgegeven is *.* voor een locatie in het bestandssysteem of een lege voorvoegsel voor een Azure-opslaglocatie. Opgeven van meerdere-bestandpatronen wordt niet ondersteund.

**Van toepassing op:** Blobs, bestanden

### <a name="destkeystorage-key"></a>/ DestKey: 'storage-key'

Hiermee geeft u de opslagaccountsleutel voor de doelresource.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="destsassas-token"></a>/DestSAS:"sas-token"

Hiermee geeft u een Shared Access Signature (SAS) met machtigingen voor lezen en schrijven voor de bestemming (indien van toepassing). De SAS met dubbele aanhalingstekens rondom omdat deze mogelijk bevat speciale tekens van de opdrachtregel.

Als de doelresource een blob-container, een bestandsshare of een tabel is, kunt u deze optie gevolgd door het SAS-token opgeven of u kunt de SAS opgeven als onderdeel van de doel-blob-container, bestandsshare of de URI van de tabel, zonder deze optie.

Als de bron- als beide blobs, klikt u vervolgens de bestemmings-blob moet zich bevinden in hetzelfde opslagaccount als de bron-blob.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: 'storage-key'

Hiermee geeft u de opslagaccountsleutel voor de bron-resource.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcesassas-token"></a>/SourceSAS:"sas-token"

Hiermee geeft u een Shared Access Signature met lees- en lijst met machtigingen voor de bron (indien van toepassing). De SAS met dubbele aanhalingstekens rondom omdat deze mogelijk bevat speciale tekens van de opdrachtregel.

Als de bron-resource een blob-container is en een sleutel noch een SAS is opgegeven, wordt de blob-container gelezen via anonieme toegang.

Als de bron een bestandsshare is of een sleutel of een SAS tabel moet worden opgegeven.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="s"></a>/S

Hiermee geeft u een recursieve modus voor kopieerbewerkingen. In de modus voor recursieve kopieert AzCopy alle blobs of bestanden die overeenkomen met het opgegeven bestandspatroon, inclusief verbindingen in submappen.

**Van toepassing op:** Blobs, bestanden

### <a name="blobtypeblock--page--append"></a>/ BlobType: 'block' | "page" | "toevoegen"

Hiermee geeft u op of de bestemmings-blob een blok-blob, een pagina-blob of een toevoeg-blob is. Deze optie is alleen van toepassing alleen wanneer u een blob wilt uploaden. Anders wordt er een fout gegenereerd. Als de bestemming een blob is en deze optie niet, standaard opgegeven is, wordt een blok-blob door AzCopy gemaakt.

**Van toepassing op:** Blobs

### <a name="checkmd5"></a>/ CheckMD5

Een MD5-hash voor gedownloade gegevens berekend en controleert of de MD5-hash die zijn opgeslagen in de blob of inhoud-MD5-eigenschap van het bestand komt overeen met de berekende hash. De MD5-controle is standaard uitgeschakeld, zodat u deze optie om uit te voeren van de MD5-controle bij het downloaden van gegevens moet opgeven.

Houd er rekening mee dat Azure Storage biedt geen garantie dat de MD5-hash die zijn opgeslagen voor de blob of het bestand bijgewerkt is. Het is de verantwoordelijkheid van de client om bij te werken van de MD5 als de blob of het bestand wordt gewijzigd.

De inhoud-MD5-eigenschap voor een Azure blob of file AzCopy altijd ingesteld na het uploaden naar de service.  

**Van toepassing op:** Blobs, bestanden

### <a name="snapshot"></a>/ Momentopname

Geeft aan of om over te dragen van momentopnamen. Deze optie is alleen geldig wanneer de bron een blob is.

De overgebrachte blobmomentopnamen worden gewijzigd in deze indeling: .extensie blob-naam (momentopname-time)

Momentopnamen worden standaard niet gekopieerd.

**Van toepassing op:** Blobs

### <a name="vverbose-log-file"></a>/ V: [uitgebreide-log-bestand]

Uitvoer uitgebreide statusberichten in een logboekbestand.

Het logboekbestand heet standaard AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Als u de locatie van een bestaande voor deze optie opgeeft, wordt het uitgebreide logboek naar dat bestand toegevoegd.  

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="zjournal-file-folder"></a>/ Z: [logboek-bestanden en mappen]

Hiermee geeft u een logboekmap-bestand voor het hervatten van een bewerking.

AzCopy ondersteunt altijd hervat als een bewerking is onderbroken.

Als deze optie niet opgegeven is of deze is opgegeven zonder een mappad, maakt AzCopy vervolgens het logboekbestand op de standaardlocatie bevindt, % LocalAppData%\Microsoft\Azure\AzCopy is.

Telkens wanneer die u een opdracht voor AzCopy gebruikt geven, wordt gecontroleerd of een logboekbestand in de standaardmap bestaat en of deze bestaat in een map die u hebt opgegeven via deze optie. Het logboekbestand bestaat niet op beide plaatsen, AzCopy wordt de bewerking wordt beschouwd als nieuwe als een nieuw logboekbestand gegenereerd.

Als het logboekbestand bestaat, AzCopy wordt gecontroleerd of de opdrachtregel die u invoert, overeenkomt met de opdrachtregel in het logboekbestand. Als de twee regels van de opdracht overeenkomen, wordt de niet-voltooide bewerking door AzCopy hervat. Als ze niet overeenkomen, wordt u gevraagd om te overschrijven ofwel het logboekbestand te starten van een nieuwe bewerking of de huidige bewerking te annuleren.

Het logboekbestand wordt verwijderd na voltooiing van de bewerking.

Houd er rekening mee dat een bewerking van een logboekbestand gemaakt met een eerdere versie van AzCopy hervatten niet wordt ondersteund.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="parameter-file"></a>/@:"parameter-File"

Hiermee geeft u een bestand met parameters. AzCopy verwerkt de parameters in het bestand net alsof ze waren is opgegeven op de opdrachtregel.

In een antwoordbestand, kunt u meerdere parameters op één regel opgeven of elke parameter op een eigen regel opgeven. Houd er rekening mee dat een afzonderlijke parameter kan niet worden gebruikt voor het bereik van meerdere regels.

Antwoord-bestanden kunnen opmerkingen regels die met het symbool beginnen # bevatten.

U kunt meerdere antwoord-bestanden opgeven. Houd er echter rekening mee AzCopy biedt geen ondersteuning voor geneste antwoord bestanden.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="y"></a>/Y

Vragen om bevestiging van alle AzCopy onderdrukt. Deze optie kan ook het gebruik van alleen-schrijven SAS-tokens voor scenario's uploaden van gegevens, wanneer /XO en /XN zijn niet opgegeven.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="l"></a>/L

Hiermee geeft u de bewerking van een aanbieding alleen; Er zijn geen gegevens worden gekopieerd.

AzCopy interpreteert de met behulp van deze optie als een simulatie voor het werken met de opdrachtregel zonder deze optie/l en telt het aantal objecten worden gekopieerd, kunt u de optie /V op hetzelfde moment om te controleren welke objecten zijn gekopieerd in het uitgebreide logboek opgeven.

Het gedrag van deze optie wordt ook bepaald door de locatie van de brongegevens en de aanwezigheid van de recursieve modus /S en de bestandsnaam patroon optie /Pattern.

AzCopy is lijst en lezen toestemming van de locatie van deze vereist wanneer u deze optie.

**Van toepassing op:** Blobs, bestanden

### <a name="mt"></a>/MT

Hiermee stelt u het gedownloade bestand laatst gewijzigd tijd moet dezelfde zijn als de bron-blob of van het bestand.

**Van toepassing op:** Blobs, bestanden

### <a name="xn"></a>/XN

Sluit een nieuwere bron-resource. De resource is niet gekopieerd als de laatst gewijzigd van de bron hetzelfde is of nieuwer zijn dan de bestemming.

**Van toepassing op:** Blobs, bestanden

### <a name="xo"></a>/XO
Niet van toepassing op een oudere bron-resource. De resource is niet gekopieerd als de laatst gewijzigd van de bron hetzelfde is of ouder zijn dan de bestemming.

**Van toepassing op:** Blobs, bestanden

### <a name="a"></a>/A

Alleen bestanden die het kenmerk Archief hebt geüpload.

**Van toepassing op:** Blobs, bestanden

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Alleen bestanden die een van de opgegeven kenmerken set hebt geüpload.

Beschikbare kenmerken zijn onder andere:

* R = alleen-lezen bestanden
* Een = bestanden gereed voor archivering
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

Sluit bestanden waarvoor een van de opgegeven kenmerken instellen.

Beschikbare kenmerken zijn onder andere:

* R = alleen-lezen bestanden
* Een = bestanden gereed voor archivering
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

Geeft aan dat het scheidingsteken dat wordt gebruikt voor het scheiden van de virtuele mappen in een blobnaam.

Standaard maakt gebruik van AzCopy / als het scheidingsteken. Echter, AzCopy ondersteunt het gebruik van een algemene teken (zoals @, #, of %) als scheidingsteken. Als u een van deze speciale tekens op de opdrachtregel opnemen moet, plaatst u de naam van het bestand met dubbele aanhalingstekens.

Deze optie is alleen van toepassing voor het downloaden van blobs.

**Van toepassing op:** Blobs

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "getal-van-gelijktijdige-bewerkingen"

Hiermee geeft u het aantal gelijktijdige bewerkingen.

AzCopy standaard wordt gestart voor een bepaald aantal gelijktijdige bewerkingen om de overdracht van gegevensdoorvoer te vergroten. Houd er rekening mee dat groot aantal gelijktijdige bewerkingen in een omgeving met een lage bandbreedte kan de netwerkverbinding blijvend wordt overbelast en te voorkomen dat de bewerkingen volledig is voltooid. Vertraging gelijktijdige bewerkingen op basis van werkelijke beschikbare netwerkbandbreedte.

De bovenste limiet voor gelijktijdige bewerkingen is 512.

**Van toepassing op:** Blobs, bestanden, tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: 'Blob' | 'Tabel'

Hiermee wordt aangegeven dat de `source` resource is een blob die beschikbaar zijn in de lokale ontwikkelomgeving, die worden uitgevoerd in de opslagemulator.

**Van toepassing op:** Blobs, tabellen

### <a name="desttypeblob--table"></a>/ DestType: 'Blob' | 'Tabel'

Hiermee wordt aangegeven dat de `destination` resource is een blob die beschikbaar zijn in de lokale ontwikkelomgeving, die worden uitgevoerd in de opslagemulator.

**Van toepassing op:** Blobs, tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: ' key1 #key2 key3 #... '

Hiermee wordt de partitiesleutelbereik om in te schakelen tabelgegevens in parallelle, waardoor de snelheid van de exportbewerking te exporteren.

Als deze optie niet opgegeven is, klikt u vervolgens AzCopy maakt gebruik van een enkele thread tabelitems exporteren. Bijvoorbeeld, als de gebruiker opgeeft /PKRS: "aa #bb" en vervolgens AzCopy begint drie gelijktijdige bewerkingen.

Elke bewerking exporteert een van drie partitie sleutelbereiken, zoals hieronder wordt weergegeven:

  [eerste partitiesleutel, aa)

  [aa, bb)

  [bb, laatste partitiesleutel]

**Van toepassing op:** tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: "bestandsgrootte"

Hiermee geeft u het geëxporteerde bestand met grootte in MB, de minimaal toegestane waarde splitsen is 32.

Als deze optie niet opgegeven is, exporteert AzCopy tabelgegevens naar één bestand.

Als de gegevens van de tabel wordt geëxporteerd naar een blob en de grootte van het geëxporteerde bestand met de limiet van 200 GB voor de blobgrootte bereikt, klikt u vervolgens splitst AzCopy het geëxporteerde bestand, zelfs als deze optie is niet opgegeven.

**Van toepassing op:** tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Hiermee geeft u de tabelgedrag voor het importeren van gegevens.

* InsertOrSkip - slaat een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet bestaat in de tabel.
* InsertOrMerge - samenvoegingen van een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet bestaat in de tabel.
* InsertOrReplace - wordt vervangen door een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet bestaat in de tabel.

**Van toepassing op:** tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: 'manifest-bestand'

Hiermee geeft u de manifest-bestand voor de tabel exporteren en importeren van de bewerking.

Deze optie is optioneel tijdens de exportbewerking, AzCopy genereert een manifestbestand met de naam van de vooraf gedefinieerde als deze optie is niet opgegeven.

Deze optie is vereist tijdens de importbewerking voor het opsporen van de gegevensbestanden.

**Van toepassing op:** tabellen

### <a name="synccopy"></a>/ SyncCopy

Hiermee wordt aangegeven of synchroon blobs of bestanden tussen de twee eindpunten van Azure Storage te kopiëren.

AzCopy standaard maakt gebruik van server-side asynchrone kopiëren. Geef deze optie om uit te voeren een synchrone kopie blobs of bestanden gedownload naar het lokale geheugen en vervolgens geüpload naar Azure Storage.

U kunt deze optie gebruiken bij het kopiëren van bestanden in Blob-opslag in File storage, of van Blob-opslag met File storage of vice versa.

**Van toepassing op:** Blobs, bestanden

### <a name="setcontenttypecontent-type"></a>/ SetContentType: 'inhoudstype'

Hiermee geeft u het MIME-inhoudstype voor doel-blobs of -bestanden.

AzCopy wordt het type inhoud voor een blob of file ingesteld op application/octet-stream standaard. U kunt het type inhoud voor alle blobs of bestanden instellen door expliciet een waarde voor deze optie op te geven.

Als u deze optie geen waarde opgeeft, stelt AzCopy elke blob of inhoudstype op basis van het de bestandsextensie van het bestand.

**Van toepassing op:** Blobs, bestanden

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: 'JSON' | "CSV"

Hiermee geeft u de indeling van het geëxporteerde tabelbestand.

Als deze optie niet opgegeven is, wordt standaard AzCopy exporteert gegevensbestand van de tabel in JSON-indeling.

**Van toepassing op:** tabellen

## <a name="known-issues-and-best-practices"></a>Bekende problemen en aanbevolen procedures

Laten we eens enkele bekende problemen en aanbevolen procedures.

### <a name="limit-concurrent-writes-while-copying-data"></a>Gelijktijdige schrijfbewerkingen tijdens het kopiëren van gegevens beperken

Wanneer u blobs of bestanden met AzCopy kopieert, houd er rekening mee dat een andere toepassing kan worden het wijzigen van gegevens terwijl u kopieert. Indien mogelijk, ervoor te zorgen dat de gegevens die u wilt kopiëren niet wordt gewijzigd tijdens de kopieerbewerking. Bijvoorbeeld, bij het kopiëren van een VHD die is gekoppeld aan een virtuele machine van Azure, moet u ervoor dat er geen andere toepassingen ontwikkelt die momenteel op de VHD. Een goede manier om dit te doen is door de overdracht van de resource moet worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en kopieer vervolgens de momentopname.

Als u niet voorkomen andere toepassingen dat bij het schrijven naar blobs of de bestanden terwijl ze worden gekopieerd, klikt u vervolgens Houd er rekening mee dat op het moment dat de taak is voltooid, de gekopieerde resources niet meer mogelijk volledige pariteit met de bron-resources.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Inschakelen van FIPS-compatibele MD5-algoritmen voor AzCopy wanneer u "Gebruik compatibele FIPS-algoritmen voor versleuteling, hashing en ondertekening."

AzCopy standaard .NET MD5-implementatie gebruikt voor het berekenen van de MD5 bij het kopiëren van objecten, maar er zijn enkele beveiligingsvereisten waarvoor AzCopy inschakelen van FIPS-compatibele MD5-instelling.

U kunt een app.config-bestand maken `AzCopy.exe.config` met eigenschap `AzureStorageUseV1MD5` en aside met AzCopy.exe plaatsen.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Voor de eigenschap 'AzureStorageUseV1MD5':

* Waar - de standaardwaarde, AzCopy .NET MD5-implementatie gebruikt.
* ONWAAR – FIPS-compatibele MD5-algoritme maakt gebruik van AzCopy.

Compatibele FIPS-algoritmen zijn standaard uitgeschakeld in Windows. U kunt deze instelling wijzigen op uw computer. Typ in het venster uitvoeren (Windows + R) secpol.msc openen de **lokaal beveiligingsbeleid** venster. In de **beveiligingsinstellingen** venster, Ga naar **beveiligingsinstellingen** > **lokaal beleid** > **beveiligingsopties**. Zoek de **Systeemcryptografie: gebruik compatibele FIPS-algoritmen voor versleuteling, hashing en ondertekening** beleid. Dubbelklik op het beleid om te zien van de waarde die wordt weergegeven de **beveiligingsinstelling** kolom.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

### <a name="azure-storage-documentation"></a>Documentatie voor Azure Storage:
* [Kennismaking met Azure Storage](../storage-introduction.md)
* [Het Blob storage gebruiken met .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Hoe File storage gebruiken met .NET](../storage-dotnet-how-to-use-files.md)
* [Hoe u kunt Table storage gebruiken met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Over het maken, beheren of verwijderen van een storage-account](../storage-create-storage-account.md)
* [Gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage-blogberichten:
* [Maak kennis met de verplaatsing van Azure Storage-bibliotheek Gegevensvoorbeeld](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introductie van synchrone kopiëren en aangepaste inhoudstype](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Aankondiging van algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 met ondersteuning voor tabel en bestand](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Geoptimaliseerd voor scenario's voor grootschalige kopiëren](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Ondersteuning voor geografisch redundante opslag met leestoegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Gegevens overdragen met modus voor opnieuw starten en SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Kopiëren van de Blob cross-account met behulp](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Uploaden/downloaden van bestanden voor Azure-Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
