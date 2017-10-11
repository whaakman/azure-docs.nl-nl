---
title: Harde schijven voor een Azure Import/Export-import-taak - v1 voorbereiden | Microsoft Docs
description: Informatie over het voorbereiden van harde schijven de WAImportExport v1-hulpprogramma gebruiken voor het maken van een import-taak voor de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 3d818245-8b1b-4435-a41f-8e5ec1f194b2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 361e16262e528c7dea1bab4b9d945a28af8be399
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Harde schijven voorbereiden voor een importtaak
Als u een of meer harde schijven voor een import-taak, de volgende stappen uit:

-   Bepaal welke gegevens importeren in de Blob-service

-   Doel-virtuele mappen en blobs in de Blob-service identificeren

-   Het aantal stations dat u moet bepalen

-   Kopieer de gegevens naar elk van de vaste schijven

 Zie voor een voorbeeldwerkstroom [voorbeeldwerkstroom naar harde schijven voorbereiden voor een Import-taak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-the-data-to-be-imported"></a>Bepaal welke gegevens moeten worden geïmporteerd
 De eerste stap bij het maken van een import-taak is om te bepalen welke mappen en bestanden die u gaat importeren. Dit kan een lijst met mappen, een lijst met unieke bestanden of een combinatie van deze twee zijn. Wanneer een directory opgenomen is, worden alle bestanden in de map en de bijbehorende submappen deel uit van de import-taak.

> [!NOTE]
>  Aangezien submappen opgenomen recursief worden wanneer een van de bovenliggende map opgenomen is, geeft u alleen de bovenliggende map. Geef geen ook op een van de bijbehorende submappen.
>
>  Het hulpprogramma voor importeren/exporteren van Microsoft Azure heeft momenteel de volgende beperking: als een map meer gegevens bevat dan een harde schijf kan bevatten, klikt u vervolgens de map moet worden opgedeeld in kleinere mappen. Bijvoorbeeld, als een map 2,5 TB aan gegevens bevat en capaciteit van de harde schijf alleen 2TB is, moet u de map 2,5 TB opdelen in kleinere mappen. Deze beperking wordt opgelost in een latere versie van het hulpprogramma.

## <a name="identify-the-destination-locations-in-the-blob-service"></a>De bestemmingslocatie in de blob-service identificeren
 Voor elk bestand of map die worden geïmporteerd, moet u een doel-virtuele map of een blob in de Azure Blob-service identificeren. U gebruikt deze doelen als invoer voor de Azure-hulpprogramma voor importeren/exporteren. Houd er rekening mee dat mappen moeten worden gescheiden met een slash-teken '/'.

 De volgende tabel ziet u enkele voorbeelden van blob-doelen:

|Bronbestand of map|Bestemmings-blob of virtuele map|
|------------------------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|
|K:\Temp\FavoriteVideo.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|

## <a name="determine-how-many-drives-are-needed"></a>Bepalen hoeveel stations nodig zijn
 Vervolgens moet u bepalen:

-   Het aantal harde schijven die nodig zijn voor het opslaan van de gegevens.

-   De mappen en/of zelfstandige bestanden die worden gekopieerd naar elk van de vaste schijf.

 Zorg ervoor dat u het aantal harde schijven, moet u de gegevens die u overbrengt opslaan.

## <a name="copy-data-to-your-hard-drive"></a>Gegevens kopiëren naar de harde schijf
 Deze sectie beschrijft het aanroepen van het Azure Import/Export-hulpprogramma voor het kopiëren van uw gegevens naar een of meer harde schijven. Elke keer dat u de Azure-hulpprogramma voor importeren/exporteren kunt aanroepen, maakt u een nieuw *kopiëren sessie*. Maken van de sessie ten minste één exemplaar voor elke schijf waarop u gegevens; kopiëren in sommige gevallen moet u mogelijk meer dan één exemplaar sessie al uw gegevens te kopiëren naar één station. Hier volgen enkele oorzaken die mogelijk moet u meerdere kopie sessies:

-   U moet voor elke schijf die u naar kopiëren een afzonderlijke kopie-sessie maken.

-   Een kopieersessie kan één map of één blob kopiëren naar het station. Als u meerdere mappen, meerdere blobs of een combinatie van beide kopieert, hebt u nodig om meerdere kopie sessies te maken.

-   U kunt de eigenschappen en metagegevens die worden ingesteld op de blobs geïmporteerd als onderdeel van een import-taak opgeven. De eigenschappen of metagegevens die u voor een kopieersessie opgeeft wordt toegepast op alle blobs die door die sessie exemplaar opgegeven. Als u opgeven van andere eigenschappen of metagegevens voor enkele blobs wilt, hebt u nodig om de sessie van een afzonderlijke kopie te maken. Zie [eigenschappen instellen en metagegevens tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import-v1.md)voor meer informatie.

> [!NOTE]
>  Als u meerdere machines die voldoen aan de vereisten die worden beschreven hebt in [instelling van de Azure Import/Export Tool](storage-import-export-tool-setup-v1.md), kunt u gegevens kopiëren naar meerdere schijven parallel door een exemplaar van dit hulpprogramma wordt uitgevoerd op elke machine.

 Voor elke harde schijf die u met het hulpprogramma voor Azure Import/Export voorbereiden, maakt het hulpprogramma een enkele journal-bestand. U moet de journaal-bestanden van alle stations de import-taak maken. Het logboek-bestand kan ook worden gebruikt om te hervatten voorbereiding van het station als het hulpprogramma wordt onderbroken.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>De syntaxis van het Azure Import/Export hulpprogramma voor een import-taak
 Aanroepen om voor te bereiden stations voor een import-taak, het Azure Import/Export-hulpprogramma met de **PrepImport** opdracht. Welke parameters die u wilt opnemen, hangt af van of dit de eerste kopieersessie of een latere kopieersessie.

 De eerste kopieersessie voor een station is vereist voor een aantal extra parameters om op te geven van de opslagaccountsleutel; de doel-stationsletter; Hiermee wordt aangegeven of het station moet worden geformatteerd; of het station moet worden versleuteld en zo ja, de BitLocker-sleutel en de logboekmap. Hier volgt de syntaxis voor een sessie van de eerste kopie naar een map of één bestand kopiëren:

 **Eerst kopiëren sessie naar één map kopiëren**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Eerst kopiëren sessie één bestand kopiëren**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 In latere kopie sessies hoeft u niet de eerste parameters opgeven. Hier volgt de syntaxis voor een sessie van de volgende kopiëren om een map of één bestand te kopiëren:

 **Daaropvolgende kopie-sessies door naar één map kopiëren**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Daaropvolgende kopie sessies één bestand kopiëren**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-the-first-copy-session-for-a-hard-drive"></a>Parameters voor de eerste kopieersessie voor een harde schijf
 Uitvoering van het Azure Import/Export-hulpprogramma om bestanden te kopiëren naar de harde schijf, maakt het hulpprogramma een kopieersessie. Elke sessie kopiëren kopieert één map of één bestand naar een harde schijf. De status van de sessie van het exemplaar wordt geschreven naar het journaalbestand. Als een kopieersessie wordt onderbroken (bijvoorbeeld als gevolg van een stroomstoring system), kan het worden hervat door het hulpprogramma opnieuw uit te voeren en het wijzigingslogboek-bestand op de opdrachtregel op te geven.

> [!WARNING]
>  Als u opgeeft de **/opmaken** parameter voor de eerste kopieersessie, de schijf wordt geformatteerd en alle gegevens op de schijf worden gewist. Het verdient aanbeveling dat u lege stations alleen voor uw sessie kopiëren gebruiken.

 De opdracht die is gebruikt voor de eerste kopieersessie voor elk station vereist verschillende parameters dan de opdrachten voor latere kopie-sessies. De volgende tabel bevat de extra parameters die beschikbaar voor de eerste kopieersessie zijn:

|Opdrachtregelparameter|Beschrijving|
|-----------------------------|-----------------|
|**/SK:**< StorageAccountKey\>|`Optional.`De opslagaccountsleutel voor het opslagaccount waarin de gegevens worden geïmporteerd. U moet ofwel opnemen **/sk:**< StorageAccountKey\> of **/csas:**< ContainerSas\> in de opdracht.|
|**/csas:**< ContainerSas\>|`Optional`. De container SAS gebruiken om gegevens te importeren naar het opslagaccount. U moet ofwel opnemen **/sk:**< StorageAccountKey\> of **/csas:**< ContainerSas\> in de opdracht.<br /><br /> De waarde voor deze parameter moet beginnen met de containernaam van de, gevolgd door een vraagteken (?) en de SAS-token. Bijvoorbeeld:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> De machtigingen of opgegeven op de URL of in een opgeslagen-beleid, moet bevatten lezen, schrijven en verwijderen voor taken van gegevensimport en lezen, schrijven en lijst voor exporteren.<br /><br /> Als deze parameter wordt opgegeven, worden alle blobs moet worden geïmporteerd of geëxporteerd moeten zich in de container die is opgegeven in de shared access signature.|
|**/ t:**< TargetDriveLetter\>|`Required.`De stationsletter van de harde schijf van het doel voor de huidige sessie kopiëren zonder de afsluitende dubbele punt.|
|**/ Format**|`Optional.`Geef deze parameter als het station worden geformatteerd moet. anders weglaten. Voordat u het hulpprogramma voor het station is geformatteerd, wordt u gevraagd om een bevestiging van de console. Als u wilt de bevestiging onderdrukken, geeft u de parameter /silentmode.|
|**/silentmode**|`Optional.`Deze parameter als u wilt onderdrukken van de bevestiging voor het formatteren van de schijf targert opgeven.|
|**/ versleutelen**|`Optional.`Deze parameter wordt opgegeven wanneer de schijf nog niet is versleuteld met BitLocker en moet worden gecodeerd door het hulpprogramma. Als het station al zijn versleuteld met BitLocker, laat u deze parameter weg en geef de `/bk` parameter, bieden de bestaande BitLocker-sleutel.<br /><br /> Als u opgeeft de `/format` parameter, dan hebt u moet ook opgeven de `/encrypt` parameter.|
|**/BK:**< BitLockerKey\>|`Optional.`Als `/encrypt` is opgegeven, laat u deze parameter. Als `/encrypt` wordt weggelaten, moet u beschikken over het station met BitLocker al hebt versleuteld. Gebruik deze parameter de BitLocker-sleutel op te geven. BitLocker-versleuteling is vereist voor alle harde schijven voor de taken van gegevensimport.|
|**schakeloptie/LOGDIR op:**< LogDirectory\>|`Optional.`De logboekmap geeft een map moet worden gebruikt voor het opslaan van uitgebreide Logboeken, evenals tijdelijke manifestbestanden. Als u niet opgeeft, wordt de huidige map gebruikt als de logboekmap.|

### <a name="parameters-required-for-all-copy-sessions"></a>Parameters die zijn vereist voor alle sessies van kopie
 Het logboek-bestand bevat de status voor alle kopie-sessies voor een harde schijf. Het bevat ook de benodigde informatie voor de import-taak maken. Bij het uitvoeren van de Azure-hulpprogramma voor importeren/exporteren, evenals een sessie-ID van het kopiëren, moet u altijd een journal-bestand opgeven:

|||
|-|-|
|Opdrachtregelparameter|Beschrijving|
|**/j:**< JournalFile\>|`Required.`Het pad naar het journaalbestand. Elk station moet precies één journaalbestand hebben. Houd er rekening mee dat het logboek-bestand niet op de doel-schijf moet zich bevinden. De extensie van het wijzigingslogboek is `.jrn`.|
|**/ID:**< sessie-id\>|`Required.`De sessie-ID identificeert een kopieersessie. Om ervoor te zorgen nauwkeurige herstel van een sessie onderbroken kopiëren wordt gebruikt. Bestanden die zijn gekopieerd in een kopieersessie worden opgeslagen in een map met de naam van de sessie-ID op de doel-schijf.|

### <a name="parameters-for-copying-a-single-directory"></a>Parameters voor het kopiëren van een enkele map
 Als één map kopieert, past u de volgende vereiste en optionele parameters:

|Opdrachtregelparameter|Beschrijving|
|----------------------------|-----------------|
|**/srcdir:**< SourceDirectory\>|`Required.`De bronmap die bestanden worden gekopieerd naar het doelstation bevat. Het mappad moet een absoluut pad (niet een relatief pad).|
|**/dstdir:**< DestinationBlobVirtualDirectory\>|`Required.`Het pad naar de virtuele map van de bestemming in uw Windows Azure-opslagaccount. De virtuele map kan of kan niet bestaat.<br /><br /> U kunt opgeven dat een container of een blob-voorvoegsel, zoals `music/70s/`. De doelmap moet beginnen met de containernaam van de, gevolgd door een slash (/), en voer kan desgewenst een blob virtuele map die eindigt op "/".<br /><br /> Wanneer de doelcontainer de container hoofdmap is, moet u expliciet de hoofdcontainer, waaronder de slash als opgeven `$root/`. Aangezien de blobs in de hoofdmap-container niet opnemen '/' in hun naam eventuele submappen in de bronmap die niet worden gekopieerd als de doelmap is een container hoofdmap.<br /><br /> Zorg ervoor dat namen van containers geldig gebruiken bij het opgeven van doel-virtuele mappen of blobs. Houd er rekening mee dat de namen van containers in kleine letters moeten zijn. Zie voor naamgevingsregels voor container, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Toestand:**< naam &#124; Nee overschrijven &#124; overschrijven >|`Optional.`Hiermee geeft u het gedrag wanneer een blob met opgegeven adres is al bestaat. Geldige waarden voor deze parameter zijn: `rename`, `no-overwrite` en `overwrite`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als geen waarde is opgegeven, is de standaardinstelling `rename`.<br /><br /> De opgegeven waarde voor deze parameter is van invloed op alle bestanden in de map die is opgegeven door de `/srcdir` parameter.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hiermee geeft u het blobtype voor de doel-blobs. Geldige waarden zijn: `BlockBlob` en `PageBlob`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als geen waarde is opgegeven, is de standaardinstelling `BlockBlob`.<br /><br /> In de meeste gevallen `BlockBlob` wordt aanbevolen. Als u opgeeft `PageBlob`, de lengte van elk bestand in de directory moet een veelvoud van 512 bytes, de grootte van een pagina voor pagina-blobs.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|
|**/ MetadataFile toe:**< MetadataFile toe\>|`Optional.`Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|

### <a name="parameters-for-copying-a-single-file"></a>Parameters voor het kopiëren van een enkel bestand
 Als één bestand worden gekopieerd, zijn de volgende vereiste en optionele parameters van toepassing:

|Opdrachtregelparameter|Beschrijving|
|----------------------------|-----------------|
|**/srcfile:**< bronbestand\>|`Required.`Het volledige pad naar het bestand moet worden gekopieerd. Het mappad moet een absoluut pad (niet een relatief pad).|
|**/dstblob:**< DestinationBlobPath\>|`Required.`Het pad naar de bestemmings-blob in uw Windows Azure-opslagaccount. De blob kan of kan niet bestaat.<br /><br /> Geef de blob naam die begint met de containernaam van de. De blob-naam mag niet beginnen met '/' of de naam van het opslagaccount. Zie voor blob-naamgevingsregels, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Wanneer de doelcontainer de container hoofdmap is, moet u expliciet opgeven `$root` als de container, zoals `$root/sample.txt`. Let op: blobs in de hoofdmap-container kan geen bevatten '/' in hun naam.|
|**/ Toestand:**< naam &#124; Nee overschrijven &#124; overschrijven >|`Optional.`Hiermee geeft u het gedrag wanneer een blob met opgegeven adres is al bestaat. Geldige waarden voor deze parameter zijn: `rename`, `no-overwrite` en `overwrite`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als geen waarde is opgegeven, is de standaardinstelling `rename`.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hiermee geeft u het blobtype voor de doel-blobs. Geldige waarden zijn: `BlockBlob` en `PageBlob`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als geen waarde is opgegeven, is de standaardinstelling `BlockBlob`.<br /><br /> In de meeste gevallen `BlockBlob` wordt aanbevolen. Als u opgeeft `PageBlob`, de lengte van elk bestand in de directory moet een veelvoud van 512 bytes, de grootte van een pagina voor pagina-blobs.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|
|**/ MetadataFile toe:**< MetadataFile toe\>|`Optional.`Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|

### <a name="resuming-an-interrupted-copy-session"></a>Een onderbroken kopieersessie te hervatten
 Als een kopieersessie wordt onderbroken om welke reden, kunt u deze hervatten door het hulpprogramma alleen journaal opgegeven bestand:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Alleen de meest recente kopie sessie, kan als is beëindigd, worden hervat.

> [!IMPORTANT]
>  Wanneer u een kopieersessie hervatten, wijzig niet de bronbestanden van de gegevens en mappen toe te voegen of te verwijderen van bestanden.

### <a name="aborting-an-interrupted-copy-session"></a>Een onderbroken kopieersessie wordt afgebroken
 Als een kopieersessie wordt onderbroken en het is niet mogelijk om te hervatten (bijvoorbeeld als een bronmap ontoegankelijk bewezen), moet u de huidige sessie afbreken zodat deze kan worden teruggedraaid en nieuwe kopie sessies kunnen worden gestart:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Alleen de laatste kopie-sessie, kan als is beëindigd, worden afgebroken. Houd er rekening mee dat u de eerste kopieersessie voor een station kan niet afbreken. In plaats daarvan moet u de kopieersessie met een nieuw journaalbestand opnieuw.

## <a name="next-steps"></a>Volgende stappen

* [Instellen van het hulpprogramma Azure Import/Export](storage-import-export-tool-setup-v1.md)
* [Eigenschappen en metagegevens instellen tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Naslaggids voor veelgebruikte opdrachten](storage-import-export-tool-quick-reference-v1.md) 
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)
* [Een importtaak herstellen](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Een exporttaak herstellen](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
