---
title: Harde schijven voorbereiden voor een importtaak Azure Import/Export | Microsoft Docs
description: Informatie over het voorbereiden van harde schijven met behulp van het hulpprogramma WAImportExport te maken van een import-taak voor de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 7645694e9f2b90bfbe26ac3d0747791570f32d1b
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55510133"
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Harde schijven voorbereiden voor een importtaak

Het hulpprogramma WAImportExport is het station voorbereiding hulpprogramma en herstel die u met gebruiken kunt de [Microsoft Azure Import/Export-service](../storage-import-export-service.md). U kunt dit hulpprogramma gebruiken om gegevens te kopiëren naar de harde schijven die u wilt verzenden naar een Azure-datacenter. Nadat een import-taak is voltooid, kunt u dit hulpprogramma gebruiken om te herstellen van alle bestaande blobs die zijn beschadigd, ontbreekt of het conflict met andere blobs. Nadat u de schijven van een voltooide exporttaak ontvangt, kunt u dit hulpprogramma kunt gebruiken om te herstellen van bestanden die beschadigd of ontbreekt op de stations zijn. In dit artikel gaan we het gebruik van dit hulpprogramma.

## <a name="prerequisites"></a>Vereisten

### <a name="requirements-for-waimportexportexe"></a>Vereisten voor WAImportExport.exe

- **Machineconfiguratie**
  - Windows 7, Windows Server 2008 R2 of een nieuwere Windows-besturingssysteem
  - .NET framework 4 moet worden geïnstalleerd. Zie [Veelgestelde vragen over](#faq) over het controleren of .net Framework is geïnstalleerd op de computer.
- **Opslagaccountsleutel** -moet u ten minste één van de accountsleutels voor het opslagaccount.

### <a name="preparing-disk-for-import-job"></a>Voorbereiden van de schijf voor de import-taak

- **BitLocker -** BitLocker moet zijn ingeschakeld op de machine waarop het hulpprogramma WAImportExport wordt uitgevoerd. Zie de [Veelgestelde vragen over](#faq) voor informatie over het inschakelen van BitLocker.
- **Schijven** toegankelijk is vanaf de computer waarop WAImportExport hulpprogramma wordt uitgevoerd. Zie [Veelgestelde vragen over](#faq) voor schijf-specificatie.
- **De bronbestanden** -de bestanden die u van plan bent om te importeren moet toegankelijk zijn vanaf de computer kopiëren of ze zich op een netwerkshare of een lokale vaste schijf.

### <a name="repairing-a-partially-failed-import-job"></a>Een gedeeltelijk mislukt importtaak herstellen

- **Logboekbestand kopiëren** die wordt gegenereerd wanneer de Azure Import/Export-service worden gegevens gekopieerd tussen Storage-Account en de schijf. Deze bevindt zich in de doel-opslagaccount.

### <a name="repairing-a-partially-failed-export-job"></a>Een gedeeltelijk mislukt exporttaak herstellen

- **Logboekbestand kopiëren** die wordt gegenereerd wanneer de Azure Import/Export-service worden gegevens gekopieerd tussen Storage-Account en de schijf. Deze bevindt zich in uw bron-storage-account.
- **Manifestbestand** -[optioneel] Located op geëxporteerde station dat is geretourneerd door Microsoft.

## <a name="download-and-install-waimportexport"></a>Download en installeer WAImportExport

Download de [meest recente versie van WAImportExport.exe](https://www.microsoft.com/download/details.aspx?id=55280). Pak de gecomprimeerde inhoud naar een map op uw computer.

De volgende taak is het maken van CSV-bestanden.

## <a name="prepare-the-dataset-csv-file"></a>Het CSV-bestand van gegevensset voorbereiden

### <a name="what-is-dataset-csv"></a>Wat is gegevensset CSV

DataSet CSV-bestand is de waarde van de vlag /dataset een CSV-bestand met een lijst met mappen en/of een lijst met bestanden moeten worden gekopieerd naar de doel-stations. De eerste stap bij het maken van een import-taak is om te bepalen welke mappen en bestanden die u wilt importeren. Dit kan een lijst met mappen, een lijst met unieke bestanden of een combinatie van deze twee zijn. Wanneer een directory opgenomen is, worden alle bestanden in de map en submappen deel uit van de import-taak.

Voor elke map of bestand dat moet worden geïmporteerd, moet u een virtuele doelmap of de blob in de Azure Blob-service identificeren. U gebruikt deze doelen als invoer voor het hulpprogramma WAImportExport. Mappen moeten worden gescheiden met een slash-teken '/'.

De volgende tabel ziet u enkele voorbeelden van blob-doelen:

| Bronbestand of map | Bestemmings-blob of virtuele map |
| --- | --- |
| H:\Video | https://mystorageaccount.blob.core.windows.net/video |
| H:\Photo | https://mystorageaccount.blob.core.windows.net/photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.blob.core.windows.net/music |

### <a name="sample-datasetcsv"></a>Voorbeeld dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Velden van de DataSet CSV-bestanden

| Veld | Description |
| --- | --- |
| BasePath | **(Vereist)**<br/>De waarde van deze parameter geeft de bron waar de gegevens worden geïmporteerd. Het hulpprogramma wordt recursief kopiëren alle gegevens die zich onder dit pad.<br><br/>**Toegestane waarden**: Dit moet een geldig pad op de lokale computer of een geldig sharepad en toegankelijk is door de gebruiker moet zijn. Het mappad moet een absoluut pad (niet een relatief pad). Als het pad eindigt op "\\', een andere map staat voor een pad beëindigen zonder'\\' staat voor een bestand.<br/>Er is geen reguliere expressie is toegestaan in dit veld. Als het pad spaties bevat, plaatst u deze in ' ".<br><br/>**Voorbeeld**: "c:\Directory\c\Directory\File.txt"<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **(Vereist)**<br/> Het pad naar de virtuele map van de bestemming in uw Windows Azure storage-account. De virtuele map kan of mogelijk niet al bestaat. Als deze niet bestaat, wordt een Import/Export-service maken.<br/><br/>Zorg ervoor dat geldige containernamen gebruiken bij het opgeven van doel-virtuele mappen of -blobs. Houd er rekening mee dat containernamen kleine letters moeten. Zie voor naamgevingsregels voor containers, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Als er slechts hoofdmap is opgegeven, wordt de mapstructuur van de bron wordt gerepliceerd in de doel-blob-container. Als een andere directory-structuur dan de versie in de bron, meerdere rijen van de toewijzing van in CSV vereist is<br/><br/>Kunt u een container of een blob-voorvoegsel, zoals muziek/70s /. De doelmap moet beginnen met de containernaam van de, gevolgd door een slash '/', en kan desgewenst een virtuele blob-map die eindigt op '/'.<br/><br/>Wanneer de doelcontainer het root-container is, moet u expliciet de root-container, met inbegrip van de gewone slash, als $root opgeven /. Sinds de blobs onder de container hoofdmap niet opnemen '/' in hun namen, worden eventuele submappen in de bronmap niet gekopieerd als de doelmap is een container voor hoofdmap.<br/><br/>**Voorbeeld**<br/>Als het pad naar de bestemming is https://mystorageaccount.blob.core.windows.net/video, de waarde van dit veld mag video /  |
| BlobType | **[Optioneel]**  blok &#124; pagina<br/>Import/Export-service ondersteunt momenteel 2 soorten Blobs. Pagina-blobs en BlobsBy blokkeren standaard die alle bestanden worden geïmporteerd als blok-Blobs. En \*.vhd en \*.vhdx moet worden geïmporteerd zoals Page BlobsThere een limiet voor de blok-blob en pagina-blob toegestane grootte geldt. Zie [opslag schaalbaarheidsdoelen](storage-scalability-targets.md) voor meer informatie.  |
| Bestemming | **[Optioneel]**  naam &#124; niet overschrijven &#124; overschrijven <br/> Dit veld geeft de kopie-gedrag tijdens het importeren van Internet Explorer Wanneer gegevens worden geüpload naar het storage-account van de schijf. De beschikbare opties zijn: Wijzig de naam van&#124;overschrijven&#124;niet overschrijven. Standaard gebruikt als 'naam' als niets opgegeven. <br/><br/>**Wijzig de naam van**: Als een object met dezelfde naam aanwezig is, maakt een kopie in doel.<br/>Overschrijven: het bestand overschreven met nieuwere-bestand. Het bestand met wins laatst gewijzigd.<br/>**Geen overschrijven**: Slaat het schrijven naar het bestand als al aanwezig zijn.|
| MetadataFile | **[Optioneel]** <br/>De waarde in dit veld is het bestand met metagegevens die kan worden opgegeven als de moet de metagegevens van de objecten behoudt of aangepaste metagegevens bevatten. Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie |
| PropertiesFile | **[Optioneel]** <br/>Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service-metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>Voorbereiden van InitialDriveSet of AdditionalDriveSet CSV-bestand

### <a name="what-is-driveset-csv"></a>Wat is driveset CSV

De waarde van de vlag /InitialDriveSet of /AdditionalDriveSet is een CSV-bestand met de lijst met schijven waarop de stationsletters worden toegewezen, zodat het hulpprogramma kan de lijst met schijven worden voorbereid correct kiezen. Als de grootte van de groter is dan de grootte van een enkele schijf, wordt het hulpprogramma WAImportExport de gegevens verdelen over meerdere schijven die zijn aangemeld bij dit CSV-bestand op een geoptimaliseerde manier.

Er is geen limiet voor het aantal schijven dat de gegevens kunnen worden geschreven naar tijdens één sessie. Het hulpprogramma wilt distribueren op basis van grootte van de schijf en de grootte van gegevens. De schijf die is het meest wordt selecteert geoptimaliseerd voor de object-grootte. De gegevens wanneer geüpload naar het opslagaccount dat wordt terug naar de directorystructuur die is opgegeven in de gegevensset-bestand worden geconvergeerd. Als u wilt maken van een driveset CSV, de volgende stappen uit te voeren.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Maak eenvoudige volume en stationsletter toewijzen

Om te maken van een standaardvolume en een stationsletter toewijzen door de instructies voor 'eenvoudiger maken van een partitie"gegeven op [overzicht van Schijfbeheer](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Voorbeeld InitialDriveSet en AdditionalDriveSet CSV-bestand

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Velden van de Driveset CSV-bestanden

| Velden | Value |
| --- | --- |
| DriveLetter | **(Vereist)**<br/> Elke schijf die wordt geleverd om het hulpprogramma omdat de bestemming moet een eenvoudige NTFS-volume op deze en toegewezen stationsaanduiding is toegekend.<br/> <br/>**Voorbeeld**: R of r |
| FormatOption | **[Required]** Format &#124; AlreadyFormatted<br/><br/> **Indeling**: Op te geven deze, zal de gegevens op de schijf formatteren. <br/>**AlreadyFormatted**: Het hulpprogramma wordt overgeslagen opmaak wanneer deze waarde is opgegeven. |
| SilentOrPromptOnFormat | **(Vereist)**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: Deze waarde op te geven, kunnen gebruikers het hulpprogramma uitvoeren in stille modus. <br/>**PromptOnFormat**: Het hulpprogramma wordt de gebruiker om te bevestigen of de actie echt in elke indeling bedoeld is gevraagd.<br/><br/>Als niet is ingesteld, opdracht wordt afgebroken en foutbericht weergegeven: ' Ongeldige waarde voor SilentOrPromptOnFormat: geen ' |
| Versleuteling | **[Required]** Encrypt &#124; AlreadyEncrypted<br/> De waarde van dit veld besluit welke schijf om te coderen en die niet op. <br/><br/>**versleutelen**: Hulpprogramma zal het station te formatteren. Als de waarde van veld 'FormatOption' is 'Vorm' is vervolgens deze waarde vereist 'Versleutelen' zijn. Als "AlreadyEncrypted" in dit geval is opgegeven, resultaat het een fout opgetreden 'Wanneer indeling is opgegeven, versleutelen moet ook worden opgegeven'.<br/>**AlreadyEncrypted**: Hulpprogramma wordt het station met de opgegeven in veld 'ExistingBitLockerKey' BitLockerKey ontsleutelen. Als waarde van het veld "FormatOption" "AlreadyFormatted" is, klikt u vervolgens deze waarde kan zijn 'Versleutelen' of "AlreadyEncrypted" |
| ExistingBitLockerKey | **(Vereist)**  Als de waarde van veld 'Codering' is "AlreadyEncrypted"<br/> De waarde van dit veld is de BitLocker-sleutel die is gekoppeld aan de specifieke schijf. <br/><br/>Dit veld moet leeg zijn als de waarde van veld 'Codering' is 'Versleutelen' blijven.  Als BitLocker Key in dit geval is opgegeven, resultaat het een fout opgetreden 'BitLocker Key mag niet worden opgegeven'.<br/>  **Voorbeeld**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Voorbereiden van de schijf voor de import-taak

Aanroepen voor het voorbereiden van schijven voor een importtaak, het hulpprogramma WAImportExport met de **PrepImport** opdracht. Welke parameters u afhankelijk van of dit de eerste kopieersessie of een latere kopieersessie.

### <a name="first-session"></a>Eerste sessie

Eerste sessie kopiëren naar een map Single/Multiple kopiëren naar een enkel/meerdere schijf (afhankelijk van wat is opgegeven in CSV-bestand) WAImportExport hulpprogramma PrepImport-opdracht voor de eerste kopieersessie om te kopiëren van mappen en/of bestanden met een nieuwe kopieersessie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] /DataSet:<dataset.csv>
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Gegevens toevoegen in de volgende sessie

In latere kopie-sessies hoeft u niet de aanvankelijke parameters opgeven. U moet de dezelfde logboekbestand in volgorde voor het hulpprogramma gebruiken om te onthouden waar deze een links in de vorige sessie. De status van de sessie kopiëren wordt naar het logboekbestand geschreven. Hier volgt de syntaxis voor een sessie voor latere kopiëren om extra mappen en/of bestanden te kopiëren:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-to-latest-session"></a>Stations toevoegen aan de meest recente sessie

Als de gegevens niet in de opgegeven stations in InitialDriveset past komt, kunt een het hulpprogramma extra schijven toevoegen aan dezelfde kopieersessie. 

>[!NOTE] 
>De sessie-id moet overeenkomen met de vorige sessie-id. Logboekbestand moet overeenkomen met de versie die is opgegeven in de vorige sessie.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-the-latest-session"></a>De meest recente sessie afgebroken:

Als een sessie kopiëren wordt onderbroken en het is niet mogelijk om te hervatten (bijvoorbeeld, als een bronmap niet toegankelijk bewezen), moet u de huidige sessie afbreken zodat het kan worden teruggezet en de nieuwe kopie sessies kunnen worden gestart:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Alleen de laatste kopie-sessie kan als is beëindigd, worden afgebroken. Houd er rekening mee dat u de eerste kopieersessie voor een station kan niet afbreken. In plaats daarvan moet u de kopieersessie opnieuw starten met een nieuw logboekbestand.

### <a name="resume-a-latest-interrupted-session"></a>Een laatste onderbroken sessie hervatten

Als een sessie kopiëren wordt onderbroken voor een bepaalde reden, kunt u deze hervatten door te voeren van het hulpprogramma met alleen het opgegeven logboekbestand:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> Wanneer u een kopieersessie hervatten, mag niet worden gewijzigd de bronbestanden van de gegevens en mappen toevoegen of verwijderen van bestanden.

## <a name="waimportexport-parameters"></a>WAImportExport parameters

| Parameters | Description |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Vereist**<br/> Pad naar het logboekbestand. Een logboekbestand houdt een set schijven en de voortgang van de records in deze schijven voorbereiden. Het logboekbestand moet altijd worden opgegeven.  |
|     schakeloptie/LOGDIR op:&lt;LogDirectory&gt;  | **Optioneel**. De logboekmap.<br/> Uitgebreide logboekbestanden, evenals een aantal tijdelijke bestanden naar deze map geschreven. Als dat niet wordt opgegeven, de huidige directory gebruikt als de logboekmap. De logboekmap kan slechts één keer worden opgegeven voor de dezelfde logboekbestand.  |
|     /ID:&lt;sessie-id&gt;  | **Vereist**<br/> De sessie-Id voor het identificeren van een sessie kopiëren wordt gebruikt. Het wordt gebruikt om ervoor te zorgen nauwkeurige herstel van een sessie onderbroken kopiëren.  |
|     / ResumeSession  | Optioneel. Als de laatste kopieersessie is beëindigd, kan deze parameter worden opgegeven om te hervatten van de sessie.   |
|     / AbortSession  | Optioneel. Als de laatste kopieersessie is beëindigd, kan deze parameter worden opgegeven om af te breken de sessie.  |
|     /sn:&lt;StorageAccountName&gt;  | **Vereist**<br/> Alleen van toepassing op RepairImport en RepairExport. De naam van het storage-account.  |
|     /sk:&lt;StorageAccountKey&gt;  | **Vereist**<br/> De sleutel van het opslagaccount. |
|     /InitialDriveSet:&lt;driveset.csv&gt;  | **Vereiste** bij het uitvoeren van de eerste kopieersessie<br/> Een CSV-bestand met een lijst met schijven om voor te bereiden.  |
|     /AdditionalDriveSet:&lt;driveset.csv&gt; | **Vereiste**. Wanneer het toevoegen van schijven aan de huidige kopieersessie. <br/> Een CSV-bestand met een lijst van extra schijven moeten worden toegevoegd.  |
|      r:&lt;RepairFile&gt; | **Vereiste** alleen van toepassing op RepairImport en RepairExport.<br/> Pad naar het bestand voor het bijhouden van herstel wordt uitgevoerd. Elk station moet slechts één herstel-bestand hebben.  |
|     /d:&lt;TargetDirectories&gt; | **Vereiste**. Alleen van toepassing op RepairImport en RepairExport. Voor RepairImport, een of meer mappen voor gescheiden door puntkomma's om te herstellen; Voor RepairExport, één map te herstellen, bijvoorbeeld de hoofdmap van het station.  |
|     /CopyLogFile:&lt;DriveCopyLogFile&gt; | **Vereiste** alleen van toepassing op RepairImport en RepairExport. Pad naar het logboekbestand van de schijf kopiëren (uitgebreide of fout).  |
|     /ManifestFile:&lt;DriveManifestFile&gt; | **Vereiste** alleen van toepassing op RepairExport.<br/> Pad naar het manifestbestand van het station.  |
|     /PathMapFile:&lt;DrivePathMapFile&gt; | **Optioneel**. Alleen van toepassing op RepairImport.<br/> Pad naar het bestand met toewijzingen van paden ten opzichte van de hoofdmap van station locaties van de werkelijke bestanden (door tabs gescheiden). Als eerste wordt opgegeven, wordt met bestandspaden met lege doelen worden ingevuld wat betekent dat ze niet zijn gevonden in TargetDirectories, de toegang is geweigerd, met een ongeldige naam, of ze bestaan in meerdere mappen. Het toewijzingsbestand pad kan handmatig worden bewerkt om op te nemen van de juiste doelpaden en opnieuw worden opgegeven voor de bestandspaden correct worden omgezet met het hulpprogramma.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Vereiste**. Alleen van toepassing op PreviewExport.<br/> Pad naar het XML-bestand opslaan met lijst met blob-paden of blob-voorvoegsels voor pad voor de blobs worden geëxporteerd. De bestandsindeling is hetzelfde als de blob lijst met blob-indeling in de taak Put-bewerking van de Import/Export-service REST-API.  |
|     / DriveSize:&lt;DriveSize&gt; | **Vereiste**. Alleen van toepassing op PreviewExport.<br/>  Grootte van schijven om te worden gebruikt voor het exporteren. Bijvoorbeeld: 500 GB, 1,5 TB. Opmerking: 1 GB = 1.000.000.000 bytes1 TB = 1,000,000,000,000 bytes  |
|     /DataSet:&lt;dataset.csv&gt; | **Vereist**<br/> Een CSV-bestand met een lijst met mappen en/of een lijst met bestanden moeten worden gekopieerd naar de doel-stations.  |
|     /silentmode  | **Optioneel**.<br/> Als dat niet is opgegeven, wordt herinneren dat u de vereiste van schijven en moet je bevestigen om door te gaan.  |

## <a name="tool-output"></a>De uitvoer van hulpprogramma

### <a name="sample-drive-manifest-file"></a>Voorbeeld van station manifestbestand

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DriveManifest Version="2011-MM-DD">
   <Drive>
      <DriveId>drive-id</DriveId>
      <StorageAccountKey>storage-account-key</StorageAccountKey>
      <ClientCreator>client-creator</ClientCreator>
      <!-- First Blob List -->
      <BlobList Id="session#1-0">
         <!-- Global properties and metadata that applies to all blobs -->
         <MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>
         <PropertiesPath Hash="md5-hash">global-properties-file-path</PropertiesPath>
         <!-- First Blob -->
         <Blob>
            <BlobPath>blob-path-relative-to-account</BlobPath>
            <FilePath>file-path-relative-to-transfer-disk</FilePath>
            <ClientData>client-data</ClientData>
            <Length>content-length</Length>
            <ImportDisposition>import-disposition</ImportDisposition>
            <!-- page-range-list-or-block-list -->
            <!-- page-range-list -->
            <PageRangeList>
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
            </PageRangeList>
            <!-- block-list -->
            <BlockList>
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
            </BlockList>
            <MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>
            <PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>
         </Blob>
      </BlobList>
   </Drive>
</DriveManifest>
```

### <a name="sample-journal-file-xml-for-each-drive"></a>Voorbeeld-logboekbestand (XML) voor elk station

```xml
[BeginUpdateRecord][2016/11/01 21:22:25.379][Type:ActivityRecord]
ActivityId: DriveInfo
DriveState: [BeginValue]
<?xml version="1.0" encoding="UTF-8"?>
<Drive>
   <DriveId>drive-id</DriveId>
   <BitLockerKey>*******</BitLockerKey>
   <ManifestFile>\DriveManifest.xml</ManifestFile>
   <ManifestHash>D863FE44F861AE0DA4DCEAEEFFCCCE68</ManifestHash> </Drive>
[EndValue]
SaveCommandOutput: Completed
[EndUpdateRecord]
```

### <a name="sample-journal-file-jrn-for-session-that-records-the-trail-of-sessions"></a>Voorbeeld-logboekbestand (JRN) voor de sessie die u de audittrail van sessies registreert

```
[BeginUpdateRecord][2016/11/02 18:24:14.735][Type:NewJournalFile]
VocabularyVersion: 2013-02-01
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.749][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
LogDirectory: F:\logs
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.754][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
StorageAccountKey: *******
[EndUpdateRecord]
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

#### <a name="what-is-waimportexport-tool"></a>Wat is een hulpprogramma WAImportExport?

Het hulpprogramma WAImportExport is het station voorbereiding hulpprogramma en herstel die u met de Microsoft Azure Import/Export-service gebruiken kunt. U kunt dit hulpprogramma gebruiken om gegevens te kopiëren naar de harde schijven die u wilt verzenden naar een Azure-Datacenter. Nadat een import-taak is voltooid, kunt u dit hulpprogramma gebruiken om te herstellen van alle bestaande blobs die zijn beschadigd, ontbreekt of het conflict met andere blobs. Nadat u de schijven van een voltooide exporttaak ontvangt, kunt u dit hulpprogramma kunt gebruiken om te herstellen van bestanden die beschadigd of ontbreekt op de stations zijn.

#### <a name="how-does-the-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Hoe werkt het hulpprogramma WAImportExport op meerdere bron dir en schijven?

Als de gegevens groter dan de grootte van de schijf is, wordt het hulpprogramma WAImportExport de gegevens verdelen over de schijven in een geoptimaliseerde manier. Het kopiëren van gegevens naar meerdere schijven kan worden gedaan parallel of sequentieel worden verwerkt. Er is geen limiet voor het aantal schijven dat de gegevens kunnen worden geschreven naar tegelijkertijd. Het hulpprogramma wilt distribueren op basis van grootte van de schijf en de grootte van gegevens. De schijf die is het meest wordt selecteert geoptimaliseerd voor de object-grootte. De gegevens wanneer geüpload naar het opslagaccount dat wordt terug naar de opgegeven directory-structuur worden geconvergeerd.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>Waar vind ik de vorige versie van WAImportExport hulpprogramma?

WAImportExport hulpprogramma heeft alle functies waarop WAImportExport V1-hulpprogramma. WAImportExport hulpprogramma kan gebruikers meerdere bronnen opgeven en schrijven naar meerdere schijven. Bovendien kan een eenvoudig meerdere bronlocaties van waaruit de gegevens moeten worden gekopieerd in één CSV-bestand beheren. Als u ondersteuning voor SAS- of wilt kopiëren van één bron naar één schijf nodig hebt, u kunt echter [WAImportExport V1 Tool downloaden](https://go.microsoft.com/fwlink/?LinkID=301900&amp;clcid=0x409) en verwijzen naar [WAImportExport V1 verwijzing](storage-import-export-tool-how-to-v1.md) voor hulp bij het gebruik van WAImportExport V1 .

#### <a name="what-is-a-session-id"></a>Wat is er een sessie-ID?

Het hulpprogramma wordt verwacht dat de sessie kopiëren (/ id) parameter moet dezelfde zijn als het doel is om de gegevens verdeeld over meerdere schijven. Onderhouden van dezelfde naam van de sessie kopiëren, kunnen gebruikers gegevens uit een of meerdere bronlocaties in een of meerdere schijven/doelmappen te kopiëren. Onderhoud van dezelfde sessie-id, kunt het hulpprogramma om op te halen de kopie van bestanden vanaf waar deze het laatst is gegeven.

Echter kan niet dezelfde kopieersessie worden gebruikt voor het importeren van gegevens naar verschillende opslagaccounts.

Wanneer de naam van de kopie-sessie is hetzelfde voor verschillende runs van het programma, het logboekbestand (/ logdir) en storage-accountsleutel (/ sk) ook naar verwachting hetzelfde zijn.

Sessie-id kan bestaan uit letters, 0 ~ 9, onderstrepingsteken (\_), streepjes (-) of hash (#), en de lengte moet 3 ~ 30.

bijvoorbeeld sessie-1 of #1-sessie of sessie\_1

#### <a name="what-is-a-journal-file"></a>Wat is een logboekbestand?

Telkens wanneer u het hulpprogramma WAImportExport als u wilt kopiëren van bestanden op de harde schijf uitvoert maakt het hulpprogramma een kopieersessie. De status van de sessie kopiëren wordt naar het logboekbestand geschreven. Als een sessie kopiëren wordt onderbroken (bijvoorbeeld, als gevolg van een stroomstoring system), kan deze worden hervat door het hulpprogramma opnieuw uit te voeren en het logboekbestand op de opdrachtregel op te geven.

Voor elke harde schijf die u met het hulpprogramma Azure Import/Export voorbereiden, is het hulpprogramma wordt een enkel logboekbestand te maken met de naam '&lt;station&gt;.xml "waarbij station Id is van het serienummer dat is gekoppeld aan het station dat het hulpprogramma wordt gelezen van de schijf. U moet de logboekbestanden van al uw schijven te maken van de import-taak. Het logboekbestand kan ook worden gebruikt om te hervatten van de voorbereiding van station als het hulpprogramma wordt onderbroken.

#### <a name="what-is-a-log-directory"></a>Wat is een logboekmap?

De logboekmap Hiermee geeft u een map moet worden gebruikt voor het opslaan van uitgebreide Logboeken, evenals tijdelijke manifestbestanden. Als niet is opgegeven, wordt de huidige map worden gebruikt als de logboekmap. De logboeken zijn uitgebreide Logboeken.

### <a name="prerequisites"></a>Vereisten

#### <a name="what-are-the-specifications-of-my-disk"></a>Wat zijn de specificaties van mijn schijf?

Een of meer lege 2,5-inch of 3,5-inch SATA of III of SSD harde schijven die zijn verbonden met de machine kopiëren.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>Hoe kan ik BitLocker inschakelen op mijn computer?

Er is een eenvoudige manier om te controleren met de rechtermuisknop op het systeemstation. Hierin ziet u opties voor BitLocker als de mogelijkheid is ingeschakeld. Als deze uitgeschakeld is, kunt u deze niet weergegeven.

![BitLocker controleren](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Hier volgt een artikel van [BitLocker inschakelen](https://technet.microsoft.com/library/cc766295.aspx)

Het is mogelijk dat uw computer geen TPM-chip. Als u niet met behulp van tpm.msc uitvoer wordt weergegeven, bekijkt u de volgende veelgestelde vragen.

#### <a name="how-to-disable-trusted-platform-module-tpm-in-bitlocker"></a>Het uitschakelen van Trusted Platform Module (TPM) in BitLocker?
> [!NOTE]
> Alleen als er geen TPM op hun servers, moet u TPM-beleid uitschakelen. Het is niet nodig zijn om uit te schakelen TPM als er een vertrouwde TPM in de server van gebruiker. 
> 

TPM in BitLocker uitschakelen, gaat u door de volgende stappen:<br/>
1. Start **Groepsbeleidsobjecteditor** door gpedit.msc in een opdrachtprompt te typen. Als **Groepsbeleidsobjecteditor** is niet beschikbaar voor het eerst inschakelen van BitLocker. Zie de vorige veelgestelde vragen.
2. Open **lokaal computerbeleid &gt; Computerconfiguratie &gt; Beheersjablonen &gt; Windows-onderdelen&gt; BitLocker-stationsversleuteling &gt; besturingssysteem stations**.
3. Bewerken **aanvullende verificatie bij opstarten vereisen** beleid.
4. Het beleid ingesteld op **ingeschakeld** en zorg ervoor dat **BitLocker toestaan zonder een compatibele TPM** is ingeschakeld.

####  <a name="how-to-check-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Hoe Controleer of .NET 4 of hoger is geïnstalleerd op mijn computer?

Alle versies van Microsoft .NET Framework zijn geïnstalleerd in de volgende map: %windir%\Microsoft.NET\Framework\

Navigeer naar de hierboven genoemde onderdeel op de doelcomputer waarop het hulpprogramma moet worden uitgevoerd. Zoek naar mapnaam beginnen met 'v4'. Afwezigheid van dergelijk een directory betekent dat.NET 4 is niet geïnstalleerd op uw computer. U kunt .net 4 downloaden op uw machine met [Microsoft .NET Framework 4 (webinstallatie)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Limieten

#### <a name="how-many-drives-can-i-preparesend-at-the-same-time"></a>Het aantal stations kan ik voorbereiden/verzenden op hetzelfde moment?

Er is geen limiet voor het aantal schijven dat het hulpprogramma kan worden voorbereid. Het hulpprogramma wordt echter stationsletters als invoer verwacht. Die beperkt de activering tot 25 gelijktijdige schijfvoorbereiding. Een enkele taak kan maximaal 10 schijven tegelijk verwerken. Als u meer dan 10 schijven die zijn gericht op hetzelfde opslagaccount hebt voorbereid, kunnen de schijven worden verdeeld over meerdere taken.

#### <a name="can-i-target-more-than-one-storage-account"></a>Kan ik verwijst naar meer dan één opslagaccount?

Slechts één opslagaccount kan worden verzonden per taak en in de sessie voor één exemplaar.

### <a name="capabilities"></a>Functionaliteit

#### <a name="does-waimportexportexe-encrypt-my-data"></a>Wordt versleuteld WAImportExport.exe mijn gegevens?

Ja. BitLocker-versleuteling is ingeschakeld en is vereist voor dit proces.

#### <a name="what-will-be-the-hierarchy-of-my-data-when-it-appears-in-the-storage-account"></a>Wat is de hiërarchie van mijn gegevens wanneer deze wordt weergegeven in de storage-account?

Hoewel gegevens wordt verdeeld over schijven, worden de gegevens wanneer geüpload naar het opslagaccount dat is geconvergeerd terug naar de directory-structuur die is opgegeven in de gegevensset CSV-bestand.

#### <a name="how-many-of-the-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Het aantal van de invoer schijven heeft actieve i/o parallel, wanneer het exemplaar wordt uitgevoerd?

Het hulpprogramma verdeeld gegevens over de invoer schijven op basis van de grootte van de invoerbestanden. Dat gezegd, het aantal actieve schijven parallel volledig afhankelijk van de aard van de invoergegevens zijn. Afhankelijk van de grootte van afzonderlijke bestanden in de invoergegevensset, kunnen een of meer schijven gelijktijdig actieve i/o tonen. Zie de volgende vraag voor meer informatie.

#### <a name="how-does-the-tool-distribute-the-files-across-the-disks"></a>Hoe het hulpprogramma de bestanden verdelen over de schijven

WAImportExport hulpprogramma leest en schrijft bestanden per partij, één batch max van 100000 bestanden bevat. Dit betekent dat maximale 100000 bestanden gelijktijdig kunnen worden geschreven. Meerdere schijven worden tegelijkertijd naar geschreven als deze 100000 bestanden worden gedistribueerd naar meerdere schijven. Wel of u het hulpprogramma schrijft naar meerdere schijven tegelijkertijd, of één schijf, is afhankelijk van de cumulatieve grootte van de batch. Bijvoorbeeld, in het geval van kleinere bestanden schrijft als alle 10,0000 bestanden kunt in één station, past hulpprogramma naar slechts één schijf tijdens de verwerking van deze batch.

### <a name="waimportexport-output"></a>WAImportExport uitvoer

#### <a name="there-are-two-journal-files-which-one-should-i-upload-to-azure-portal"></a>Er zijn twee logboekbestanden, welke moet ik upload naar Azure portal?

**.XML** -voor elke harde schijf die u met het hulpprogramma WAImportExport voorbereidt, wordt het hulpprogramma een één logboekbestand maken met de naam `<DriveID>.xml` daar van het serienummer dat is gekoppeld aan het station dat het hulpprogramma wordt gelezen van de schijf in station. U moet de logboekbestanden van al uw schijven te maken van de import-taak in Azure portal. Dit logboekbestand kan ook worden gebruikt om te hervatten van de voorbereiding van station als het hulpprogramma wordt onderbroken.

**jrn** -het logboekbestand met achtervoegsel `.jrn` bevat de status voor alle kopie-sessies voor een harde schijf. Het bevat ook de informatie die nodig zijn voor het maken van de import-taak. U moet altijd een logboekbestand opgeven bij het uitvoeren van het hulpprogramma WAImportExport, evenals een kopie sessie-ID.

## <a name="next-steps"></a>Volgende stappen

* [Het hulpprogramma Azure Import/Export instellen](storage-import-export-tool-setup.md)
* [Eigenschappen en metagegevens instellen tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import.md)
* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Naslaggids voor veelgebruikte opdrachten](storage-import-export-tool-quick-reference.md) 
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)
* [Een importtaak herstellen](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Een exporttaak herstellen](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
