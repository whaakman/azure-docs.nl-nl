---
title: Bepalen wanneer u Azure-Blobs, Azure Files of Azure-schijven gebruiken
description: Meer informatie over de verschillende manieren voor het opslaan en toegang tot gegevens in Azure om te bepalen van welke technologie die u wilt gebruiken.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/28/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: bc6d615409f3c2d0f46286d2ad2ba20c32574afd
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091709"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Bepalen wanneer u Azure-Blobs, Azure Files of Azure-schijven gebruiken

Microsoft Azure biedt verschillende functies in Azure Storage voor het opslaan en openen van uw gegevens in de cloud. In dit artikel bevat informatie over Azure Files, Blobs en schijven, en is ontworpen om te kiezen tussen deze functies.

## <a name="scenarios"></a>Scenario's

De volgende tabel vergelijkt de bestanden, Blobs en schijven en ziet u geschikt is voor elk scenario's met voorbeelden.

| Functie | Beschrijving | Wanneer gebruikt u dit? |
|--------------|-------------|-------------|
| **Azure Files** | Voorziet in een SMB-interface, clientbibliotheken, en een [REST-interface](/rest/api/storageservices/file-service-rest-api) waarmee toegang vanaf elke locatie opgeslagen bestanden. | U wilt 'lift and shift' een toepassing in de cloud die al gebruikmaken van de systeemeigen bestandssysteem-API's voor het delen van gegevens tussen deze en andere toepassingen die worden uitgevoerd in Azure.<br/><br/>U wilt opslaan van de ontwikkeling en foutopsporing van hulpprogramma's die moeten worden geopend via een groot aantal virtuele machines. |
| **Azure-Blobs** | Beschikt over clientbibliotheken en een [REST-interface](/rest/api/storageservices/blob-service-rest-api) waarmee ongestructureerde gegevens worden opgeslagen en geopend op een zeer grote schaal in blok-blobs. | Wilt u uw toepassing voor de ondersteuning van streaming en scenario's voor willekeurige toegang.<br/><br/>U wilt toegang krijgen tot toepassingsgegevens vanaf elke locatie. |
| **Azure-schijven** | Beschikt over clientbibliotheken en een [REST-interface](/rest/api/compute/manageddisks/disks/disks-rest-api) waarmee gegevens worden permanent opgeslagen en geopend via een gekoppelde virtuele harde schijf. | Wilt u lift- and -shift-toepassingen die gebruikmaken van systeemeigen bestandssysteem-API's om te lezen en schrijven van gegevens naar permanente schijven.<br/><br/>U wilt opslaan van gegevens die is niet vereist voor toegang vanuit buiten de virtuele machine waarop de schijf is aangesloten. |

## <a name="comparison-files-and-blobs"></a>Vergelijking: Bestanden en Blobs

De volgende tabel vergelijkt de bestanden van Azure met Azure-Blobs.  
  
||||  
|-|-|-|  
|**Kenmerk**|**Azure-Blobs**|**Azure Files**|  
|Opties voor duurzaamheid|LRS, ZRS, GRS, RA-GRS|LRS, ZRS, GRS|  
|Toegankelijkheid|REST-API’s|REST-API’s<br /><br /> SMB 2.1 als SMB 3.0 (standard bestandssysteem-API's)|  
|Connectiviteit|REST-API's over de hele wereld|REST-API's - wereldwijd<br /><br /> SMB 2.1--binnen regio<br /><br /> SMB 3.0--over de hele wereld|  
|Eindpunten|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Mappen|Platte-naamruimte|De waarde True directory-objecten|  
|Hoofdlettergevoeligheid van namen|Hoofdlettergevoelig|Niet hoofdlettergevoelig, maar de aanvraag te behouden|  
|Capaciteit|Maximaal 500 TiB containers|5 TiB-bestandsshares|  
|Doorvoer|Maximaal 60 MiB/s per blok-blob|Maximaal 60 MiB/s per share|  
|Grootte van object|Maximaal ongeveer 4.75 TiB per blok-blob|Tot 1 TiB per bestand|  
|Gefactureerde capaciteit|Op basis van aantal geschreven bytes|Op basis van de bestandsgrootte|  
|Clientbibliotheken|Meerdere talen|Meerdere talen|  
  
## <a name="comparison-files-and-disks"></a>Vergelijking: Bestanden en schijven

Azure Files een aanvulling op Azure-schijven. Een schijf kan alleen worden gekoppeld aan één Azure virtuele Machine op een tijdstip. Schijven zijn vaste indeling VHD's opgeslagen als pagina-blobs in Azure Storage, en door de virtuele machine worden gebruikt voor duurzame gegevensopslag. Bestandsshares in Azure Files kunnen worden geopend op dezelfde manier als de lokale schijf wordt geopend (met behulp van systeemeigen bestandssysteem-API's) en kunnen worden gedeeld met veel virtuele machines.  
 
De volgende tabel vergelijkt Azure Files met Azure-schijven.  
 
||||  
|-|-|-|  
|**Kenmerk**|**Azure-schijven**|**Azure Files**|  
|Bereik|Exclusief voor een enkele virtuele machine|Gedeelde toegang over meerdere virtuele machines|  
|Momentopnamen en kopiëren|Ja|Ja|  
|Configuratie|Bij het opstarten van de virtuele machine verbonden|Verbonden nadat de virtuele machine is gestart|  
|Verificatie|Ingebouwd|Met net use instellen|  
|Opschonen|Automatisch|Handmatig|  
|Toegang met behulp van REST|Bestanden in de VHD kunnen niet worden geopend.|Bestanden die zijn opgeslagen in een share kunnen worden geopend.|  
|Maximale grootte|4 TiB-schijf|5 TiB-bestandsshare en 1 TiB-bestand in de share|  
|Max. aantal IOP 's|500 IOP 's|1000 IOps|  
|Doorvoer|Maximaal 60 MiB/s per schijf|Het doel is 60 MiB/s per bestandsshare (ophalen voor hogere i/o-grootte hoger)|  

## <a name="next-steps"></a>Volgende stappen

Bij het nemen van besluiten over hoe uw gegevens worden opgeslagen en gebruikt, moet u ook overwegen de kosten die betrokken zijn. Zie voor meer informatie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Sommige SMB-functies zijn niet van toepassing op de cloud. Zie voor meer informatie, [die niet worden ondersteund door de Azure File service](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Zie voor meer informatie over schijven [schijven en installatiekopieën beheren](../../virtual-machines/windows/about-disks-and-vhds.md) en [hoe u een gegevensschijf koppelen aan een Windows-Machine](../../virtual-machines/windows/attach-managed-disk-portal.md).
