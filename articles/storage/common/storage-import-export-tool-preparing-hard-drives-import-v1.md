---
title: Harde schijven voorbereiden voor een importtaak Azure Import/Export - v1 | Microsoft Docs
description: Informatie over het voorbereiden van harde schijven met behulp van het WAImportExport v1-hulpprogramma voor het maken van een import-taak voor de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 03b504524b2f489f1ee042c6e825ccffe0a60bb3
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315059"
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Harde schijven voorbereiden voor een importtaak
Als u wilt een of meer harde schijven voorbereiden voor een importtaak, de volgende stappen uit:

- De gegevens te importeren in de Blob-service identificeren

- Doel-virtuele mappen en blobs in de Blob-service identificeren

- Het aantal schijven u moet, bepalen

- Kopieer de gegevens naar elk van uw harde schijven

  Zie voor een voorbeeldwerkstroom [voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-the-data-to-be-imported"></a>Bepalen welke gegevens moeten worden geïmporteerd
 De eerste stap bij het maken van een import-taak is om te bepalen welke mappen en bestanden die u wilt importeren. Dit kan een lijst met mappen, een lijst met unieke bestanden of een combinatie van deze twee zijn. Wanneer een directory opgenomen is, worden alle bestanden in de map en submappen deel uit van de import-taak.

> [!NOTE]
>  Sinds de submappen zijn opgenomen recursief wanneer een bovenliggende map opgenomen is, geeft u alleen de bovenliggende map. Geef geen op ook een van de bijbehorende submappen.
>
>  Op dit moment het Microsoft Azure Import/Export-hulpprogramma heeft de volgende beperking: als een map meer gegevens bevat dan een harde schijf kan bevatten, klikt u vervolgens de map moet worden opgesplitst in kleinere mappen. Bijvoorbeeld, als een map 2,5 TB aan gegevens bevat en de harde schijf alleen 2TB is, moet u de map 2,5 TB op te splitsen in kleinere mappen. Deze beperking wordt opgelost in een latere versie van het hulpprogramma.

## <a name="identify-the-destination-locations-in-the-blob-service"></a>Identificeren van de doellocaties in de blob-service
 Voor elke map of het bestand die worden geïmporteerd, moet u een virtuele doelmap of de blob in de Azure Blob-service identificeren. U gebruikt deze doelen als invoer voor de Azure Import/Export-hulpprogramma. Houd er rekening mee dat mappen moeten worden gescheiden met een slash-teken '/'.

 De volgende tabel ziet u enkele voorbeelden van blob-doelen:

|Bronbestand of map|Bestemmings-blob of virtuele map|
|------------------------------|-------------------------------------------|
|H:\Video|https:\//mystorageaccount.blob.core.windows.net/video|
|H:\Photo|https:\//mystorageaccount.blob.core.windows.net/photo|
|K:\Temp\FavoriteVideo.ISO|https:\//mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https:\//mystorageaccount.blob.core.windows.net/music|

## <a name="determine-how-many-drives-are-needed"></a>Bepalen hoeveel schijven nodig zijn
 Vervolgens moet u om te bepalen:

- Het aantal harde schijven die nodig zijn voor het opslaan van de gegevens.

- De mappen en/of zelfstandige-bestanden die moeten worden gekopieerd naar elk van de harde schijf.

  Zorg ervoor dat u hebt het aantal harde schijven u moet voor het opslaan van de gegevens die u overbrengt.

## <a name="copy-data-to-your-hard-drive"></a>Gegevens kopiëren naar de harde schijf
 Deze sectie wordt beschreven hoe u aan te roepen van het Azure Import/Export-hulpprogramma voor het kopiëren van uw gegevens voor een of meer harde schijven. Elke keer dat u het hulpprogramma Azure Import/Export kunt aanroepen, maakt u een nieuw *sessie kopiëren*. U maken ten minste één exemplaar sessie voor elk station is waarop u kopiëren van gegevens; in sommige gevallen moet u mogelijk meer dan één exemplaar sessie al uw gegevens naar één station kopiëren. Hier volgen enkele redenen dat u mogelijk meerdere sessies voor kopiëren:

-   U moet voor elke schijf die u naar kopiëren een sessie afzonderlijke kopie maken.

-   Een kopieersessie kunt één map of één blob kopiëren naar het station. Als u meerdere mappen, meerdere blobs of een combinatie van beide kopieert, moet u meerdere kopie-sessies maken.

-   U kunt de eigenschappen en metagegevens die worden ingesteld op de blobs die zijn geïmporteerd als onderdeel van een import-taak opgeven. De eigenschappen of metagegevens die u voor een sessie kopiëren opgeeft wordt toegepast op alle blobs die zijn opgegeven in die sessie kopiëren. Als u opgeven van verschillende eigenschappen en metagegevens voor sommige blobs wilt, moet u om de sessie van een afzonderlijk exemplaar te maken. Zie [Instellingseigenschappen en metagegevens tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import-v1.md)voor meer informatie.

> [!NOTE]
>  Als u meerdere machines die voldoen aan de vereisten die worden beschreven hebt in [instelling van de Azure Import/Export-hulpprogramma](storage-import-export-tool-setup-v1.md), kunt u gegevens kopiëren naar meerdere harde schijven parallel door een instantie van dit hulpprogramma wordt uitgevoerd op elke computer.

 Voor elke harde schijf die u met het hulpprogramma Azure Import/Export voorbereiden, maakt het hulpprogramma een één logboekbestand. U moet de logboekbestanden van al uw schijven te maken van de import-taak. Het logboekbestand kan ook worden gebruikt om te hervatten van de voorbereiding van station als het hulpprogramma wordt onderbroken.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>Syntaxis van de Azure Import/Export-hulpprogramma voor een importtaak
 Aanroepen voor het voorbereiden van schijven voor een importtaak, de Azure Import/Export-hulpprogramma met de **PrepImport** opdracht. Welke parameters u afhankelijk van of dit de eerste kopieersessie of een latere kopieersessie.

 De eerste kopieersessie voor een station is vereist voor sommige extra parameters om op te geven van de opslagaccountsleutel; de stationsletter van het doel. of het station moet worden geformatteerd. of het station moet worden versleuteld en zo ja, de BitLocker-sleutel; en de logboekmap. Hier volgt de syntaxis voor een eerste kopie-sessie om een map of één bestand te kopiëren:

 **Eerst kopiëren sessie naar één map kopiëren**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Eerst kopiëren-sessie met een enkel bestand kopiëren**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 In latere kopie-sessies hoeft u niet de aanvankelijke parameters opgeven. Hier volgt de syntaxis voor een sessie voor latere kopiëren om een map of één bestand te kopiëren:

 **Volgende kopie-sessies voor één map kopiëren**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Volgende kopiëren-sessies voor een enkel bestand kopiëren**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-the-first-copy-session-for-a-hard-drive"></a>Parameters voor de eerste kopieersessie voor een harde schijf
 Telkens wanneer u de Azure Import/Export-hulpprogramma voor het kopiëren van bestanden op de harde schijf uitvoert maakt het hulpprogramma een kopieersessie. Elke sessie kopiëren wordt één map of één bestand gekopieerd naar een harde schijf. De status van de sessie kopiëren wordt naar het logboekbestand geschreven. Als een sessie kopiëren wordt onderbroken (bijvoorbeeld, als gevolg van een stroomstoring system), kan deze worden hervat door het hulpprogramma opnieuw uit te voeren en het logboekbestand op de opdrachtregel op te geven.

> [!WARNING]
>  Als u opgeeft de **/opmaken** parameter voor de eerste kopieersessie, de schijf wordt geformatteerd en alle gegevens op de schijf worden gewist. Het verdient aanbeveling gebruik te maken van lege schijven alleen voor uw sessie kopiëren.

 De opdracht die wordt gebruikt voor de eerste kopieersessie voor elk station vereist verschillende parameters dan de opdrachten voor latere kopie-sessies. De volgende tabel bevat de extra parameters die beschikbaar voor de eerste kopieersessie zijn:

|Opdrachtregelparameter|Description|
|-----------------------------|-----------------|
|**/sk:**<StorageAccountKey\>|`Optional.` De opslagaccountsleutel voor het opslagaccount waarnaar de gegevens worden geïmporteerd. U moet een bevatten **/sk:**< StorageAccountKey\> of **/csas:**< ContainerSas\> in de opdracht.|
|**/csas:**<ContainerSas\>|`Optional`. De container SAS gebruiken om gegevens te importeren naar het opslagaccount. U moet een bevatten **/sk:**< StorageAccountKey\> of **/csas:**< ContainerSas\> in de opdracht.<br /><br /> De waarde voor deze parameter moet beginnen met de containernaam van de, gevolgd door een vraagteken (?) en de SAS-token. Bijvoorbeeld:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> De machtigingen of opgegeven op de URL of in een opgeslagen toegangsbeleid, lezen, moet bevatten schrijven en verwijderen voor de taken van gegevensimport, en lezen, schrijven en lijst voor export-taken.<br /><br /> Als deze parameter is opgegeven, worden alle blobs moeten worden geïmporteerd of geëxporteerd moeten zich binnen de container die in de shared access signature is opgegeven.|
|**/t:**<TargetDriveLetter\>|`Required.` De stationsletter van de harde schijf van het doel voor de huidige sessie kopiëren zonder de afsluitende dubbele punt.|
|**/ Format**|`Optional.` Deze parameter opgeeft als het station worden geformatteerd moet. anders achterwege. Voordat u het hulpprogramma voor het station is geformatteerd, wordt u gevraagd om een bevestiging van de console. Als u wilt de bevestiging onderdrukken, moet u voor de parameter /silentmode opgeven.|
|**/silentmode**|`Optional.` Geef deze parameter als u wilt onderdrukken van de bevestiging voor de opmaak van de doel-station.|
|**/ versleutelen**|`Optional.` Deze parameter opgegeven wanneer het station nog niet is versleuteld met BitLocker en moet worden versleuteld door het hulpprogramma. Als het station al is versleuteld met BitLocker, klikt u vervolgens deze parameter niet opgeeft en geeft u de `/bk` parameter, die de bestaande BitLocker-sleutel.<br /><br /> Als u opgeeft de `/format` parameter, dan hebt u moet ook opgeven de `/encrypt` parameter.|
|**/bk:**<BitLockerKey\>|`Optional.` Als `/encrypt` is opgegeven, wordt deze parameter niet opgeeft. Als `/encrypt` wordt weggelaten, moet u beschikken over hebt versleuteld al het station met BitLocker. Gebruik deze parameter om op te geven van de BitLocker-sleutel. BitLocker-versleuteling is vereist voor alle harde schijven voor de taken van gegevensimport.|
|**/logdir:**<LogDirectory\>|`Optional.` De logboekmap Hiermee geeft u een map moet worden gebruikt voor het opslaan van uitgebreide Logboeken, evenals tijdelijke manifestbestanden. Als niet is opgegeven, wordt de huidige map worden gebruikt als de logboekmap.|

### <a name="parameters-required-for-all-copy-sessions"></a>Vereiste parameters voor alle sessies van exemplaar
 Het logboekbestand bevat de status voor alle kopie-sessies voor een harde schijf. Het bevat ook de informatie die nodig zijn voor het maken van de import-taak. U moet altijd een logboekbestand opgeven bij het uitvoeren van het hulpprogramma Azure Import/Export, evenals een sessie-ID kopiëren:

|||
|-|-|
|Opdrachtregelparameter|Description|
|**/j:**< JournalFile\>|`Required.` Het pad naar het logboekbestand. Elk station moet exact één logboekbestand hebben. Houd er rekening mee dat het logboekbestand niet op de doel-schijf moet zich bevinden. De bestandsextensie van het logboek is `.jrn`.|
|**/id:**<SessionId\>|`Required.` De sessie-ID identificeert een kopieersessie. Het wordt gebruikt om ervoor te zorgen nauwkeurige herstel van een sessie onderbroken kopiëren. Bestanden die zijn gekopieerd in een sessie kopiëren worden opgeslagen in een map met de naam van de sessie-ID op de doel-station.|

### <a name="parameters-for-copying-a-single-directory"></a>Parameters voor het kopiëren van één map
 Als één map worden gekopieerd, zijn de volgende vereiste en optionele parameters van toepassing:

|Opdrachtregelparameter|Description|
|----------------------------|-----------------|
|**/srcdir:**<SourceDirectory\>|`Required.` De bronmap met bestanden die moeten worden gekopieerd naar de doel-station. Het mappad moet een absoluut pad (niet een relatief pad).|
|**/dstdir:**<DestinationBlobVirtualDirectory\>|`Required.` Het pad naar de virtuele map van de bestemming in uw Windows Azure storage-account. De virtuele map kan of mogelijk niet al bestaat.<br /><br /> U kunt een container opgeven of een blob-voorvoegsel, zoals `music/70s/`. De doelmap moet beginnen met de containernaam van de, gevolgd door een slash '/', en kan desgewenst een virtuele blob-map die eindigt op '/'.<br /><br /> Wanneer de doelcontainer het root-container is, moet u expliciet de root-container, met inbegrip van de gewone slash, als opgeven `$root/`. Sinds de blobs onder de container hoofdmap niet opnemen '/' in hun namen, worden eventuele submappen in de bronmap niet gekopieerd als de doelmap is een container voor hoofdmap.<br /><br /> Zorg ervoor dat geldige containernamen gebruiken bij het opgeven van doel-virtuele mappen of -blobs. Houd er rekening mee dat containernamen kleine letters moeten. Zie voor naamgevingsregels voor containers, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/Disposition:**<rename&#124;no-overwrite&#124;overwrite>|`Optional.` Hiermee geeft u het gedrag wanneer een blob met het opgegeven adres al bestaat. Geldige waarden voor deze parameter zijn: `rename`, `no-overwrite` en `overwrite`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als er geen waarde is opgegeven, is de standaardinstelling `rename`.<br /><br /> De opgegeven waarde voor deze parameter is van invloed op alle bestanden in de map die is opgegeven door de `/srcdir` parameter.|
|**/BlobType:**<BlockBlob&#124;PageBlob>|`Optional.` Hiermee geeft u het blobtype voor de doel-blobs. Geldige waarden zijn: `BlockBlob` en `PageBlob`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als er geen waarde is opgegeven, is de standaardinstelling `BlockBlob`.<br /><br /> In de meeste gevallen `BlockBlob` wordt aanbevolen. Als u opgeeft `PageBlob`, de lengte van elk bestand in de map moet een meervoud van 512 bytes, de grootte van een pagina voor pagina-blobs.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.` Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.` Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|

### <a name="parameters-for-copying-a-single-file"></a>Parameters voor het kopiëren van een enkel bestand
 Bij het kopiëren van een enkel bestand, zijn de volgende vereiste en optionele parameters van toepassing:

|Opdrachtregelparameter|Description|
|----------------------------|-----------------|
|**/srcfile:**< bronbestand\>|`Required.` Het volledige pad naar het bestand moet worden gekopieerd. Het mappad moet een absoluut pad (niet een relatief pad).|
|**/dstblob:**<DestinationBlobPath\>|`Required.` Het pad naar het bestemmings-blob in uw Windows Azure storage-account. De blob kan of mogelijk niet al bestaat.<br /><br /> Geef de blob naam die begint met de containernaam. De blob-naam mag niet beginnen met '/' of de naam van het opslagaccount. Zie voor blob-naamgevingsregels, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Wanneer de doelcontainer het root-container is, moet u expliciet opgeven `$root` als de container, zoals `$root/sample.txt`. Merk op dat onder het root-container-blobs niet opnemen '/' in hun namen.|
|**/Disposition:**<rename&#124;no-overwrite&#124;overwrite>|`Optional.` Hiermee geeft u het gedrag wanneer een blob met het opgegeven adres al bestaat. Geldige waarden voor deze parameter zijn: `rename`, `no-overwrite` en `overwrite`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als er geen waarde is opgegeven, is de standaardinstelling `rename`.|
|**/BlobType:**<BlockBlob&#124;PageBlob>|`Optional.` Hiermee geeft u het blobtype voor de doel-blobs. Geldige waarden zijn: `BlockBlob` en `PageBlob`. Houd er rekening mee dat deze waarden hoofdlettergevoelig zijn. Als er geen waarde is opgegeven, is de standaardinstelling `BlockBlob`.<br /><br /> In de meeste gevallen `BlockBlob` wordt aanbevolen. Als u opgeeft `PageBlob`, de lengte van elk bestand in de map moet een meervoud van 512 bytes, de grootte van een pagina voor pagina-blobs.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.` Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.` Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie.|

### <a name="resuming-an-interrupted-copy-session"></a>Een onderbroken kopieersessie te hervatten
 Als een sessie kopiëren wordt onderbroken voor een bepaalde reden, kunt u deze hervatten door te voeren van het hulpprogramma met alleen het opgegeven logboekbestand:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Alleen de meest recente kopie-sessie kan als is beëindigd, worden hervat.

> [!IMPORTANT]
>  Wanneer u een kopieersessie hervatten, mag niet worden gewijzigd de bronbestanden van de gegevens en mappen toevoegen of verwijderen van bestanden.

### <a name="aborting-an-interrupted-copy-session"></a>Een onderbroken kopieersessie wordt afgebroken
 Als een sessie kopiëren wordt onderbroken en het is niet mogelijk om te hervatten (bijvoorbeeld, als een bronmap niet toegankelijk bewezen), moet u de huidige sessie afbreken zodat het kan worden teruggezet en de nieuwe kopie sessies kunnen worden gestart:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Alleen de laatste kopie-sessie kan als is beëindigd, worden afgebroken. Houd er rekening mee dat u de eerste kopieersessie voor een station kan niet afbreken. In plaats daarvan moet u de kopieersessie opnieuw starten met een nieuw logboekbestand.

## <a name="next-steps"></a>Volgende stappen

* [Het hulpprogramma Azure Import/Export instellen](storage-import-export-tool-setup-v1.md)
* [Eigenschappen en metagegevens instellen tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Naslaggids voor veelgebruikte opdrachten](storage-import-export-tool-quick-reference-v1.md) 
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)
* [Een importtaak herstellen](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Een exporttaak herstellen](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
