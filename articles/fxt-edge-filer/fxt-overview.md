---
title: Overzicht van Microsoft Azure FXT Edge Filer
description: Beschrijving van Azure FXT Edge Filer hybride opslagcache, een bestand toegang accelerator oplossing voor hoge prestaties en actieve archiveren
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 58d4a2a52757b6ace1059fcccf379df3de5fd75c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542906"
---
# <a name="what-is-azure-fxt-edge-filer-hybrid-storage-cache"></a>Wat is Azure FXT Edge Filer hybride opslagcache?

Azure FXT Edge Filer is een hybride opslag caching-apparaat dat snelle toegang en actieve archief voor taken high performance computing (HPC biedt).

Het werkt met meerdere gegevensbronnen, of die zijn opgeslagen in een lokale datacentrum, op afstand, of in de cloud. De Azure FXT Edge Filer bieden een uniforme naamruimte voor gegevens in diverse opslagsystemen.

Drie of meer FXT Edge Filer hardwareapparaten samenwerken als een geclusterde bestandssysteem voor de cache. Neem contact op met uw Microsoft-vertegenwoordiger voor meer informatie over het aanschaffen van de vereiste hardware. 

Lees voor meer informatie het product informatie en gegevens blad op [Azure FXT Edge Filer](https://azure.microsoft.com/services/fxt-edge-filer/).

## <a name="use-cases"></a>Gebruiksvoorbeelden

De Azure FXT Edge Filer verbetert de productiviteit aanbevolen voor werkstromen, zoals deze:

* Leesintensief bestand-werkstroom 
* NFSv3 of SMB2-protocollen
* COMPUTE-farms van 1000 tot 100.000 CPU-kernen

### <a name="nas-optimization-and-scaling"></a>NAS-optimalisatie en schalen

De cache Azure FXT Edge Filer kunt u toegang tot bestaande systemen van NetApp en Dell EMC Isilon NAS soepel te verwerken. Ook kunt u Azure Blob of andere opslag in de cloud om schaalbaarheid zonder te hoeven bijwerken processen voor toegang tot de gegevens aan de clientzijde toevoegen. 

### <a name="wan-caching"></a>WAN-caching

Azure FXT Edge Filer kan worden gebruikt ter ondersteuning van snel bestandstoegang via Hoofdgebruikers wanneer de gegevens die ze nodig hebben elders wordt opgeslagen. Toegang bieden terwijl de back-up en andere beheersystemen gegevens blijven in een gecentraliseerd Datacenter. 

### <a name="active-archive-in-azure-blob"></a>Actieve archiveren in Azure Blob

Uw datacenter uitbreiden naar cloudopslag met Azure FXT Edge Filer als het toegangspunt. 

## <a name="features"></a>Functies 

Er zijn twee hardware modellen beschikbaar. 

| Model | DRAM | NVMe SSD | Netwerkpoorten | 
|-------|------|----------|---------------|
| FXT 6600 | 1536 GB | 25,6 TB | 6 x 25Gb / 10Gb + 2 x 1 Gb |
| FXT 6400 | 768 GB | 12,8 TB | 6 x 25Gb / 10Gb + 2 x 1 Gb |


## <a name="next-steps"></a>Volgende stappen

* Leren over de Azure FXT Edge Filer door te lezen die de [specificaties](fxt-specs.md) of [installatie zelfstudie](fxt-install.md).
* Informatie over het aanschaffen van Azure FXT Edge Filer op de [productpagina van Azure FXT Edge Filer](https://azure.microsoft.com/services/fxt-edge-filer/).