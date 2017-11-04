---
title: Cloud toewijzingsfout bij het oplossen van problemen | Microsoft Docs
description: Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
services: azure-service-management, cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 33d017d0e09e9b288b0514e85c8865f83a8a2fa1
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
## <a name="summary"></a>Samenvatting
Als u exemplaren naar een Cloudservice distribueren of nieuwe web- of worker-rolexemplaren toevoegen, wijst Microsoft Azure compute-bronnen. Tijd tot tijd krijgt u fouten bij het uitvoeren van deze bewerkingen zelfs voordat u de Azure-abonnement limieten bereiken. Dit artikel wordt uitgelegd van de oorzaken van een deel van de algemene toewijzingsfouten en mogelijk doorvoeren wordt voorgesteld. De gegevens zijn mogelijk ook nuttig bij het plannen van de implementatie van uw services.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Achtergrond – de werking van toewijzing
De servers in Azure-datacenters worden in clusters gepartitioneerd. Een nieuwe toewijzingsaanvraag voor cloud-service wordt uitgevoerd in meerdere clusters. Wanneer de eerste instantie wordt geïmplementeerd op een cloudservice (in fasering of productie), die cloudservice opgehaald vastgemaakt aan een cluster. Eventuele verdere implementaties voor de cloudservice gebeurt in hetzelfde cluster. In dit artikel verwijzen we naar dit zoals 'vastgemaakt aan een cluster'. Afbeelding 1 hieronder ziet u dat het geval van een normale toewijzing die wordt uitgevoerd in meerdere clusters; Diagram 2 ziet u dat het geval van een toewijzing die is vastgemaakt aan Cluster 2 omdat waar de bestaande CS_1 voor Cloud-Service wordt gehost.

![Diagram van toewijzing](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Waarom toewijzingsfout gebeurt
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans niet te vinden van resources vrij, omdat de resourcegroep beschikbaar beperkt tot een cluster is. Bovendien als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar het type resource dat u hebt aangevraagd wordt niet ondersteund door dit cluster, zal uw aanvraag mislukken zelfs als het cluster gratis resource heeft. Diagram 3 hieronder ziet u het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate geen gratis resources. Diagram 4 ziet u het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de aangevraagde VM-grootte, zelfs als het cluster resources vrij heeft.

![Vastgemaakte geheugentoewijzing is mislukt](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Toewijzingsfouten voor cloudservices oplossen
### <a name="error-message"></a>Foutbericht
U weergegeven het volgende foutbericht:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Algemene problemen
Hier volgen de algemene toewijzing scenario's die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt aan één cluster.

* Implementeren naar sleuf Staging - als een cloudservice een implementatie in een sleuf heeft, is klikt u vervolgens de volledige in de cloud-service vastgemaakt aan een specifiek cluster.  Dit betekent dat als een implementatie al in de productiesite bestaat, klikt u vervolgens een nieuwe implementatie van de test kan alleen worden toegewezen in hetzelfde cluster als de productiesite. Als het cluster heeft capaciteit bijna bereikt, kan het verzoek mislukken.
* Schalen: nieuwe exemplaren toe te voegen aan een bestaande cloudservice moet toewijzen in hetzelfde cluster.  Kleine schaal aanvragen kan doorgaans worden verdeeld, maar niet altijd. Als het cluster heeft capaciteit bijna bereikt, kan het verzoek mislukken.
* Affiniteitsgroep - een nieuwe implementatie met een leeg cloudservice kan worden toegewezen door de infrastructuurresources in een cluster in deze regio, tenzij de cloudservice is vastgemaakt aan een affiniteitsgroep. Implementaties van dezelfde affiniteitsgroep worden geprobeerd in hetzelfde cluster. Als het cluster heeft capaciteit bijna bereikt, kan het verzoek mislukken.
* Affiniteitsgroep vNet - oudere virtuele netwerken zijn gekoppeld aan affiniteitsgroepen in plaats van de regio's en cloudservices in deze virtuele netwerken zou worden vastgemaakt aan het cluster affiniteitsgroep. Implementaties voor dit type virtueel netwerk wordt een poging op het cluster vastgemaakt. Als het cluster heeft capaciteit bijna bereikt, kan het verzoek mislukken.

## <a name="solutions"></a>Oplossingen
1. Implementeren naar een nieuwe cloudservice - deze oplossing is waarschijnlijk meest geslaagd omdat het platform kunt kiezen uit alle clusters in deze regio.

   * De werkbelasting implementeren op een nieuwe cloudservice  
   * De CNAME- of een record verkeer verwijst naar de nieuwe cloudservice bijwerken
   * Zodra de oude site nul verkeer gaat, kunt u de oude cloudservice verwijderen. Deze oplossing moet rekening worden gebracht zonder uitvaltijd.
2. Verwijderen van de productie- en staging-sleuven: deze oplossing uw bestaande DNS-naam blijft behouden, maar zal leiden tot uitvaltijd voor uw toepassing.

   * De productie- en staging-sleuven van een bestaande cloudservice zodat de cloudservice leeg is, verwijderen en vervolgens
   * Maak een nieuwe implementatie in de bestaande cloudservice. Dit wordt opnieuw geprobeerd te toewijzing in alle clusters in de regio. Zorg ervoor dat de cloudservice is niet gekoppeld aan een affiniteitsgroep.
3. Gereserveerd IP - behoudt deze oplossing voor uw bestaande IP-adres, maar zal leiden tot uitvaltijd voor uw toepassing.  

   * Maken van een ReservedIP voor uw bestaande implementatie met behulp van Powershell

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Ga als volgt #2 hierboven, alleen voor zorgen dat de nieuwe ReservedIP opgeven in de service CSCFG.
4. Verwijder affiniteitsgroep voor nieuwe implementaties - Affiniteitsgroepen niet langer worden aanbevolen. Volg de stappen voor #1 hierboven voor het implementeren van een nieuwe cloudservice. Zorg ervoor dat cloudservice is niet in een affiniteitsgroep.
5. Converteren naar een regionaal virtueel netwerk - Zie [van Affiniteitsgroepen migreren naar een regionaal virtueel netwerk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
