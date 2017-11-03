---
title: Azure bestanden schaalbaarheids- en prestatiedoelen | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure-bestanden, met inbegrip van de capaciteit, aanvraagsnelheid en ondergrenzen voor de bandbreedte voor inkomend en uitgaand.
services: storage
documentationcenter: na
author: wmgries
manager: klaasl
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/17/2017
ms.author: wgries
ms.openlocfilehash: 35d430b683224d1035cccdfb58b9db415d47ea3b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure schaalbaarheids- en prestatiedoelen van bestanden
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via het SMB-standaardprotocol. Dit artikel wordt de schaalbaarheids- en prestatiedoelen voor Azure-bestanden en Azure bestand Sync (Preview).

De schaalbaarheids- en prestatiedoelen hier vermeld zijn geavanceerde doelen, maar kunnen worden beïnvloed door andere variabelen in uw implementatie. Bijvoorbeeld, kan de doorvoer voor een bestand ook worden beperkt door de beschikbare netwerkbandbreedte niet alleen de servers die als host fungeert voor de service Azure-bestanden. Het is raadzaam het testen van uw gebruikspatroon om te bepalen of de schaalbaarheid en prestaties van Azure-bestanden voldoen aan uw vereisten. We zijn ook toegewezen aan het verhogen van deze limieten gedurende een bepaalde periode. Kunt u altijd Geef ons feedback, hetzij in de opmerkingen hieronder of op de [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), over welke beperkingen u zou willen zien ons verhogen.

## <a name="azure-storage-account-scale-targets"></a>Azure storage-account schalen doelen
De bovenliggende resource voor een Azure-bestandsshare is een Azure storage-account. Een opslagaccount vertegenwoordigt een groep met opslag in Azure die kan worden gebruikt door meerdere opslagservices, inclusief Azure-bestanden voor het opslaan van gegevens. Andere services die gegevens in de storage-accounts bevatten zijn Azure Blob storage, Azure Queue storage en Azure Table storage. De volgende doelen alle storage-services voor het opslaan van gegevens in een opslagaccount van toepassing:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Gebruik van de account van andere opslagservices opslag is van invloed op uw Azure-bestandsshares in uw opslagaccount. Bijvoorbeeld, als u de maximale opslagcapaciteit met account met Azure Blob storage bereiken, zich u niet kunnen maken van nieuwe bestanden op uw Azure-bestandsshare, zelfs als uw Azure-bestandsshare lager dan de grootte van de maximale bestandsshare is.

## <a name="azure-files-scale-targets"></a>Azure bestanden scale doelen
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure bestand Sync scale doelen
Met het synchroniseren van Azure-bestand, hebben we geprobeerd zo veel mogelijk voor een ontwerp voor oneindig gebruik, maar dit niet altijd mogelijk is. De onderstaande tabel geeft aan dat de grenzen van onze tests en welke doelen daadwerkelijk vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="see-also"></a>Zie ook
- [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md)
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)
- [Schaalbaarheids- en prestatiedoelen voor andere storage-services](../common/storage-scalability-targets.md)