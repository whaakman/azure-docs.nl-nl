---
title: Harde schijven voor de import-taak van een Azure Import/Export voorbereiden | Microsoft Docs
description: Informatie over het voorbereiden van harde schijven met het hulpmiddel WAImportExport te maken van een import-taak voor de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: 2854822907e818297c8d2f74cab48b0afa0d646c
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Harde schijven voorbereiden voor een Import-taak

Het hulpprogramma WAImportExport is het station voorbereiding hulpprogramma en herstel die u met gebruiken kunt de [Microsoft Azure Import/Export-service](../storage-import-export-service.md). U kunt dit hulpprogramma gebruiken om gegevens te kopiëren naar de harde schijven die u wilt verzenden naar een Azure-datacenter. Nadat een import-taak is voltooid, kunt u dit hulpprogramma kunt gebruiken om te herstellen van alle bestaande blobs zijn beschadigd, ontbreekt of het conflict met andere blobs. Nadat u de stations van een voltooide exporttaak ontvangt, kunt u dit hulpprogramma kunt gebruiken om te herstellen van bestanden die beschadigd of ontbreken op de stations zijn. In dit artikel gaan we via het gebruik van dit hulpprogramma.

## <a name="prerequisites"></a>Vereisten

### <a name="requirements-for-waimportexportexe"></a>Vereisten voor WAImportExport.exe

- **Machineconfiguratie**
  - Windows 7, Windows Server 2008 R2 of een nieuwere Windows-besturingssysteem
  - .NET framework 4 moet worden geïnstalleerd. Zie [Veelgestelde vragen over](#faq) over het controleren of .net Framework is geïnstalleerd op de computer.
- **Opslagaccountsleutel** -u moet ten minste één van de sleutels voor het opslagaccount.

### <a name="preparing-disk-for-import-job"></a>Voorbereiden van de schijf voor import-taak

- **BitLocker -** BitLocker moet zijn ingeschakeld op de computer waarop het hulpprogramma WAImportExport. Zie de [Veelgestelde vragen over](#faq) voor het inschakelen van BitLocker.
- **Schijven** toegankelijk is vanaf de computer waarop WAImportExport hulpprogramma wordt uitgevoerd. Zie [Veelgestelde vragen over](#faq) voor schijf-specificatie.
- **Bronbestanden** -de bestanden die u van plan bent om te importeren moet toegankelijk zijn vanaf de computer kopiëren of ze zich op een netwerkshare of een lokale vaste schijf.

### <a name="repairing-a-partially-failed-import-job"></a>Herstellen van een gedeeltelijk mislukt import-taak

- **Logboekbestand kopiëren** die wordt gegenereerd wanneer het Azure Import/Export-service gegevens tussen de Storage-Account en de schijf kopiëren. Bevindt het zich in uw doelopslagaccount.

### <a name="repairing-a-partially-failed-export-job"></a>Herstellen van een taak export gedeeltelijk is mislukt

- **Logboekbestand kopiëren** die wordt gegenereerd wanneer het Azure Import/Export-service gegevens tussen de Storage-Account en de schijf kopiëren. Bevindt het zich in uw opslagaccount bron.
- **Manifestbestand** -Located [optioneel] op de geëxporteerde schijf die is geretourneerd door Microsoft.

## <a name="download-and-install-waimportexport"></a>Download en installeer WAImportExport

Download de [meest recente versie van WAImportExport.exe](https://www.microsoft.com/download/details.aspx?id=55280). Pak de gecomprimeerde inhoud naar een map op uw computer.

Uw volgende taak is het maken van CSV-bestanden.

## <a name="prepare-the-dataset-csv-file"></a>Voorbereiden van het CSV-bestand van gegevensset

### <a name="what-is-dataset-csv"></a>Wat is gegevensset CSV

DataSet CSV-bestand is de waarde van de vlag /dataset is een CSV-bestand dat bevat een lijst met mappen en/of een lijst met bestanden worden gekopieerd naar de doel-stations. De eerste stap bij het maken van een import-taak is om te bepalen welke mappen en bestanden die u gaat importeren. Dit kan een lijst met mappen, een lijst met unieke bestanden of een combinatie van deze twee zijn. Wanneer een directory opgenomen is, worden alle bestanden in de map en de bijbehorende submappen deel uit van de import-taak.

Voor elke map of bestand dat moet worden geïmporteerd, moet u een doel-virtuele map of een blob in de Azure Blob-service identificeren. U gebruikt deze doelen als invoer voor het hulpprogramma WAImportExport. Mappen moeten worden gescheiden met een slash-teken '/'.

De volgende tabel ziet u enkele voorbeelden van blob-doelen:

| Bronbestand of map | Bestemmings-blob of virtuele map |
| --- | --- |
| H:\Video | https://mystorageaccount.BLOB.Core.Windows.NET/video |
| H:\Photo | https://mystorageaccount.BLOB.Core.Windows.NET/Photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.BLOB.Core.Windows.NET/Music |

### <a name="sample-datasetcsv"></a>Voorbeeld dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>DataSet CSV-bestandsvelden

| Veld | Beschrijving |
| --- | --- |
| Het basispad naar | **[Vereist]**<br/>De waarde van deze parameter geeft de bron waarbij de gegevens moeten worden geïmporteerd. Het hulpprogramma wordt recursief kopiëren alle gegevens die zich onder dit pad.<br><br/>**Toegestane waarden**: dit moet een geldig pad op de lokale computer of een geldig sharepad en toegankelijk is door de gebruiker moet zijn. Het mappad moet een absoluut pad (niet een relatief pad). Als het pad eindigt op "\\", deze een andere map vertegenwoordigt een einddatum zonder pad"\\' een bestand vertegenwoordigt.<br/>Er is geen Regex-waarde is toegestaan in dit veld. Als het pad spaties bevat, moet deze ' '.<br><br/>**Voorbeeld**: 'c:\Directory\c\Directory\File.txt'<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Vereist]**<br/> Het pad naar de virtuele map van de bestemming in uw Windows Azure-opslagaccount. De virtuele map kan of kan niet bestaat. Als deze niet bestaat nog, wordt een Import/Export-service maken.<br/><br/>Zorg ervoor dat namen van containers geldig gebruiken bij het opgeven van doel-virtuele mappen of blobs. Houd er rekening mee dat de namen van containers in kleine letters moeten zijn. Zie voor naamgevingsregels voor container, [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Als alleen hoofdmap is opgegeven, wordt de directorystructuur van de bron wordt gerepliceerd in de doel-blob-container. Als u de structuur van een andere map dan de versie in de bron, meerdere rijen van CSV-toewijzing<br/><br/>U kunt een container of een blob-voorvoegsel zoals muziek/70s opgeven /. De doelmap moet beginnen met de containernaam van de, gevolgd door een slash (/), en voer kan desgewenst een blob virtuele map die eindigt op "/".<br/><br/>Wanneer de doelcontainer de container hoofdmap is, moet u expliciet de basiscontainer, inclusief de slash, als $root opgeven /. Aangezien de blobs in de hoofdmap-container niet opnemen '/' in hun naam eventuele submappen in de bronmap die niet worden gekopieerd als de doelmap is een container hoofdmap.<br/><br/>**Voorbeeld**<br/>Als het doelpad van de blob https://mystorageaccount.blob.core.windows.net/video is, de waarde van dit veld mag video /  |
| BlobType | **[Optioneel]**  blok &#124; pagina<br/>Import/Export-service ondersteunt momenteel 2 soorten Blobs. Pagina-blobs en het blok BlobsBy standaard die alle bestanden worden geïmporteerd als blok-Blobs. En \*VHD en \*.vhdx moet worden geïmporteerd zoals Page BlobsThere een limiet op de blok-blobs en pagina-blob toegestane grootte geldt. Zie [schaalbaarheidsdoelen van opslag](storage-scalability-targets.md) voor meer informatie.  |
| Toestand | **[Optioneel]**  Wijzig de naam van &#124; Nee overschrijven &#124; overschrijven <br/> In dit veld wordt de kopie-gedrag tijdens het importeren van Internet Explorer Wanneer gegevens worden er geüpload naar de storage-account van de schijf. Beschikbare opties zijn: Wijzig de naam van &#124; wilt &#124; Nee-overschrijven. Standaard gebruikt als 'Wijzig de naam' als niets wordt opgegeven. <br/><br/>**Wijzig de naam van**: als een object met dezelfde naam aanwezig is, maakt u een kopie in het doel.<br/>Overschrijven: het bestand overschreven met nieuwere-bestand. Het bestand met wins laatst is gewijzigd.<br/>**Er is geen overschrijven**: slaat het schrijven van het bestand als deze al aanwezig.|
| MetadataFile toe | **[Optioneel]** <br/>De waarde voor dit veld is de metagegevensbestand dat kan worden opgegeven als de moet de metagegevens van de objecten behoudt of aangepaste metagegevens bieden. Pad naar het bestand met metagegevens voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie |
| PropertiesFile | **[Optioneel]** <br/>Pad naar het eigenschappenbestand voor de doel-blobs. Zie [Import/Export-service metagegevens en eigenschappen bestandsindeling](../storage-import-export-file-format-metadata-and-properties.md) voor meer informatie. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>Bereid InitialDriveSet of AdditionalDriveSet CSV-bestand

### <a name="what-is-driveset-csv"></a>Wat is driveset CSV

De waarde van de vlag /InitialDriveSet of /AdditionalDriveSet is een CSV-bestand met de lijst met schijven waarop de stationsletters zijn toegewezen, zodat het hulpprogramma kan correct kiezen voor de lijst met schijven worden voorbereid. Als de gegevens groter dan de grootte van een enkele schijf is, wordt het hulpprogramma WAImportExport de gegevens verdelen over meerdere schijven die zijn aangemeld bij dit CSV-bestand op een geoptimaliseerde manier.

Er is geen limiet voor het aantal schijven die kan de gegevens worden geschreven in één sessie. Het hulpprogramma wordt gegevens op basis van de schijfgrootte en Mapgrootte distribueren. De schijf die het meest selecteert geoptimaliseerd voor de grootte van het object. De gegevens wanneer geüpload naar het opslagaccount wordt worden geconvergeerd terug naar de directory-structuur die is opgegeven in de dataset-bestand. Volg de onderstaande stappen om te kunnen maken van een driveset CSV.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Standaardvolume maken en stationsletter toewijzen

Om te kunnen maken van een standaardvolume en een stationsletter toewijzen door de instructies voor het 'Eenvoudiger partitie maken' gegeven op [overzicht van Schijfbeheer](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Voorbeeld InitialDriveSet en AdditionalDriveSet CSV-bestand

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Velden voor Driveset CSV-bestand

| Velden | Waarde |
| --- | --- |
| Stationsletter | **[Vereist]**<br/> Elke schijf die wordt geleverd om het hulpprogramma omdat het doel moet een eenvoudige NTFS-volume op het bestand en een stationsletter toegewezen hebben.<br/> <br/>**Voorbeeld**: R of r |
| FormatOption | **[Vereist]**  Indeling &#124; AlreadyFormatted<br/><br/> **Indeling**: geven deze alle gegevens op de schijf wordt geformatteerd. <br/>**AlreadyFormatted**: het hulpprogramma wordt overgeslagen opmaak wanneer deze waarde wordt opgegeven. |
| SilentOrPromptOnFormat | **[Vereist]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: gebruiker kan het hulpprogramma uitvoeren in stille modus opgeven van deze waarde wordt ingeschakeld. <br/>**PromptOnFormat**: het hulpprogramma wordt de gebruiker om te bevestigen of de actie echt elke indeling is bedoeld.<br/><br/>Als niet is ingesteld, opdracht wordt afgebroken en foutbericht weergegeven: ' onjuiste waarde voor SilentOrPromptOnFormat: geen ' |
| Versleuteling | **[Vereist]**  Versleutelen &#124; AlreadyEncrypted<br/> De waarde van dit veld besluit welke schijf voor het versleutelen en die niet op. <br/><br/>**Versleutelen**: hulpprogramma voor de schijf wordt geformatteerd. Als de waarde van veld 'FormatOption' is 'Indeling' is deze waarde moet 'Coderen' vereist. Als 'AlreadyEncrypted' in dit geval wordt opgegeven, resulteert dit in een fout 'Wanneer de opgegeven indeling versleutelen moet ook worden opgegeven'.<br/>**AlreadyEncrypted**: hulpprogramma voor het station met de opgegeven in het veld 'ExistingBitLockerKey' BitLockerKey worden gedecodeerd. Als de waarde van veld "FormatOption" "AlreadyFormatted", klikt u vervolgens deze waarde kan zijn 'Coderen' of 'AlreadyEncrypted' |
| ExistingBitLockerKey | **[Vereist]**  Als de waarde van veld 'Versleuteling' is 'AlreadyEncrypted'<br/> De waarde van dit veld is de BitLocker-sleutel die is gekoppeld aan een bepaalde schijf. <br/><br/>Dit veld mag leeg als de waarde van veld 'Versleuteling' 'Coderen'.  Als BitLocker Key in dit geval wordt opgegeven, hierdoor een fout opgetreden 'Bitlocker Key mag niet worden opgegeven'.<br/>  **Voorbeeld**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Voorbereiden van de schijf voor import-taak

Aanroepen om voor te bereiden stations voor een import-taak, het hulpprogramma WAImportExport met de **PrepImport** opdracht. Welke parameters die u wilt opnemen, hangt af van of dit de eerste kopieersessie of een latere kopieersessie.

### <a name="first-session"></a>Eerste sessie

Eerste sessie kopiëren naar een map Single/Multiple kopiëren naar een single/meerdere schijf (afhankelijk van wat is opgegeven in CSV-bestand) WAImportExport hulpprogramma PrepImport opdracht voor de eerste kopieersessie voor het kopiëren van mappen en/of bestanden met een nieuwe kopieersessie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Gegevens toevoegen in de volgende sessie

In latere kopie sessies hoeft u niet de eerste parameters opgeven. U moet hetzelfde journaalbestand om het hulpprogramma gebruiken om te onthouden waar deze in de vorige sessie was. De status van de sessie van het exemplaar wordt geschreven naar het journaalbestand. Hier volgt de syntaxis voor een sessie van de volgende kopiëren om extra mappen en/of bestanden te kopiëren:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-to-latest-session"></a>Schijven toevoegen aan de meest recente sessie

Als de gegevens niet in de opgegeven stations in InitialDriveset past komt, kan een het hulpprogramma extra stations toevoegen aan dezelfde kopieersessie gebruiken. 

>[!NOTE] 
>De sessie-id moet overeenkomen met de vorige sessie-id. Journal-bestand moet overeenkomen met de versie opgegeven in de vorige sessie.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-the-latest-session"></a>De meest recente sessie wordt afgebroken:

Als een kopieersessie wordt onderbroken en het is niet mogelijk om te hervatten (bijvoorbeeld als een bronmap ontoegankelijk bewezen), moet u de huidige sessie afbreken zodat deze kan worden teruggedraaid en nieuwe kopie sessies kunnen worden gestart:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Alleen de laatste kopie-sessie, kan als is beëindigd, worden afgebroken. Houd er rekening mee dat u de eerste kopieersessie voor een station kan niet afbreken. In plaats daarvan moet u de kopieersessie met een nieuw journaalbestand opnieuw.

### <a name="resume-a-latest-interrupted-session"></a>Een recente onderbroken sessie hervatten

Als een kopieersessie wordt onderbroken om welke reden, kunt u deze hervatten door het hulpprogramma alleen journaal opgegeven bestand:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Voorbeeld:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> Wanneer u een kopieersessie hervatten, wijzig niet de bronbestanden van de gegevens en mappen toe te voegen of te verwijderen van bestanden.

## <a name="waimportexport-parameters"></a>WAImportExport parameters

| Parameters | Beschrijving |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Vereist**<br/> Pad naar het journaalbestand. Een journal-bestand houdt een set van schijven en registreert de voortgang bij het voorbereiden van deze stations. Het journaalbestand moet altijd worden opgegeven.  |
|     schakeloptie/LOGDIR op:&lt;LogDirectory&gt;  | **Optioneel**. De logboekmap.<br/> Uitgebreide logboekbestanden, evenals een aantal tijdelijke bestanden naar deze map geschreven. Als dat niet wordt opgegeven, de huidige directory gebruikt als de logboekmap. De logboekmap kan slechts één keer worden opgegeven voor hetzelfde journaalbestand.  |
|     /ID:&lt;sessie-id&gt;  | **Vereist**<br/> De sessie-Id wordt gebruikt voor het identificeren van een kopieersessie. Om ervoor te zorgen nauwkeurige herstel van een sessie onderbroken kopiëren wordt gebruikt.  |
|     / ResumeSession  | Optioneel. Als de laatste kopieersessie is beëindigd, kunt u deze parameter opgegeven hervatten van de sessie.   |
|     / AbortSession  | Optioneel. Als de laatste kopieersessie is beëindigd, kan deze parameter worden opgegeven om af te breken van de sessie.  |
|     /sn:&lt;StorageAccountName&gt;  | **Vereist**<br/> Alleen van toepassing voor RepairImport en RepairExport. De naam van het opslagaccount.  |
|     /SK:&lt;StorageAccountKey&gt;  | **Vereist**<br/> De sleutel van het opslagaccount. |
|     / InitialDriveSet:&lt;driveset.csv&gt;  | **Vereist** bij het uitvoeren van de eerste kopieersessie<br/> Een CSV-bestand dat bevat een lijst met stations om voor te bereiden.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Vereist**. Wanneer het toevoegen van schijven aan de huidige kopieersessie. <br/> Een CSV-bestand met een lijst van extra stations worden toegevoegd.  |
|      / r:&lt;RepairFile&gt; | **Vereist** alleen van toepassing op RepairImport en RepairExport.<br/> Pad naar het bestand voor het traceren van herstel uitgevoerd. Elk station moet slechts één repair-bestand hebben.  |
|     / d:&lt;TargetDirectories&gt; | **Vereist**. Alleen van toepassing voor RepairImport en RepairExport. Voor RepairImport, een of meer door puntkomma's gescheiden mappen te herstellen; Voor RepairExport, één map te herstellen, bijvoorbeeld de hoofdmap van het station.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Vereist** alleen van toepassing op RepairImport en RepairExport. Pad naar het station kopiëren-logboekbestand (uitgebreid of fout).  |
|     / ManifestFile:&lt;DriveManifestFile&gt; | **Vereist** alleen van toepassing op RepairExport.<br/> Pad naar het station manifestbestand.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **Optioneel**. Alleen van toepassing op RepairImport.<br/> Pad naar het bestand met toewijzingen van paden ten opzichte van de hoofdmap van station naar de locaties van bestanden (door tabs gescheiden). Als eerste wordt opgegeven, wordt met bestandspaden leeg doelen worden ingevuld wat betekent dat ze niet zijn gevonden in TargetDirectories, de toegang is geweigerd, met een ongeldige naam of aanwezig zijn in meerdere mappen. Het toewijzingsbestand pad kan handmatig worden bewerkt zodanig dat de juiste doelpaden en opnieuw worden opgegeven voor het hulpprogramma de bestandspaden correct omzetten.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Vereist**. Alleen van toepassing op PreviewExport.<br/> Pad naar het XML-bestand met lijst met blob-paden bestand of blob-pad voorvoegsels voor de blobs worden geëxporteerd. De bestandsindeling is hetzelfde als de indeling van de blob-blob in de taak Put-bewerking van de Import/Export-service REST-API.  |
|     / DriveSize:&lt;DriveSize&gt; | **Vereist**. Alleen van toepassing op PreviewExport.<br/>  Grootte van stations moet worden gebruikt voor het exporteren. Bijvoorbeeld, 500 GB, 1,5 TB. Opmerking: 1 GB = 1.000.000.000 bytes1 TB = 1,000,000,000,000 bytes  |
|     / Gegevensset:&lt;dataset.csv&gt; | **Vereist**<br/> Een CSV-bestand dat bevat een lijst met mappen en/of een lijst met bestanden worden gekopieerd naar de doel-stations.  |
|     /silentmode  | **Optioneel**.<br/> Als niet wordt opgegeven, wordt de herinneren dat u de vereiste van schijven en moet uw bevestiging om door te gaan.  |

## <a name="tool-output"></a>Hulpprogramma uitvoer

### <a name="sample-drive-manifest-file"></a>Voorbeeld station-manifestbestand

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

### <a name="sample-journal-file-xml-for-each-drive"></a>Voorbeeld journaal-bestand (XML) voor elk station

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

### <a name="sample-journal-file-jrn-for-session-that-records-the-trail-of-sessions"></a>Wijzigingslogboek voorbeeldbestand (JRN) voor de sessie die de audittrail van sessies registreert

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

#### <a name="what-is-waimportexport-tool"></a>Wat is WAImportExport tool?

Het hulpprogramma WAImportExport is het station voorbereiding hulpprogramma en herstel die u met de Microsoft Azure Import/Export-service gebruiken kunt. U kunt dit hulpprogramma gebruiken om gegevens te kopiëren naar de harde schijven die u wilt verzenden naar een Azure-Datacenter. Nadat een import-taak is voltooid, kunt u dit hulpprogramma kunt gebruiken om te herstellen van alle bestaande blobs zijn beschadigd, ontbreekt of het conflict met andere blobs. Nadat u de stations van een voltooide exporttaak ontvangt, kunt u dit hulpprogramma kunt gebruiken om te herstellen van bestanden die beschadigd of ontbreken op de stations zijn.

#### <a name="how-does-the-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Hoe werkt het hulpprogramma WAImportExport op meerdere bron dir en schijven?

Als de gegevens groter dan de grootte van de schijf is, wordt het hulpprogramma WAImportExport de gegevens verdelen over de schijven op een geoptimaliseerde manier. Kopiëren van gegevens naar meerdere schijven kan worden gedaan parallel of sequentieel worden verwerkt. Er is geen limiet voor het aantal schijven dat de gegevens kunnen gelijktijdig naar worden geschreven. Het hulpprogramma wordt gegevens op basis van de schijfgrootte en Mapgrootte distribueren. De schijf die het meest selecteert geoptimaliseerd voor de grootte van het object. De gegevens wanneer geüpload naar het opslagaccount wordt worden geconvergeerd terug naar de opgegeven directory-structuur.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>Waar vind ik een vorige versie van WAImportExport hulpprogramma?

WAImportExport hulpprogramma heeft alle functionaliteit die WAImportExport V1 hulpprogramma had. WAImportExport hulpprogramma kan gebruikers meerdere bronnen opgeven en schrijven naar meerdere schijven. Bovendien kan een eenvoudig meerdere bronlocaties van waaruit de gegevens moeten worden gekopieerd in één CSV-bestand beheren. In geval moet u echter wilt kopiëren van één bron op één schijf, u kunt [hulpprogramma downloaden als WAImportExport V1] of SAS-ondersteuning (http://go.microsoft.com/fwlink/?LinkID=301900&amp;clcid = 0x409) en verwijzen naar [WAImportExport V1 verwijzing](storage-import-export-tool-how-to-v1.md) voor hulp bij het gebruik van WAImportExport V1.

#### <a name="what-is-a-session-id"></a>Wat is er een sessie-ID?

Het hulpprogramma verwacht dat de kopieersessie (/ id) parameter moet dezelfde zijn als de bedoeling is om de gegevens verdeeld over meerdere schijven. Onderhoud van dezelfde naam van de kopieersessie toe te voegen gebruiker gegevens gekopieerd van een of meerdere bronlocaties in een of meerdere bestemming schijven/mappen. Onderhoud van dezelfde sessie-id, kunt het hulpprogramma voor het kopiëren van bestanden van waar deze het laatst is gegeven kunnen worden opgepikt.

Echter kan niet dezelfde kopieersessie worden gebruikt voor het importeren van gegevens naar verschillende opslagaccounts.

Wanneer kopiëren sessienaam is hetzelfde voor meerdere uitvoert het hulpprogramma voor het logboekbestand (/ logdir) en de opslagaccountsleutel (/ sk) ook naar verwachting hetzelfde zijn.

Sessie-id kan bestaan uit letters, 0 ~ 9 understore (\_), streepjes (-) of hash (#), en de lengte moet 3 ~ 30.

bijvoorbeeld sessie-1 of sessie nummer 1 of sessie\_1

#### <a name="what-is-a-journal-file"></a>Wat is er een journal-bestand?

Telkens wanneer u het hulpprogramma WAImportExport om bestanden te kopiëren naar de harde schijf uitvoert maakt het hulpprogramma een kopieersessie. De status van de sessie van het exemplaar wordt geschreven naar het journaalbestand. Als een kopieersessie wordt onderbroken (bijvoorbeeld als gevolg van een stroomstoring system), kan het worden hervat door het hulpprogramma opnieuw uit te voeren en het wijzigingslogboek-bestand op de opdrachtregel op te geven.

Voor elke harde schijf die u met het hulpprogramma voor Azure Import/Export voorbereiden, het hulpprogramma een enkele journal-bestand wordt gemaakt met de naam '&lt;station&gt;.xml ' waarbij station Id is van het serienummer dat is gekoppeld aan het station dat het hulpprogramma van de schijf leest. U moet de journaal-bestanden van alle stations de import-taak maken. Het logboek-bestand kan ook worden gebruikt om te hervatten voorbereiding van het station als het hulpprogramma wordt onderbroken.

#### <a name="what-is-a-log-directory"></a>Wat is er een logboekmap?

De logboekmap geeft een map moet worden gebruikt voor het opslaan van uitgebreide Logboeken, evenals tijdelijke manifestbestanden. Als u niet opgeeft, wordt de huidige map gebruikt als de logboekmap. De logboeken zijn uitgebreide Logboeken.

### <a name="prerequisites"></a>Vereisten

#### <a name="what-are-the-specifications-of-my-disk"></a>Wat zijn de specificaties van mijn schijf?

Een of meer leeg 2,5-inch of 3.5-inch SATA of III of SSD harde schijven gekoppeld aan de machine kopiëren.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>Hoe kan ik BitLocker inschakelen op mijn computer?

Er is een eenvoudige manier om te controleren door met de rechtermuisknop op het systeemstation. Hierin ziet u opties voor Bitlocker als de mogelijkheid is ingeschakeld. Als deze uitgeschakeld is, kunt u het niet zien.

![Controle van BitLocker](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Hier volgt een artikel op [BitLocker inschakelen](https://technet.microsoft.com/library/cc766295.aspx)

Het is mogelijk dat de computer geen TPM-chip. Als u niet met behulp van tpm.msc uitvoer krijgt, bekijkt u de volgende veelgestelde vragen.

#### <a name="how-to-disable-trusted-platform-module-tpm-in-bitlocker"></a>Het uitschakelen van de Trusted Platform Module (TPM) in BitLocker?
> [!NOTE]
> Alleen als er geen TPM in hun servers, moet u TPM-beleid uitschakelen. Het is niet nodig zijn voor de TPM uitschakelen als er een vertrouwde TPM op de server van de gebruiker. 
> 

Om de TPM in BitLocker uitschakelen, gaat u de volgende stappen:<br/>
1. Start **Groepsbeleidsobjecteditor** door gpedit.msc in te voeren op een opdrachtprompt. Als **Groepsbeleidsobjecteditor** lijkt te zijn niet beschikbaar voor het eerst inschakelen van BitLocker. Zie de vorige veelgestelde vragen.
2. Open **lokaal computerbeleid &gt; Computerconfiguratie &gt; Beheersjablonen &gt; Windows-onderdelen&gt; BitLocker-stationsversleuteling &gt; besturingssysteem stations**.
3. Bewerken **aanvullende verificatie bij opstarten vereisen** beleid.
4. Het beleid instellen op **ingeschakeld** en zorg ervoor dat **BitLocker toestaan zonder een compatibele TPM** is ingeschakeld.

####  <a name="how-to-check-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Hoe Controleer of .NET 4 of hoger is geïnstalleerd op mijn computer?

Alle versies van Microsoft .NET Framework zijn geïnstalleerd in de volgende map: %windir%\Microsoft.NET\Framework\

Navigeer naar de bovengenoemde onderdeel op de doelcomputer waarop het hulpprogramma moet worden uitgevoerd. Zoek naar de mapnaam die begint met 'v4'. Afwezigheid van dergelijk directory betekent dat.NET 4 is niet geïnstalleerd op uw computer. U kunt .net 4 downloaden op uw machine met [Microsoft .NET Framework 4 (webinstallatie)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Limieten

#### <a name="how-many-drives-can-i-preparesend-at-the-same-time"></a>Het aantal stations kan ik voorbereiden/verzenden op hetzelfde moment?

Er is geen limiet voor het aantal schijven dat het hulpprogramma kan worden voorbereid. Het hulpprogramma wordt echter stationsletters verwacht als invoer. Die beperkt de activering tot 25 gelijktijdige schijfvoorbereiding. Een enkele taak kan maximaal 10 schijven tegelijk verwerken. Als u meer dan 10 schijven die gericht is op hetzelfde opslagaccount voorbereidt, kunnen de schijven worden verdeeld over meerdere taken.

#### <a name="can-i-target-more-than-one-storage-account"></a>Kan ik meer dan één opslagaccount richten?

Slechts één opslagaccount kan worden verzonden per taak en in één exemplaar sessie.

### <a name="capabilities"></a>Functionaliteit

#### <a name="does-waimportexportexe-encrypt-my-data"></a>WAImportExport.exe mijn gegevens versleutelen?

Ja. BitLocker-versleuteling is ingeschakeld en is vereist voor dit proces.

#### <a name="what-will-be-the-hierarchy-of-my-data-when-it-appears-in-the-storage-account"></a>Wat is de hiërarchie van mijn gegevens wanneer deze wordt weergegeven in de storage-account?

Hoewel de gegevens worden gedistribueerd naar meerdere schijven, wordt de gegevens wanneer naar het opslagaccount geüpload geconvergeerd terug naar de opgegeven in het CSV-bestand van gegevensset mappenstructuur.

#### <a name="how-many-of-the-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Hoeveel van de invoer schijven hebben active IO parallel wanneer kopie uitgevoerd wordt?

Het hulpprogramma verdeelt gegevens over de invoer schijven op basis van de grootte van de invoerbestanden. Die gezegd, het aantal actieve schijven parallel delends volledig op de aard van de invoergegevens zijn. Afhankelijk van de grootte van afzonderlijke bestanden in de invoergegevensset kunnen een of meer schijven active IO parallel tonen. Zie de volgende vraag voor meer informatie.

#### <a name="how-does-the-tool-distribute-the-files-across-the-disks"></a>Hoe het hulpprogramma de bestanden verdelen over de schijven

WAImportExport hulpprogramma leest en schrijft bestanden per partij, één batch maximaal 100000 bestanden bevat. Dit betekent dat maximale 100000 bestanden parallel kunnen worden geschreven. Meerdere schijven zijn gelijktijdig naar geschreven als deze 100000 bestanden worden gedistribueerd naar meerdere schijven. Evenwel of het hulpprogramma schrijft naar meerdere schijven tegelijkertijd of één schijf is afhankelijk van de totale grootte van de batch. Bijvoorbeeld, in geval van een kleinere bestanden schrijft als alle 10,0000 bestanden kunt in één station, past hulpprogramma naar slechts één schijf tijdens het verwerken van deze batch.

### <a name="waimportexport-output"></a>WAImportExport uitvoer

#### <a name="there-are-two-journal-files-which-one-should-i-upload-to-azure-portal"></a>Er zijn twee journaal-bestanden die ik uploaden naar Azure-portal?

**.XML** -voor elke harde schijf die u met het hulpprogramma WAImportExport voorbereiden, het hulpprogramma een enkele journal-bestand wordt gemaakt met de naam `<DriveID>.xml` waarbij station is het serienummer dat is gekoppeld aan het station dat het hulpprogramma van de schijf leest. U moet de journaal-bestanden van alle stations de import-taak maken in de Azure-portal. Dit logboek-bestand kan ook worden gebruikt om te hervatten voorbereiding van het station als het hulpprogramma wordt onderbroken.

**.jrn** -journal-bestand met achtervoegsel `.jrn` bevat de status voor alle kopie-sessies voor een harde schijf. Het bevat ook de benodigde informatie voor de import-taak maken. U moet altijd een journal-bestand opgeven bij het uitvoeren van het hulpprogramma WAImportExport, evenals een kopie-sessie-ID.

## <a name="next-steps"></a>Volgende stappen

* [Instellen van het hulpprogramma Azure Import/Export](storage-import-export-tool-setup.md)
* [Eigenschappen en metagegevens instellen tijdens het importproces](storage-import-export-tool-setting-properties-metadata-import.md)
* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Naslaggids voor veelgebruikte opdrachten](storage-import-export-tool-quick-reference.md) 
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)
* [Een importtaak herstellen](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Een exporttaak herstellen](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
