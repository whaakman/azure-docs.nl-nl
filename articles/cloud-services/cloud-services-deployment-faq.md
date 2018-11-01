---
title: Problemen bij de implementatie voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: In dit artikel geeft een lijst van de veelgestelde vragen over de implementatie voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: d44cfc0164892c34bcbe16ca07e9ec67190ada24
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50415301"
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemen bij de implementatie voor Azure Cloud Services: veelgestelde vragen

Dit artikel bevat veelgestelde vragen over problemen met de implementatie voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Waarom een cloudservice implementeren in de staging-site soms mislukt met een toewijzingsfout resource als er al een bestaande implementatie in de productiesite?
Als een service in de cloud een implementatie in een sleuf heeft, wordt het hele cloudservice is vastgemaakt aan een specifieke cluster. Dit betekent dat als een implementatie al in de productiesite bestaat, een nieuwe staging-implementatie kan alleen worden toegewezen in hetzelfde cluster als de productiesite.

Toewijzingsfouten optreden wanneer het cluster waar uw cloudservice zich bevindt geen voldoende fysieke compute-resources om te voldoen aan de implementatieaanvraag.

Zie voor meer informatie over het oplossen van dergelijke toewijzingsfouten [Cloudservice toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Waarom wordt omhoog of uit een cloud service-implementatie soms leiden tot toewijzingsfout schalen?
Wanneer een service in de cloud wordt geïmplementeerd, wordt deze meestal vastgemaakt aan een specifieke cluster. Dit betekent dat nieuwe exemplaren binnen hetzelfde cluster omhoog/uit een bestaande cloudservice schalen moet toewijzen. Als het cluster heeft capaciteit bijna bereikt of de gewenste VM-instantiegrootte /-type niet beschikbaar is, kan het verzoek mislukken.

Zie voor meer informatie over het oplossen van dergelijke toewijzingsfouten [Cloudservice toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Waarom voor het implementeren van een service in de cloud in een affiniteitsgroep is het soms resulteert in toewijzingsfout?
Een nieuwe implementatie in een lege cloudservice kan worden toegewezen door de infrastructuur in een cluster in deze regio, tenzij de cloudservice is vastgemaakt aan een affiniteitsgroep. Implementaties naar dezelfde affiniteitsgroep bevinden worden geprobeerd in hetzelfde cluster. Als het cluster heeft capaciteit bijna bereikt, mislukken de aanvraag.

Zie voor meer informatie over het oplossen van dergelijke toewijzingsfouten [Cloudservice toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Waarom wijzigen van VM-grootte of voor het toevoegen van een nieuwe virtuele machine aan een bestaande cloudservice is het soms resulteert in toewijzingsfout?
De clusters in een datacenter mogelijk verschillende configuraties van machine-typen (bijvoorbeeld, een serie, Av2-serie, D-serie, Dv2-serie, G-serie, H-serie, enzovoort). Maar niet alle clusters moet altijd alle typen virtuele machines. Als u probeert een virtuele machine uit de D-serie toevoegen aan een cloudservice die al in een A-serie alleen-lezen-cluster is geïmplementeerd, zult u bijvoorbeeld een toewijzingsfout ervaren. Dit wordt ook gebeuren als u probeert te wijzigen dat VM-SKU-grootten (bijvoorbeeld van een A-serie overschakelen naar een D-serie).

Zie voor meer informatie over het oplossen van dergelijke toewijzingsfouten [Cloudservice toewijzingsfout: oplossingen](cloud-services-allocation-failures.md#solutions).

Om te controleren of de grootten die beschikbaar zijn in uw regio, Zie [Microsoft Azure: producten beschikbaar per regio](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Waarom enige tijd opnieuw uit met het implementeren van een cloudservice mislukken vanwege quota-limieten/beperkingen op mijn abonnement of de service?
Implementatie van een service in de cloud kan mislukken als de resources die nodig zijn om te worden toegewezen langer zijn dan de standaard- of maximumquotum voor uw service op het niveau van de regio/datacenter wordt toegestaan. Zie voor meer informatie, [Cloud Services beperkt](../azure-subscription-service-limits.md#cloud-services-limits).

U kunt de huidige gebruiksquotum ook bijhouden voor uw abonnement in de portal: Azure portal = > abonnementen = > \<abonnement van toepassing > = > 'Gebruik + quota'.

Resource-gebruik/verbruik-gerelateerde informatie kan ook worden opgehaald via de Azure Billing API's. Zie [Azure Resourcegebruik-API (Preview)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Hoe kan ik de grootte van een geïmplementeerde cloudservice-VM wijzigen zonder deze opnieuw te implementeren?
U kunt de VM-grootte van een geïmplementeerde cloudservice niet wijzigen zonder deze opnieuw te implementeren. De VM-grootte is ingebouwd in het CSDEF, kan alleen worden bijgewerkt met een opnieuw implementeren.

Zie voor meer informatie, [het bijwerken van een cloudservice](cloud-services-update-azure-service.md).

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>Waarom kan ik geen implementeren van Cloud-Services via Service Management API's of PowerShell bij het gebruik van Azure Resource Manager-Storage-account? 

Omdat de Service in de Cloud een Classic-resource die niet rechtstreeks compatibel is met het Azure Resource Manager-model is, kunt u deze niet koppelen met de Azure Resource Manager-opslagaccounts. Hier zijn enkele opties: 
 
- Implementeren via REST-API.

    Wanneer u via de Service Management REST-API implementeert, kan u de beperking omzeilen door een SAS-URL naar de blob-opslag, die is voor zowel klassieke als Azure Resource Manager-Storage-account geschikt op te geven. Meer informatie over de eigenschap 'PackageUrl' [hier](https://msdn.microsoft.com/library/azure/ee460813.aspx).  
- Implementeren via [Azure-portal](https://portal.azure.com).

    Dit werkt vanuit de [Azure-portal](https://portal.azure.com) wanneer de aanroep niet via een proxy/shim waarmee de communicatie tussen Azure Resource Manager en klassieke resources. 
 
## <a name="why-does-azure-portal-require-me-to-provide-a-storage-account-for-deployment"></a>Waarom vereist Azure-portal me voor een storage-account voor implementatie? 

Het pakket rechtstreeks naar de laag van de API management is geüpload in de klassieke portal en vervolgens de API-laag tijdelijk plaatst het pakket naar een interne storage-account.  Dit proces zorgt ervoor dat problemen met prestaties en schaalbaarheid omdat de API-laag niet ontworpen is om te worden van een bestand uploaden-service.  In de Azure-portal (Resource Manager-implementatiemodel) hebben we de tussentijdse stap van het eerste uploaden naar de API-laag, overgeslagen leidt tot sneller en betrouwbaarder implementaties. 

Als de kosten voor het is erg klein en u kunt hetzelfde opslagaccount opnieuw gebruiken voor alle implementaties. U kunt de [opslag kostencalculator](https://azure.microsoft.com/pricing/calculator/#storage1) om te bepalen van de kosten voor het uploaden van het servicepakket (CSPKG), de CSPKG downloaden en verwijder vervolgens de CSPKG. 
