---
title: Handleiding voor het maken van een oplossingssjabloon voor Marketplace | Microsoft Docs
description: Gedetailleerde instructies over het maken, verklaart en implementeren van een Multi-VM-installatiekopie-oplossingssjabloon voor aankoop op Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: 0e1c049596b5231ad72590fc17a73cd5aa457e4d
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42054930"
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Handleiding voor het maken van een oplossingssjabloon voor Azure Marketplace
Na het voltooien van stap 1, [van accountaanmaking en registratie][link-acct-creation], we u op het maken van een sjabloon op Azure-compatibele oplossing bij begeleide [technische vereisten voor het maken van een oplossingssjabloon](marketplace-publishing-solution-template-creation-prerequisites.md). Nu we u stapsgewijs door de stappen begeleidt voor het maken van een oplossingssjabloon voor meerdere virtuele machines op de [Portal voor publiceren] [ link-pubportal] voor de Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Uw aanbieding van de sjabloon oplossing maken in de Portal voor publiceren
Ga naar [ https://publish.windowsazure.com ](http://publish.windowsazure.com). Tijdens het aanmelden voor de eerste keer de [Portal voor publiceren](https://publish.windowsazure.com/), gebruiken hetzelfde account gebruiken met die van uw bedrijf verkoper profiel is geregistreerd. U kunt later een werknemer van uw bedrijf toevoegen als een co-beheerder in de Portal voor publiceren.

### <a name="1-select-solution-templates"></a>1. Selecteer "Oplossingssjablonen"
  ![tekenen][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Een nieuwe oplossingssjabloon maken
  ![tekenen][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Beginnen met topologieën
Een oplossingssjabloon is een bovenliggende topologie voor alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in één aanbieding/oplossingssjabloon. Wanneer een aanbieding wordt doorgegeven voor fasering, wordt deze doorgegeven met alle bijbehorende topologieën. De volgende stappen voor het definiëren van uw aanbieding:     

* Maken van een topologie: "ID" is doorgaans de naam van de topologie van de oplossingssjabloon. De id van de topologie wordt gebruikt in de URL, zoals hieronder wordt weergegeven:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Azure-Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Voeg een nieuwe versie toe.

### <a name="4-get-your-topology-versions-certified"></a>4. De versies van uw topologie certified ophalen
Upload een zipbestand dat alle vereiste bestanden voor het inrichten van deze specifieke versie van de topologie bevat. Dit zip-bestand moet bevatten het volgende:

* *mainTemplate.json* en *createUiDefinition.json* bestand in de hoofdmap.
* Alle gekoppelde sjablonen en alle vereiste scripts.

  > [!TIP]
  > Terwijl uw ontwikkelaars werkt over het maken van de oplossing sjabloon topologieën en hun aandacht gecertificeerd, het bedrijf kunnen marketing en/of juridische afdelingen van uw bedrijf worden gebruikt voor de marketing en geldige inhoud.
  >
  >

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw oplossingssjabloon hebt gemaakt en het zip-bestand wordt geüpload,. Volg de instructies in de [marketing content voor Marketplace](marketplace-publishing-push-to-staging.md) voordat u de aanbieding voor fasering pusht. Als u wilt zien van de volledige set marketplace artikelen te publiceren, gaat u naar [aan de slag: een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md).

U mogelijk ook geïnteresseerd in deze artikelen:

* VM-installatiekopieën: [About VM Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM-extensies: [Azure VM-extensies en functies](../virtual-machines/extensions/features-windows.md)
* Azure Resource Manager: [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) en [eenvoudige sjabloonvoorbeelden](https://github.com/rjmax/ArmExamples)
* Storage-account beperkt: [bewaken voor de beperking van de Storage-Account](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) en [Premium-opslag](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
