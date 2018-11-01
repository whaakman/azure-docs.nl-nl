---
title: Implementatieoverzicht van - Avere vFXT voor Azure
description: Overzicht van de implementatie van Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633961"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT voor Azure - implementatie-overzicht

In dit artikel biedt een overzicht van de stappen die nodig zijn om op te halen van een vFXT Avere voor Azure-cluster omhoog en die wordt uitgevoerd.

De eerste keer dat u een Avere vFXT systeem implementeert, zult u merken dat deze bestaat uit meer stappen dan het implementeren van de meeste andere Azure-hulpprogramma's. Met een helder beeld van het proces van begin tot eind, krijgt u het bereik van de hoeveelheid werk die nodig zijn. Nadat het systeem actief en werkend is, maakt de kracht om te versnellen, op basis van cloud computing-taken het zeker de moeite waard.

## <a name="deployment-steps"></a>Implementatiestappen

Na [plannen van uw systeem](avere-vfxt-deploy-plan.md), kunt u beginnen met het maken van uw Avere vFXT-cluster. 

Beginnen met het maken van een cluster domeincontroller-VM, die wordt gebruikt om de vFXT-cluster te maken.

Nadat het cluster vFXT actief en werkend is, u wilt weten hoe u kunt clients verbinden en, indien nodig, het verplaatsen van uw gegevens naar de nieuwe Blob storage-container.  

Hier volgt een overzicht van alle de stappen.

1. Vereisten configureren 

   Voordat u een virtuele machine maakt, moet u een nieuw abonnement voor het Avere vFXT-project maken, abonnement eigendom configureren, quota controleren en een verhoging als nodig hebt en voorwaarden voor het gebruik van de Avere vFXT-software accepteren. Lezen [voorbereidingen voor het maken van de vFXT Avere](avere-vfxt-prereqs.md) voor gedetailleerde instructies.

1. De netwerkcontroller cluster maken

   De *clustercontroller* is een eenvoudige virtuele machine die zich in hetzelfde virtuele netwerk bevinden als het Avere vFXT cluster bevinden. De controller maakt de formulieren en vFXT knooppunten aan het cluster en het bevat ook een opdrachtregelinterface voor het beheren van het cluster tijdens de levensduur.

   Als u de controller is geconfigureerd met een openbaar IP-adres, kan deze ook dienen als een host landingspagina voor de verbinding met het cluster op basis van buiten het vnet.

   Alle software die nodig zijn voor het cluster vFXT maken en beheren van de knooppunten ervan is vooraf geïnstalleerd op de clustercontroller.

   Lezen [maken van de clustercontroller VM](avere-vfxt-deploy.md#create-the-cluster-controller-vm) voor meer informatie.

1. Maak een runtimerol voor de clusterknooppunten 

   Azure maakt gebruik van [op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) voor het toestaan van het clusterknooppunt VM's bepaalde taken uit te voeren. Bijvoorbeeld, moeten de clusterknooppunten kunnen toe te wijzen of opnieuw toewijzen van IP-adressen aan andere knooppunten van het cluster. Voordat u het cluster maakt, moet u een rol met voldoende machtigingen definiëren.

   De clustercontroller vooraf geïnstalleerde software omvat een prototype-rol voor u om aan te passen. Lezen [maken van de functie cluster knooppunt toegang](avere-vfxt-deploy.md#create-the-cluster-node-access-role) voor instructies.

1. De Avere vFXT-cluster maken 

   Op de controller het juiste cluster maken van het script bewerken en uitvoeren om het cluster te maken. [Bewerk het script voor implementatie](avere-vfxt-deploy.md#edit-the-deployment-script) bevat gedetailleerde instructies. 

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