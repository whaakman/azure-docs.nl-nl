---
title: Problemen bij de implementatie voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: Dit artikel worden de veelgestelde vragen over de implementatie voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/20/2017
ms.author: genli
ms.openlocfilehash: 755b8e7414f6e77d0013d2678e8d4228091e1e4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemen bij de implementatie voor Azure Cloud Services: veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over problemen bij de implementatie voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Waarom soms een cloudservice implementeren in de faseringssleuf mislukt met een resource toewijzingsfout als er al een bestaande implementatie in de productiesite?
Als een cloudservice een implementatie in een sleuf heeft, wordt de hele cloudservice is vastgemaakt aan een specifiek cluster. Dit betekent dat als een implementatie al in de productiesite bestaat, een nieuwe implementatie van de test kan alleen worden toegewezen in hetzelfde cluster als de productiesite.

Toewijzingsfouten optreden wanneer het cluster waar uw cloudservice zich bevindt geen voldoende fysieke rekenresources om te voldoen aan uw implementatieaanvraag.

Zie voor informatie over deze toewijzingsfouten beperkende [Service in de Cloud toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Waarom wordt omhoog schalen of buiten een cloud service-implementatie soms leiden tot toewijzingsfout schalen?
Wanneer een cloudservice wordt geïmplementeerd, opgehaald deze meestal vastgemaakt aan een specifieke cluster. Dit betekent dat nieuwe exemplaren in hetzelfde cluster schalen van een bestaande cloudservice up/out moet toewijzen. Als het cluster heeft capaciteit bijna bereikt of het gewenste VM-grootte/type niet beschikbaar is, mislukken de aanvraag.

Zie voor informatie over deze toewijzingsfouten beperkende [Service in de Cloud toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Waarom voor het implementeren van een cloudservice in een affiniteitsgroep is het soms resulteert in toewijzingsfout?
Een nieuwe implementatie met een leeg cloudservice kan worden toegewezen door de infrastructuurresources in een cluster in deze regio, tenzij de cloudservice is vastgemaakt aan een affiniteitsgroep. Implementaties van dezelfde affiniteitsgroep worden geprobeerd in hetzelfde cluster. Als het cluster heeft capaciteit bijna bereikt, kan het verzoek mislukken.

Zie voor informatie over deze toewijzingsfouten beperkende [Service in de Cloud toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Waarom VM-grootte wijzigen of het toevoegen van een nieuwe virtuele machine aan een bestaande cloudservice soms resulteert in toewijzingsfout?
De clusters in een datacenter mogelijk verschillende configuraties van typen machines (bijvoorbeeld een reeks, Av2 reeks, D-reeks, Dv2 reeks, reeks G, H reeks, enzovoort). Maar niet alle clusters zou hoeven alle soorten virtuele machines. Bijvoorbeeld, als u probeert een virtuele machine van de D-reeks toevoegen aan een cloudservice die al is geïmplementeerd in een reeks alleen-lezen cluster, treden er al een geheugentoewijzing is mislukt. Dit wordt ook gebeuren als u probeert te wijzigen dat VM SKU groottes (bijvoorbeeld uit een reeks A overschakelen naar een reeks D).

Zie voor informatie over deze toewijzingsfouten beperkende [Service in de Cloud toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

De grootten die beschikbaar zijn in uw regio Zie [Microsoft Azure: producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Waarom enige tijd opnieuw implementeren van een cloudservice mislukt als gevolg van quota-limieten/beperkingen op mijn abonnement of de service?
Implementatie van een cloudservice kan mislukken als de resources die vereist zijn om te worden toegewezen groter zijn dan de standaard- of maximumquotum toegestaan voor uw service op het niveau van de regio/datacenter. Zie voor meer informatie [Cloudservices beperkt](../azure-subscription-service-limits.md#cloud-services-limits).

U kan ook de huidige gebruiksgegevens voor uw abonnement op de portal volgen: Azure-portal = > abonnementen = > \<betreffende abonnement > = > 'Gebruik + quotum'.

Gebruik/verbruik-gerelateerde resourcegegevens kan ook worden opgehaald via de Azure Billing API's. Zie [Azure-Resource gebruiks-API (Preview)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Hoe kan ik het formaat van een geïmplementeerde cloudservice-VM wijzigen zonder dat dit?
U kunt de VM-grootte van een geïmplementeerde cloudservice niet wijzigen zonder deze opnieuw distribueren. De VM-grootte is ingebouwd in het CSDEF, kan alleen worden bijgewerkt met een implementatie opnieuw uit.

Zie voor meer informatie [het bijwerken van een cloudservice](cloud-services-update-azure-service.md).

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>Waarom kan ik niet implementeren van Cloud-Services via Service Management-API's of PowerShell bij gebruik van Azure Resource Manager Storage-account? 

Aangezien de Cloudservice een klassieke resource die niet rechtstreeks compatibel zijn met het Azure Resource Manager-model is, kan niet koppelt u deze met de Azure Resource Manager Storage-accounts. Hier volgen een aantal opties: 
 
- Via REST API wordt geïmplementeerd.

    Wanneer u via REST API van Opslagservicebeheer implementeert, kan u rond de beperking ophalen door te geven van een SAS-URL naar de blob-opslag die compatibel is met zowel klassieke en het Azure Resource Manager-Opslagaccount. Meer informatie over de eigenschap 'PackageUrl' [hier](https://msdn.microsoft.com/library/azure/ee460813.aspx).
  
- Implementeren via [Azure-portal](https://portal.azure.com).

    Dit werkt vanuit de [Azure-portal](https://portal.azure.com) als de aanroep verloopt via een proxy/shim waarmee de communicatie tussen Azure Resource Manager en klassieke resources. 
 
