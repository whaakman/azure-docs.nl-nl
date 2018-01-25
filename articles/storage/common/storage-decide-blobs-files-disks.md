---
title: Bepalen wanneer u wilt gebruiken Azure Blobs, Azure-bestanden of Azure-schijven
description: Meer informatie over de verschillende manieren opslaan en gebruiken van gegevens in Azure om te bepalen van welke technologie die u wilt gebruiken.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: b9c7913d1e95693a5ec72b24cf020928d67f0133
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Bepalen wanneer u wilt gebruiken Azure Blobs, Azure-bestanden of Azure-schijven

Microsoft Azure biedt verschillende functies in Azure Storage voor het opslaan en toegang tot uw gegevens in de cloud. Dit artikel bevat informatie over Azure Files, Blobs en schijven en is ontworpen om te kiezen tussen deze functies.

## <a name="scenarios"></a>Scenario's

De volgende tabel vergelijkt bestanden, Blobs en schijven en voorbeeldscenario's geschikt is voor elk bevat.

| Functie | Beschrijving | Wanneer gebruikt u dit? |
|--------------|-------------|-------------|
| **Azure Files** | Biedt een interface SMB, de clientbibliotheken, en een [REST-interface](/rest/api/storageservices/file-service-rest-api) waarmee toegang vanaf elke locatie opgeslagen bestanden. | U wilt 'lift- en verschuiven' een toepassing naar de cloud die het systeemeigen bestandssysteem API's al gebruikt voor het delen van gegevens tussen deze en andere toepassingen worden uitgevoerd in Azure.<br/><br/>U wilt opslaan ontwikkeling en foutopsporing van hulpprogramma's die moeten worden geopend vanaf een groot aantal virtuele machines. |
| **Azure Blobs** | Biedt clientbibliotheken en een [REST-interface](/rest/api/storageservices/blob-service-rest-api) waarmee ongestructureerde gegevens kunnen worden opgeslagen en gebruikt een grootschalige in blok-blobs. | Wilt u uw toepassing ter ondersteuning van streaming en willekeurige-scenario's.<br/><br/>U wilt toepassingsgegevens vanaf elke locatie toegang moeten hebben. |
| **Azure Disks** | Biedt clientbibliotheken en een [REST-interface](/rest/api/compute/manageddisks/disks/disks-rest-api) waarmee gegevens naar permanent worden opgeslagen en is toegankelijk vanuit een gekoppelde virtuele harde schijf. | Wilt u lift- en toepassingen die gebruikmaken van systeemeigen API's van het bestandssysteem te lezen en schrijven van gegevens naar permanente schijven verplaatsen.<br/><br/>U wilt opslaan van gegevens die u hoeft niet worden geopend vanaf buiten de virtuele machine waaraan de schijf is gekoppeld. |

## <a name="comparison-files-and-blobs"></a>Vergelijking: De bestanden en de Blobs

De volgende tabel vergelijkt de Azure-bestanden met Azure Blobs.  
  
||||  
|-|-|-|  
|**Kenmerk**|**Azure Blobs**|**Azure Files**|  
|Opties voor duurzaamheid|LRS, ZRS, GRS, RA-GRS|LRS, ZRS, GRS|  
|Toegankelijkheid|REST-API’s|REST-API’s<br /><br /> SMB 2.1 als SMB 3.0 (standaard bestandssysteem API's)|  
|Connectiviteit|REST-API's--wereldwijd|REST-API's - wereldwijd<br /><br /> SMB 2.1--binnen de regio<br /><br /> SMB 3.0 -- Worldwide|  
|Eindpunten|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Mappen|Platte naamruimte|De waarde True directory-objecten|  
|Hoofdlettergevoeligheid van namen|Hoofdlettergevoelig|Niet hoofdlettergevoelig, maar case behouden|  
|Capaciteit|Maximaal 500 TB containers|Bestandsshares 5 TB|  
|Doorvoer|Maximaal 60 MB/s per blok-blob|Maximaal 60 MB/s per share|  
|Grootte van object|Maximaal 200 GB/blok-blob|Maximaal 1 TB per bestand|  
|Gefactureerd capaciteit|Op basis van geschreven bytes|Op basis van de bestandsgrootte|  
|Clientbibliotheken|Meerdere talen|Meerdere talen|  
  
## <a name="comparison-files-and-disks"></a>Vergelijking: Bestanden en schijven

Azure Files een aanvulling op Azure-schijven. Een schijf kan alleen worden gekoppeld aan één Azure virtuele Machine op een tijdstip. Schijven worden vaste indeling VHD's opgeslagen als pagina-blobs in Azure Storage en door de virtuele machine worden gebruikt voor het opslaan van duurzame gegevens. Bestandsshares in de Azure-bestanden toegankelijk zijn op dezelfde manier als de lokale schijf (met behulp van systeemeigen API's van het bestandssysteem) wordt geopend en kunnen worden gedeeld door veel virtuele machines.  
 
De volgende tabel vergelijkt de Azure-bestanden met Azure-schijven.  
 
||||  
|-|-|-|  
|**Kenmerk**|**Azure Disks**|**Azure Files**|  
|Bereik|Uitsluitend van toepassing op een enkele virtuele machine|Gedeelde toegang over meerdere virtuele machines|  
|Momentopnamen en kopiëren|Ja|Nee|  
|Configuratie|Bij het opstarten van de virtuele machine verbonden|Verbonden nadat de virtuele machine is gestart|  
|Verificatie|Ingebouwd|Met net use instellen|  
|Opruimen|Automatisch|Handmatig|  
|Toegang met behulp van REST|Bestanden in de VHD kunnen niet worden geopend.|Bestanden die zijn opgeslagen op een share kunnen worden geopend|  
|Maximale grootte|4 TB-schijf|5 TB-bestandsshare en 1 TB bestand binnen share|  
|Max. 8KB IOP 's|500 IOP 's|1000 IOP 's|  
|Doorvoer|Maximaal 60 MB/s per schijf|Maximaal 60 MB/s per bestandsshare|  

## <a name="next-steps"></a>Volgende stappen

Wanneer u beslissingen neemt over hoe uw gegevens worden opgeslagen en toegankelijk is, moet u ook overwegen de kosten die zijn betrokken. Zie voor meer informatie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Sommige SMB-functies zijn niet van toepassing op de cloud. Zie voor meer informatie [die niet worden ondersteund door de Azure File service](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Zie voor meer informatie over schijven [beheren van schijven en installatiekopieën](../../virtual-machines/windows/about-disks-and-vhds.md) en [hoe een gegevensschijf koppelen aan een virtuele Windows-Machine](../../virtual-machines/windows/attach-managed-disk-portal.md).
