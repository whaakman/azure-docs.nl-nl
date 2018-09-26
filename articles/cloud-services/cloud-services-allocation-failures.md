---
title: Problemen met de Cloudservice toewijzing oplossen | Microsoft Docs
description: Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
services: azure-service-management, cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 22888c76b27d287a8d7fb0f0f1f0a0d39d92375d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092949"
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
## <a name="summary"></a>Samenvatting
Wanneer u exemplaren naar een Cloudservice distribueren of toevoegen van nieuwe web- of werkrolinstanties, wijst Microsoft Azure compute-resources. U ontvangt mogelijk af en toe fouten bij het uitvoeren van deze bewerkingen, zelfs voordat u de limieten van Azure-abonnement hebt bereikt. Dit artikel worden de oorzaken van enkele van de algemene toewijzingsfouten en mogelijke herstel voorgesteld. De informatie kan ook zijn nuttig wanneer u van plan de implementatie van uw services bent.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Achtergrond: de werking van toewijzing
De servers in Azure-datacenters worden in clusters gepartitioneerd. Een nieuwe cloud service toewijzingsaanvraag in meerdere clusters. Wanneer het eerste exemplaar is geïmplementeerd in een cloudservice (in fasering of productie), die service in de cloud wordt vastgemaakt aan een cluster. Eventuele verdere implementaties voor de cloudservice gebeurt in hetzelfde cluster. In dit artikel verwijzen we naar dit als 'vastgemaakt aan een cluster'. Afbeelding 1 hieronder ziet u dat het geval van een normale verdeling die wordt uitgevoerd in meerdere clusters. Diagram 2 ziet u het geval van een toewijzing die is vastgemaakt aan het Cluster 2 omdat daar waar de bestaande CS_1 voor Cloud-Service wordt gehost.

![Diagram van toewijzing](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Waarom toewijzingsfout gebeurt
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans niet te zoeken naar gratis bronnen omdat de resourcegroep beschikbaar beperkt tot een cluster is. Bovendien, als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar het type resource dat u hebt aangevraagd niet wordt ondersteund door het cluster, uw aanvraag mislukt, zelfs als het cluster is, gratis resource. Afbeelding 3 hieronder ziet u dat het geval waarbij een vastgemaakte toewijzing is mislukt omdat het cluster alleen candidate heeft geen gratis resources. Diagram 4 ziet u het geval waarbij een vastgemaakte toewijzing is mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de aangevraagde VM-grootte, zelfs als het cluster gratis resources heeft.

![Vastgemaakte toewijzingsfout](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Toewijzingsfouten voor cloudservices oplossen
### <a name="error-message"></a>Foutbericht
U ziet mogelijk de volgende strekking weergegeven:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Algemene problemen
Hier volgen de algemene toewijzing scenario's die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt aan een cluster.

* Naar de Staging-site - implementeert als een service in de cloud een implementatie in een sleuf heeft, is klikt u vervolgens het hele cloudservice vastgemaakt aan een specifieke cluster.  Dit betekent dat als een implementatie al in de productiesite bestaat, dan kan een nieuwe faseringsimplementatie alleen worden toegewezen in hetzelfde cluster als de productiesite. Als het cluster heeft capaciteit bijna bereikt, mislukken de aanvraag.
* Schalen: nieuwe exemplaren toe te voegen aan een bestaande cloudservice moet toewijzen in hetzelfde cluster.  Kleine schaalaanvragen kunnen doorgaans worden toegewezen, maar niet altijd. Als het cluster heeft capaciteit bijna bereikt, mislukken de aanvraag.
* Affiniteitsgroep - een nieuwe implementatie in een lege cloudservice kan worden toegewezen door de infrastructuur in een cluster in deze regio, tenzij de cloudservice is vastgemaakt aan een affiniteitsgroep. Implementaties naar dezelfde affiniteitsgroep bevinden worden geprobeerd in hetzelfde cluster. Als het cluster heeft capaciteit bijna bereikt, mislukken de aanvraag.
* Affiniteitsgroep vNet - oudere virtuele netwerken zijn gekoppeld aan affiniteitsgroepen in plaats van regio's en cloudservices in deze virtuele netwerken zou worden vastgemaakt aan het cluster affiniteitsgroep bevinden. Implementaties voor dit type virtueel netwerk worden geprobeerd op de vastgemaakte cluster. Als het cluster heeft capaciteit bijna bereikt, mislukken de aanvraag.

## <a name="solutions"></a>Oplossingen
1. Implementeren naar een nieuwe cloudservice - in deze oplossing is waarschijnlijk meest succesvolle omdat hiermee het platform om te kiezen uit alle clusters in die regio.

   * De workload van een nieuwe cloudservice implementeren  
   * De CNAME- of een record verkeer verwijst naar de nieuwe cloudservice bijwerken
   * Zodra de oude site nul verkeer wordt gebruikt, kunt u de oude cloudservice kunt verwijderen. Deze oplossing moet in rekening worden gebracht zonder downtime.
2. Verwijder zowel productie- als stagingsleuven - in deze oplossing behoudt de bestaande DNS-naam, maar zorgt ervoor dat de downtime voor uw toepassing.

   * Verwijderen van de productie- als stagingsleuven van een bestaande cloudservice zodat de cloudservice leeg is, en vervolgens
   * Maak een nieuwe implementatie in de bestaande cloudservice. Dit wordt opnieuw geprobeerd te toewijzing op alle clusters in de regio. Zorg ervoor dat de cloudservice is niet gekoppeld aan een affiniteitsgroep.
3. Gereserveerde IP-adres - deze oplossing behoudt de bestaande IP-adres, maar zorgt ervoor dat de downtime voor uw toepassing.  

   * Maken van een gereserveerde IP-adres voor uw bestaande implementatie met behulp van Powershell

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Ga als volgt #2 in het navolgende, zorg ervoor dat de nieuwe gereserveerde IP-adres in van de service CSCFG opgeven.
4. Verwijder affiniteitsgroep voor nieuwe implementaties - Affiniteitsgroepen zijn niet meer wordt aanbevolen. Volg de stappen voor #1 hierboven voor het implementeren van een nieuwe cloudservice. Zorg ervoor dat cloudservice zich niet in een affiniteitsgroep.
5. Converteren naar een regionaal Virtueelnetwerk - Zie [van Affiniteitsgroepen migreren naar een regionaal Virtueelnetwerk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
