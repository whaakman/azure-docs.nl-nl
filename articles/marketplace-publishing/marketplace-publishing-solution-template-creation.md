---
title: Handleiding voor het maken van een oplossingssjabloon voor de Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken, certificeren en implementeren van een oplossingssjabloon Multi-VM-installatiekopie voor aanschaffen op Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: 675316b97e821a81ca4946d57a76d7bce978361b
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Handleiding voor het maken van een oplossingssjabloon voor Azure Marketplace
Na het voltooien van stap 1, [accountaanmaking en registratie][link-acct-creation], wij u op het maken van een Azure-compatibele oplossingssjabloon op begeleide [technische vereisten voor het maken van een oplossingssjabloon](marketplace-publishing-solution-template-creation-prerequisites.md). Nu we u stapsgewijs door de stappen begeleidt voor het maken van een oplossingssjabloon voor meerdere virtuele machines op de [Publishing Portal] [ link-pubportal] voor Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Maken van uw oplossing sjabloon aanbieding in de Portal publiceren
Ga naar [https://publish.windowsazure.com](http://publish.windowsazure.com). Als u zich aanmeldt voor het eerst naar de [Publishing Portal](https://publish.windowsazure.com/), gebruik hetzelfde account gebruiken met de verkoper-profiel van uw bedrijf is geregistreerd. Later kunt u alle werknemers van uw bedrijf toevoegen als een co-beheerder in de Portal voor publiceren.

### <a name="1-select-solution-templates"></a>1. Selecteer "Oplossingssjablonen"
  ![tekenen][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Maak een nieuwe oplossingssjabloon
  ![tekenen][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Beginnen met topologieën
Een oplossingssjabloon is een bovenliggende topologie voor alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in één aanbieding/oplossingssjabloon. Wanneer een aanbieding wordt doorgegeven voor fasering, wordt deze doorgegeven met alle bijbehorende topologieën. Volg onderstaande stappen voor het definiëren van uw aanbieding:     

* Een topologie maakt: "Topologie id" is doorgaans de naam van de topologie voor de oplossingssjabloon. De topologie-id wordt gebruikt in de URL, zoals hieronder wordt weergegeven:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}
* Voeg een nieuwe versie toe.

### <a name="4-get-your-topology-versions-certified"></a>4. De versies van uw topologie gecertificeerd ophalen
Upload een zipbestand dat alle vereiste bestanden voor het inrichten van die bepaalde versie van de topologie bevat. Dit zipbestand moet bevatten het volgende:

* *mainTemplate.json* en *createUiDefinition.json* -bestand in de hoofdmap.
* Alle gekoppelde sjablonen en alle vereiste scripts.

  > [!TIP]
  > Terwijl de ontwikkelaars werkt over het maken van de oplossing sjabloon topologieën en hen gecertificeerd, het bedrijf zijn, kunnen de inhoud marketing- en juridische marketing en/of juridische afdelingen van uw bedrijf werken.
  >
  >

## <a name="next-steps"></a>Volgende stappen
Nu u uw oplossingssjabloon gemaakt en dat het zip-bestand wordt geüpload, volg de instructies in de [Marketplace marketing inhoud handleiding](marketplace-publishing-push-to-staging.md) voordat u de aanbieding voor fasering. De volledige set van marketplace artikelen publiceren, Ga naar [aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md).

U is mogelijk ook geïnteresseerd in deze verwante artikelen:

* VM-installatiekopieën: [over installatiekopieën van virtuele machines in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM-extensies: [VM-Agent en een overzicht van de VM-extensies](https://msdn.microsoft.com/library/azure/dn832621.aspx) en [Azure VM-extensies en functies](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* Azure Resource Manager: [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md) en [eenvoudige sjabloon voorbeelden](https://github.com/rjmax/ArmExamples)
* Storage-account beperkt: [bewaken voor beperking van de Storage-Account](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) en [Premium-opslag](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
