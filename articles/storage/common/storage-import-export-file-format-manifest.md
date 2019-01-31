---
title: Azure Import/Export-servicemanifest bestandsindeling | Microsoft Docs
description: Meer informatie over de indeling van het station manifestbestand waarin wordt beschreven van de toewijzing tussen de blobs in Azure Blob-opslag en bestanden op een station in een importeren of exporteren van de taak in de Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 831286f1c98a2fc3d26277f4006283c3de64f900
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463239"
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure Import/Export-service manifest-bestand-indeling
Het manifestbestand van het station beschrijving van de toewijzing tussen de blobs in Azure Blob-opslag en bestanden op schijf die bestaat uit een taak importeren of exporteren. Het manifestbestand is gemaakt als onderdeel van het voorbereidingsproces station en is opgeslagen op de schijf voordat het station wordt verzonden naar de Azure-Datacenter voor een importbewerking. Tijdens een exportbewerking, het manifest gemaakt en opgeslagen op de schijf met de Azure Import/Export-service.  
  
Voor beide importeren en exporteren van taken, zijn het manifestbestand van het station opgeslagen op de schijf importeren of exporteren; het is niet verzonden naar de service via een API-bewerking.  
  
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

## <a name="manifest-xml-elements-and-attributes"></a>Manifest van de XML-elementen en kenmerken

De gegevenselementen en kenmerken van het station manifest XML-indeling zijn opgegeven in de volgende tabel.  
  
|XML-Element|Type|Description|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Root-element|Het hoofdelement van het manifestbestand. Alle andere elementen in het bestand zijn onder dit element.|  
|`Version`|Tekenreeks-kenmerk|De versie van het manifestbestand.|  
|`Drive`|Geneste XML-element|Bevat het manifest voor elk station.|  
|`DriveId`|String|De unieke stations-id voor het station. De stations-id is gevonden door het opvragen van het station voor het serienummer. Het serienummer van het station wordt meestal aan de buitenkant van het station ook weergegeven. De `DriveID` element moet worden weergegeven voordat een `BlobList` element in het manifestbestand.|  
|`StorageAccountKey`|String|Vereist voor het importeren van taken als en alleen als `ContainerSas` is niet opgegeven. De accountsleutel voor de Azure storage-account die is gekoppeld aan de taak.<br /><br /> Dit element wordt weggelaten uit het manifest voor een bewerking voor exporteren.|  
|`ContainerSas`|String|Vereist voor het importeren van taken als en alleen als `StorageAccountKey` is niet opgegeven. De container SAS voor toegang tot de blobs die zijn gekoppeld aan de taak. Zie [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) voor de indeling. Dit element wordt weggelaten uit het manifest voor een bewerking voor exporteren.|  
|`ClientCreator`|String|Hiermee geeft u de client die het XML-bestand gemaakt. Deze waarde wordt niet geïnterpreteerd door de Import/Export-service.|  
|`BlobList`|Geneste XML-element|Bevat een lijst met blobs die deel uitmaken van het importeren of exporteren van de taak. Elke blob in een blob-lijst deelt dezelfde metagegevens en eigenschappen.|  
|`BlobList/MetadataPath`|String|Optioneel. Hiermee geeft u het relatieve pad van een bestand op de schijf die de metagegevens van de standaard die worden ingesteld op blobs in de blob-lijst voor een importbewerking bevat. Deze metagegevens kan eventueel worden genegeerd op basis van de blob-van-blob.<br /><br /> Dit element wordt weggelaten uit het manifest voor een bewerking voor exporteren.|  
|`BlobList/MetadataPath/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash-waarde voor het bestand met metagegevens.|  
|`BlobList/PropertiesPath`|String|Optioneel. Hiermee geeft u het relatieve pad van een bestand op de schijf met de standaardeigenschappen die worden ingesteld op blobs in de blob-lijst voor een importbewerking. Deze eigenschappen kunnen eventueel worden genegeerd op basis van de blob-van-blob.<br /><br /> Dit element wordt weggelaten uit het manifest voor een bewerking voor exporteren.|  
|`BlobList/PropertiesPath/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash-waarde voor het eigenschappenbestand.|  
|`Blob`|Geneste XML-element|Bevat informatie over elke blob in de lijst van elke blob.|  
|`Blob/BlobPath`|String|De relatieve URI naar de blob beginnen met de containernaam van de. Als de blob-root-container, moet beginnen met `$root`.|  
|`Blob/FilePath`|String|Hiermee geeft u het relatieve pad naar het bestand op het station. Voor export-taken, wordt het blobpad gebruikt voor het pad indien mogelijk; *bijvoorbeeld*, `pictures/bob/wild/desert.jpg` worden geëxporteerd naar `\pictures\bob\wild\desert.jpg`. Vanwege de beperkingen van NTFS-namen, kan een blob worden geëxporteerd naar een bestand met een pad dat niet lijken op het blobpad.|  
|`Blob/ClientData`|String|Optioneel. Bevat opmerkingen van de klant. Deze waarde wordt niet geïnterpreteerd door de Import/Export-service.|  
|`Blob/Snapshot`|DateTime|Dit is optioneel voor export-taken. Hiermee geeft u de momentopname-id voor een geëxporteerde blob-momentopname.|  
|`Blob/Length`|Geheel getal|Geeft de totale lengte van de blob in bytes. De waarde mag maximaal 200 GB voor een blok-blob zijn en maximaal 1 TB voor pagina-blob. Deze waarde moet een meervoud van 512 bytes, voor een pagina-blob.|  
|`Blob/ImportDisposition`|String|Dit is optioneel voor importtaken voor de export-taken worden weggelaten. Hiermee wordt aangegeven hoe de Import/Export-service moet verwerken de aanvraag voor een importtaak waar een blob met dezelfde naam al bestaat. Als deze waarde wordt weggelaten uit het manifest importeren, de standaardwaarde is `rename`.<br /><br /> De waarden voor dit element zijn onder andere:<br /><br /> -   `no-overwrite`: Als een bestemmings-blob al met dezelfde naam aanwezig is, slaat de importbewerking over dit bestand importeren.<br />-   `overwrite`: Eventuele bestaande bestemmings-blob is volledig overschreven door het geïmporteerde bestand.<br />-   `rename`: De nieuwe blob wordt geüpload met een gewijzigde naam.<br /><br /> De naam regel is als volgt:<br /><br /> -Als de naam van de blob niet een punt bevat, een nieuwe naam wordt gegenereerd door toe te voegen `(2)` in de oorspronkelijke blobnaam; als deze nieuwe naam ook veroorzaakt een met de blobnaam van een bestaande, klikt u vervolgens conflict `(3)` wordt toegevoegd in plaats van `(2)`; enzovoort.<br />-Als de naam van de blob een punt bevat, wordt de naam van de uitbreiding beschouwd als het gedeelte na de laatste punt. Vergelijkbaar met de bovenstaande procedure `(2)` wordt ingevoegd voordat de laatste punt voor het genereren van een nieuwe naam; als de nieuwe naam nog steeds conflicteert met een bestaande blob-naam en vervolgens de service probeert `(3)`, `(4)`, enzovoort, totdat de naam van een niet-conflicterende is gevonden.<br /><br /> Een aantal voorbeelden:<br /><br /> De blob `BlobNameWithoutDot` worden gewijzigd in:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> De blob `Seattle.jpg` worden gewijzigd in:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|Geneste XML-element|Vereist voor een pagina-blob.<br /><br /> Voor een geeft bewerking, een lijst met bytereeksen van een bestand dat moet worden geïmporteerd. Het paginabereik van elke is beschreven door een offset en lengte in de bronbestand met een beschrijving van het paginabereik, samen met een MD5-hash van de regio. De `Hash` kenmerk van het paginabereik van een is vereist. De service valideert dat de hash van de gegevens in de blob komt overeen met de berekende MD5-hash van het paginabereik. Een willekeurig aantal page-bereiken kan worden gebruikt om een bestand voor invoer te beschrijven, met de totale grootte van maximaal 1 TB. Alle bereiken moeten worden gesorteerd op offset en er geen overlap zijn toegestaan.<br /><br /> Exporteren van een bewerking, Hiermee geeft u een set met bytereeksen van een blob die zijn geëxporteerd naar het station.<br /><br /> De bereiken bij elkaar kunnen betrekking hebben alleen onderliggende bereiken van een blob of een bestand.  Het resterende deel van het bestand niet wordt gedekt door een bereik wordt verwacht en de inhoud is niet gedefinieerd.|  
|`PageRange`|XML-element|Hiermee geeft u een bereik.|  
|`PageRange/@Offset`|Kenmerk, geheel getal zijn|Geeft de verschuiving in het overdrachtsbestand en de blob waar het opgegeven paginabereik begint. Deze waarde moet een meervoud van 512 bytes.|  
|`PageRange/@Length`|Kenmerk, geheel getal zijn|Hiermee geeft u de lengte van het paginabereik. Deze waarde moet een meervoud van 512 en niet meer dan 4 MB.|  
|`PageRange/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash-waarde voor het paginabereik.|  
|`BlockList`|Geneste XML-element|Vereist voor een blok-blob met benoemde blokken.<br /><br /> Hiermee geeft u een reeks blokken die worden geïmporteerd in Azure Storage voor een bewerking importeren op de lijst met geblokkeerde websites. De lijst met geblokkeerde websites geeft voor een exportbewerking, waarbij elk blok is opgeslagen in het bestand op de schijf exporteren. Elk blok wordt beschreven door een offset in het bestand en een bloklengte; elk blok is bovendien met de naam van een blok-ID-kenmerk en een MD5-hash voor het blok bevat. Maximaal 50.000 blokken kan worden gebruikt om te beschrijven van een blob.  Alle blokken moeten worden besteld door de verschuiving en samen moeten betrekking hebben op het volledige bereik van het bestand, *dat wil zeggen*, moet er zijn geen tussenruimte tussen blokken. Als de blob niet langer zijn dan 64 MB is, moet de blok-id's voor elk blok alle ontbrekende of alle aanwezig zijn. Blok-id's moeten met Base64 gecodeerde tekenreeksen zijn. Zie [Put-blokken](/rest/api/storageservices/put-block) meer vereisten voor blok-id's.|  
|`Block`|XML-element|Hiermee geeft u een blok.|  
|`Block/@Offset`|Kenmerk, geheel getal zijn|Geeft de verschuiving waar het opgegeven blok begint.|  
|`Block/@Length`|Kenmerk, geheel getal zijn|Hiermee geeft u het aantal bytes in het blok; Deze waarde moet niet meer dan 4MB.|  
|`Block/@Id`|Tekenreeks-kenmerk|Hiermee geeft u een tekenreeks voor de blok-ID voor het blok.|  
|`Block/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash van het blok.|  
|`Blob/MetadataPath`|String|Optioneel. Hiermee geeft u het relatieve pad van een bestand met metagegevens. Tijdens het importeren, worden de metagegevens is ingesteld op de bestemmings-blob. Tijdens een exportbewerking is van de blob-metagegevens opgeslagen in het bestand met metagegevens op de schijf.|  
|`Blob/MetadataPath/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash van bestand met metagegevens van de blob.|  
|`Blob/PropertiesPath`|String|Optioneel. Hiermee geeft u het relatieve pad van een eigenschappenbestand. Tijdens het importeren, worden de eigenschappen worden ingesteld op de bestemmings-blob. Tijdens een exportbewerking, worden de blobeigenschappen opgeslagen in het eigenschappenbestand op het station.|  
|`Blob/PropertiesPath/@Hash`|Tekenreeks-kenmerk|Hiermee geeft u de Base16 gecodeerde MD5-hash van het bestand van de blob-eigenschappen.|  
  
## <a name="next-steps"></a>Volgende stappen
 
* [REST-API van Storage Import/Export](/rest/api/storageimportexport/)
