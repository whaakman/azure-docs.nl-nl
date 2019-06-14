---
title: Implementatieoverzicht van - Avere vFXT voor Azure
description: Overzicht van de implementatie van Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 88305e5d877f5bc3cc7ebb116f69b0f49f162b79
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409909"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT voor Azure - implementatie-overzicht

In dit artikel biedt een overzicht van de stappen die nodig zijn om op te halen van een vFXT Avere voor Azure-cluster omhoog en die wordt uitgevoerd.

Verschillende taken die nodig zijn voor en na het maken van het cluster vFXT vanuit Azure Marketplace. Met een helder beeld van het proces van begin tot eind, krijgt u het bereik van de hoeveelheid werk die nodig zijn. 

## <a name="deployment-steps"></a>Implementatiestappen

Na [plannen van uw systeem](avere-vfxt-deploy-plan.md), kunt u beginnen met het maken van uw Avere vFXT-cluster. 

Een Azure Resource Manager-sjabloon in de Azure Marketplace de benodigde gegevens verzamelt en wordt het hele cluster automatisch geïmplementeerd. 

Nadat het cluster vFXT actief en werkend is, u wilt weten hoe u kunt clients verbinden en, indien nodig, het verplaatsen van uw gegevens naar de nieuwe Blob storage-container.  

Hier volgt een overzicht van alle de stappen.

1. Vereisten configureren 

   Voordat u een virtuele machine maakt, moet u een nieuw abonnement voor het Avere vFXT-project maken, abonnement eigendom configureren, quota controleren en een verhoging als nodig hebt en voorwaarden voor het gebruik van de Avere vFXT-software accepteren. Lezen [voorbereidingen voor het maken van de vFXT Avere](avere-vfxt-prereqs.md) voor gedetailleerde instructies.

1. De Avere vFXT-cluster maken 

   Gebruik de Azure Marketplace te maken van de vFXT Avere voor Azure-cluster. Een sjabloon voor de vereiste gegevens verzamelt en scripts voor het maken van het uiteindelijke product wordt uitgevoerd.

   Maken van een cluster bestaat uit stappen die worden uitgevoerd door de marketplace-sjabloon: 

   * Het maken van nieuwe netwerk-infrastructuur en resource-groepen, indien nodig
   * Het maken van een *clustercontroller*  

     De clustercontroller is een eenvoudige virtuele machine die zich in hetzelfde virtuele netwerk bevinden als het Avere vFXT cluster bevinden en de aangepaste software nodig is om te maken en beheren van het cluster. De controller maakt de formulieren en vFXT knooppunten aan het cluster en het bevat ook een opdrachtregelinterface voor het beheren van het cluster tijdens de levensduur.

     Als u een nieuw vnet tijdens de implementatie maken, is de controller heeft een openbaar IP-adres. Dit betekent dat de controller kan fungeren als host voor een korte inleiding voor de verbinding met het cluster op basis van buiten het vnet.

   * Het maken van het cluster knooppunt VM 's

   * Het clusterknooppunt VM's om het cluster configureren

   * (Optioneel) een nieuwe Blob-container maken en deze configureren als back-end-opslag voor het cluster

1. Het cluster configureren 

   Verbinding maken met de Avere vFXT configuratie-interface (Configuratiescherm Avere) om aan te passen van de instellingen van het cluster. Opt-in voor het bewaken van ondersteuning en uw opslagsysteem toevoegen als u een on-premises Datacenter.

   * [Toegang tot het cluster vFXT](avere-vfxt-cluster-gui.md)
   * [Ondersteuning inschakelen](avere-vfxt-enable-support.md)
   * [Opslag configureren](avere-vfxt-add-storage.md) (indien nodig)

1. Koppelen van clients

   Volg de richtlijnen in [koppelen van het cluster van Avere vFXT](avere-vfxt-mount-clients.md) voor meer informatie over taakverdeling en hoe clientcomputers het cluster moeten koppelen.

1. Gegevens toevoegen (indien nodig)

   Omdat de vFXT Avere een schaalbaar multi-clientcache is, is de beste manier om gegevens te verplaatsen naar een nieuwe back-end-storage-container met meerdere client, met meerdere threads bestand kopiëren strategie. Lezen [om gegevens te verplaatsen naar het cluster vFXT](avere-vfxt-data-ingest.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Blijven [voorbereidingen voor het maken van de vFXT Avere](avere-vfxt-prereqs.md) de voorbereidende taken voor het implementeren van de vFXT Avere voor Azure. 