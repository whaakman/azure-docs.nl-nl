---
title: Azure Import/Export manifestbestand indeling | Microsoft Docs
description: Meer informatie over de indeling van het manifestbestand van station die de toewijzing tussen blobs in Azure Blob storage en bestanden op een station in een importeren beschrijft of exporteren van de taak in de Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f3119e1c-2c25-48ad-8752-a6ed4adadbb0
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c1857eb94fba13c30e7f07669616f5d0ab9953f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure Import/Export-service manifest bestandsindeling
Het manifestbestand station beschrijft de toewijzing tussen blobs in Azure Blob storage en bestanden op schijf, bestaande uit een taak worden geïmporteerd of geëxporteerd. Het manifestbestand is gemaakt als onderdeel van het voorbereidingsproces station en wordt opgeslagen op de schijf voordat het station wordt verzonden naar de Azure-Datacenter voor een bewerking importeren. Tijdens een exportbewerking, het manifest gemaakt en opgeslagen op het station door de Azure Import/Export-service.  
  
Voor beide importeren en exporteren van taken, wordt het manifestbestand van de schijf opgeslagen op de schijf worden geïmporteerd of geëxporteerd; het is niet verzonden naar de service via een API-bewerking.  
  
Hier volgen de algemene indeling van een manifestbestand station:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveManifest Version="2014-11-01">  
  <Drive>  
    <DriveId>drive-id</DriveId>  
    import-export-credential  
  
    <!-- First Blob List -->  
    <BlobList>  
      <!-- Global properties and metadata that applies to all blobs -->  
      [<MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>]  
      [<PropertiesPath   
        Hash="md5-hash">global-properties-file-path</PropertiesPath>]  
  
      <!-- First Blob -->  
      <Blob>  
        <BlobPath>blob-path-relative-to-account</BlobPath>  
        <FilePath>file-path-relative-to-transfer-disk</FilePath>  
        [<ClientData>client-data</ClientData>]  
        [<Snapshot>snapshot</Snapshot>]  
        <Length>content-length</Length>  
        [<ImportDisposition>import-disposition</ImportDisposition>]  
        page-range-list-or-block-list          
        [<MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>]  
        [<PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>]  
      </Blob>  
  
      <!-- Second Blob -->  
      <Blob>  
      . . .  
      </Blob>  
    </BlobList>  
  
    <!-- Second Blob List -->  
    <BlobList>  
    . . .  
    </BlobList>  
  </Drive>  
</DriveManifest>  
  
import-export-credential ::=   
  <StorageAccountKey>storage-account-key</StorageAccountKey> | <ContainerSas>container-sas</ContainerSas>  
  
page-range-list-or-block-list ::=   
  page-range-list | block-list  
  
page-range-list ::=   
    <PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
    </PageRangeList>  
  
block-list ::=  
    <BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       Hash="md5-hash"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       Hash="md5-hash"/>]  
    </BlockList>  

```

## <a name="manifest-xml-elements-and-attributes"></a>XML-elementen en kenmerken manifest

De gegevenselementen en kenmerken van het station manifest XML-indeling zijn opgegeven in de volgende tabel.  
  
|XML-Element|Type|Beschrijving|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Hoofdelement|Het hoofdelement van het manifestbestand. Alle elementen in het bestand zijn onder dit element.|  
|`Version`|Kenmerk, tekenreeks|De versie van het manifestbestand.|  
|`Drive`|Geneste XML-element|Bevat het manifest voor elk station.|  
|`DriveId`|Tekenreeks|De unieke stations-id voor het station. De stations-id worden gevonden door het opvragen van de schijf om bijbehorende serienummer. Het serienummer van het station wordt meestal aan de buitenkant van het station ook weergegeven. De `DriveID` element moet worden weergegeven voordat een `BlobList` element in het manifestbestand.|  
|`StorageAccountKey`|Tekenreeks|Vereist is voor de import taken als en alleen als `ContainerSas` is niet opgegeven. De accountsleutel voor de Azure storage-account gekoppeld aan de taak.<br /><br /> Dit element wordt weggelaten uit het manifest voor een exportbewerking.|  
|`ContainerSas`|Tekenreeks|Vereist is voor de import taken als en alleen als `StorageAccountKey` is niet opgegeven. De container SAS voor toegang tot de blobs die zijn gekoppeld aan de taak. Zie [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) voor de indeling. Dit element wordt weggelaten uit het manifest voor een exportbewerking.|  
|`ClientCreator`|Tekenreeks|Hiermee geeft u de client die het XML-bestand gemaakt. Deze waarde niet wordt geïnterpreteerd door de Import/Export-service.|  
|`BlobList`|Geneste XML-element|Bevat een lijst met blobs die deel uitmaken van het importeren of exporteren van de taak. Elke blob in een lijst met blob deelt dezelfde metagegevens en eigenschappen.|  
|`BlobList/MetadataPath`|Tekenreeks|Optioneel. Hiermee geeft u het relatieve pad van een bestand op de schijf die de metagegevens van het standaard die worden ingesteld op blobs in de lijst met blob voor een importbewerking bevat. Deze metagegevens kan eventueel worden genegeerd op basis van de blob met blob.<br /><br /> Dit element wordt weggelaten uit het manifest voor een exportbewerking.|  
|`BlobList/MetadataPath/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash-waarde voor het metagegevensbestand.|  
|`BlobList/PropertiesPath`|Tekenreeks|Optioneel. Hiermee geeft u het relatieve pad van een bestand op de schijf met de standaard-eigenschappen die worden ingesteld op blobs in de lijst met blob voor een importbewerking. Deze eigenschappen kunnen eventueel worden genegeerd op basis van de blob met blob.<br /><br /> Dit element wordt weggelaten uit het manifest voor een exportbewerking.|  
|`BlobList/PropertiesPath/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash-waarde voor het eigenschappenbestand.|  
|`Blob`|Geneste XML-element|Bevat informatie over elke blob in elke blob-lijst.|  
|`Blob/BlobPath`|Tekenreeks|De relatieve URI naar de blob beginnen met de containernaam van de. Als de blob van root-container, moet beginnen met `$root`.|  
|`Blob/FilePath`|Tekenreeks|Hiermee geeft u het relatieve pad naar het bestand op het station. Voor exporttaken, wordt het blobpad gebruikt voor het bestandspad indien mogelijk; *bijvoorbeeld*, `pictures/bob/wild/desert.jpg` worden geëxporteerd naar `\pictures\bob\wild\desert.jpg`. Vanwege de beperkingen van NTFS-namen, kan een blob worden geëxporteerd naar een bestand met een pad dat niet fungeren als het blobpad.|  
|`Blob/ClientData`|Tekenreeks|Optioneel. Bevat opmerkingen van de klant. Deze waarde niet wordt geïnterpreteerd door de Import/Export-service.|  
|`Blob/Snapshot`|Datum/tijd|Optioneel voor exporteren. Hiermee geeft u de momentopname-id voor een momentopname van de geëxporteerde blob.|  
|`Blob/Length`|Geheel getal|Geeft de totale lengte van de blob in bytes. De waarde van 200 GB voor een blok-blob is en maximaal 1 TB voor een pagina-blob. Deze waarde moet een veelvoud van 512 bytes, voor een pagina-blob.|  
|`Blob/ImportDisposition`|Tekenreeks|Optioneel voor de taken van gegevensimport weggelaten voor exporteren. Hiermee wordt aangegeven hoe de Import/Export-service moet verwerken het geval is bij een import-taak waarbij een blob met dezelfde naam al bestaat. Als deze waarde wordt weggelaten uit het manifest importeren, wordt de standaardwaarde is `rename`.<br /><br /> De waarden voor dit element zijn onder andere:<br /><br /> -   `no-overwrite`: Als er al een bestemmings-blob met dezelfde naam aanwezig is, slaat de importbewerking over dit bestand te importeren.<br />-   `overwrite`: Alle bestaande bestemmings-blob is volledig overschreven door het geïmporteerde bestand.<br />-   `rename`: De nieuwe blob wordt geüpload met een gewijzigde naam.<br /><br /> Het wijzigen van regel is als volgt:<br /><br /> -Als de blobnaam van de niet een punt bevat, een nieuwe naam wordt gegenereerd door toe te voegen `(2)` in de oorspronkelijke blobnaam; als deze nieuwe naam ook veroorzaakt een met de blobnaam van een bestaande, klikt u vervolgens conflict `(3)` wordt toegevoegd in plaats van `(2)`; enzovoort.<br />-De blob-naam bevat een punt, het gedeelte na de laatste punt worden beschouwd als de bestandsnaamextensie. Vergelijkbaar met de bovenstaande procedure `(2)` wordt ingevoegd voordat de laatste punt voor het genereren van een nieuwe naam; als de nieuwe naam nog steeds conflicteert met een bestaande blob-naam en vervolgens de service probeert `(3)`, `(4)`, enzovoort, totdat de naam van een niet-conflicterende wordt gevonden.<br /><br /> Een aantal voorbeelden:<br /><br /> De blob `BlobNameWithoutDot` worden gewijzigd in:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> De blob `Seattle.jpg` worden gewijzigd in:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|Geneste XML-element|Vereist voor een pagina-blob.<br /><br /> Voor een import geeft bewerking, een lijst met bereiken in bytes van een bestand dat moet worden geïmporteerd. Elk paginabereik is beschreven door een verschuiving en lengte in het bronbestand die worden beschreven van het paginabereik, samen met een MD5-hash van de regio. De `Hash` kenmerk van een paginabereik is vereist. De service valideert de berekende MD5-hash van het paginabereik komt overeen met de hash van de gegevens in de blob. Een willekeurig aantal paginabereiken kan worden gebruikt voor beschrijving van een bestand voor een import, met de totale grootte van 1 TB. Alle bereiken moeten worden gerangschikt op offset en geen overlappingen zijn toegestaan.<br /><br /> Voor het exporteren van een geeft bewerking, een set met bereiken in bytes van een blob die zijn geëxporteerd naar het station.<br /><br /> De paginabereiken kunnen samen alleen onderliggende bereiken van een blob of het bestand omvatten.  Het resterende deel van het bestand niet wordt gedekt door een paginabereik wordt verwacht en de inhoud ervan kan worden gedefinieerd.|  
|`PageRange`|XML-element|Hiermee geeft u een paginabereik.|  
|`PageRange/@Offset`|Kenmerk, geheel getal|Geeft de verschuiving in het overdrachtsbestand en de blob waar het opgegeven paginabereik begint. Deze waarde moet een veelvoud van 512 bytes zijn.|  
|`PageRange/@Length`|Kenmerk, geheel getal|Hiermee geeft u de lengte van het paginabereik. Deze waarde moet een veelvoud van 512 en niet meer dan 4 MB.|  
|`PageRange/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash-waarde voor het paginabereik.|  
|`BlockList`|Geneste XML-element|Vereist voor een blok-blob met benoemde blokken.<br /><br /> Voor een importbewerking geeft de lijst met geblokkeerde een reeks blokken die worden geïmporteerd in Azure Storage. Voor een bewerking exporteren geeft de lijst met geblokkeerde websites waar elk blok is opgeslagen in het bestand op de schijf exporteren. Elk blok is beschreven door een offset in het bestand en een bloklengte; elk blok is bovendien met de naam door een blok-ID-kenmerk en bevat een MD5-hash voor het blok. Maximaal 50.000 blokken kan worden gebruikt om een blob te beschrijven.  Alle blokken moeten worden geordend door de verschuiving en samen moeten voldoende zijn voor het volledige bereik van het bestand *dat wil zeggen*, moet er geen tussenruimte tussen blokken. Als de blob niet meer dan 64 MB is, moet de blok-id's voor elk blok alle ontbrekende of alle aanwezig. Blok-id's zijn vereist voor Base64-gecodeerde tekenreeksen zijn. Zie [plaatsen blok](/rest/api/storageservices/put-block) voor verdere vereisten voor blok-id's.|  
|`Block`|XML-element|Hiermee geeft u een blok.|  
|`Block/@Offset`|Kenmerk, geheel getal|Geeft de verschuiving waar het opgegeven blok begint.|  
|`Block/@Length`|Kenmerk, geheel getal|Hiermee geeft u het aantal bytes in het blok; Deze waarde moet niet meer dan 4MB.|  
|`Block/@Id`|Kenmerk, tekenreeks|Hiermee geeft u een tekenreeks voor de blok-ID voor het blok.|  
|`Block/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash van het blok.|  
|`Blob/MetadataPath`|Tekenreeks|Optioneel. Hiermee geeft u het relatieve pad van een bestand met metagegevens. Tijdens het importeren, worden de metagegevens is ingesteld op het bestemmings-blob. Metagegevens van de blob wordt tijdens een exportbewerking opgeslagen in het bestand met metagegevens op de schijf.|  
|`Blob/MetadataPath/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash van het metagegevensbestand van de blob.|  
|`Blob/PropertiesPath`|Tekenreeks|Optioneel. Hiermee geeft u het relatieve pad van een eigenschappenbestand. Tijdens het importeren, worden de eigenschappen ingesteld voor de bestemmings-blob. Tijdens een exportbewerking worden de blob-eigenschappen opgeslagen in het eigenschappenbestand op het station.|  
|`Blob/PropertiesPath/@Hash`|Kenmerk, tekenreeks|Hiermee geeft u de Base16 gecodeerd MD5-hash van de blob-bestand voor eigenschappen.|  
  
## <a name="next-steps"></a>Volgende stappen
 
* [REST-API van Storage Import/Export](/rest/api/storageimportexport/)
