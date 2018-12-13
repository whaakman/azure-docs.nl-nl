---
title: Aanbieding voor Azure Application | Microsoft Docs
description: Overzicht van het proces voor het publiceren van een Azure-toepassing aanbieden op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: pbutlerm
ms.openlocfilehash: 63b7ee4e0d9cb9d0d1f26119fe73573b124d04e8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196827"
---
# <a name="azure-application-offer"></a>Aanbieding voor Azure-toepassing

Deze sectie wordt uitgelegd hoe u een nieuwe Azure-toepassing-aanbieding publiceert naar de Microsoft <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>.
|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Elke Azure-toepassing bevat een Azure Resource Manager-sjabloon die de technische activa die worden gebruikt door de toepassing, die normaal gesproken een of meer virtuele machines en andere ondersteunende Azure - of Web-services op basis van bevat definieert. | ![Pictogram van de Azure-apps](./media/azureapp-icon1.png)  |

## <a name="benefits"></a>Voordelen

De voordelen van de aanbieding van uw toepassingen op een Microsoft-marketplace zijn onder andere:

* 100 miljoen Azure Active Directory-gebruikers voor Office 365 en Dynamics 365 wordt bereikt.

* Uitbreiden van uw verkoopteam: doelgroep uit zakelijke gebruikers over de hele wereld en krijgen een verkoopkanaal dat praat met eindgebruikers, bij het genereren van leads en gesprekken met nieuwe klanten initieert tussen verschillende industrieën.

* Inzichten verkrijgen: inzicht in hoe uw app wordt uitgevoerd op AppSource, wat goed werkt en hoe u voor het verder verbeteren van uw verkoop procedures wordt gedeeld.

## <a name="types-of-azure-applications"></a>Typen Azure-toepassingen

Er zijn twee soorten Azure-toepassingen: een beheerde toepassing en een oplossingssjabloon. Hoewel vergelijkbaar, zijn er enkele belangrijke verschillen.

### <a name="solution-template"></a>Oplossingssjabloon

Sjablonen voor oplossingen zijn een van de belangrijkste manieren om een oplossing op Marketplace publiceert. Dit aanbiedingtype wordt gebruikt wanneer uw oplossing extra implementatie en automatisering van de configuratie meer dan één virtuele machine (VM vereist). U kunt automatiseren met het leveren van meer dan één virtuele machine met behulp van een oplossingssjabloon. Dit omvat het inrichten van netwerk- en opslagresources om complexe IaaS-oplossingen te bieden. Zie voor een overzicht van de oplossing sjabloonvereisten en de facturering van maandabonnementen [Azure-toepassingen: oplossingssjablonen](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

### <a name="managed-application"></a>Beheerde toepassing

Een beheerde toepassing is vergelijkbaar met een oplossingssjabloon in de Microsoft Azure Marketplace, met één belangrijk verschil. In een beheerde toepassing worden de resources geïmplementeerd vanuit een resourcegroep die wordt beheerd door de uitgever van de app. De resourcegroep is opgenomen in het abonnement van de consument, maar een identiteit in de tenant van de uitgever heeft toegang tot de resourcegroep. De uitgever bepaalt de kosten voor de voortdurende ondersteuning van de oplossing. Gebruik Azure beheerde toepassingen eenvoudig bouwen en leveren van volledig beheerde, kant-en toepassingen om uw klanten.

U kunt ook beheerde toepassingen in een Servicecatalogus bieden naast de Marketplace. De servicecatalogus is een interne catalogus van goedgekeurde oplossingen voor gebruikers in een organisatie. U kunt de catalogus gebruiken om te voldoen aan de standaarden van de organisatie terwijl oplossingen voor groepen in een organisatie. Werknemers gebruiken de catalogus om eenvoudig toepassingen te vinden die worden aanbevolen en zijn goedgekeurd door hun IT-afdelingen.

Zie voor meer informatie over de voordelen en typen van beheerde toepassingen, de [Azure beheerde toepassingen-overzicht](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="publishing-overview"></a>Overzicht van publiceren

De volgende video [gebouw Oplossingssjablonen en beheerde toepassingen voor Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603), wordt een overzicht over het ontwerpen van een Azure Resource Manager-sjabloon voor het definiëren van een Azure-toepassing-oplossing en klik vervolgens hoe u vervolgens de app-aanbieding publiceren op Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]

## <a name="publishing-process-workflow"></a>Werkstromen voor publicatie van proces

Het volgende diagram toont het proces op hoog niveau voor het publiceren van een aanbieding van Azure-toepassing.

![Werkstroom voor de aanbieding publiceren](./media/new-offer-process.png)

## <a name="offer-components"></a>Onderdelen van de aanbieding

In deze sectie geeft een overzicht van de elementen van de publicatie van een beheerder toepassingsbieding en is bedoeld als richtlijn voor de uitgever in de Azure Marketplace. De publicatie is onderverdeeld in de volgende hoofdonderdelen: 

* [Vereisten](./cpp-prerequisites.md) -geeft een lijst van de technische en zakelijke vereisten voor het maken of een beheerder toepassingsbieding publiceren. 
* [Maken van de aanbieding](./cpp-create-offer.md) -bevat de stappen die vereist voor het maken van een beheerde toepassing aanbieding vermelding met behulp van de Cloud Partner-Portal. 
* [De aanbieding publiceren](./cpp-publish-offer.md)-wordt beschreven hoe u om in te dienen de aanbieding voor publicatie naar de Azure Marketplace.

## <a name="steps-in-the-publishing-process"></a>Stappen in het publicatieproces

De stappen op hoog niveau voor het publiceren van een aanbieding van Azure-toepassing zijn:

1. Maken van de aanbieding - bieden gedetailleerde informatie over de aanbieding. Deze informatie omvat: de beschrijving van de aanbieding, marketingmaterialen, informatie over ondersteuning en assetspecificaties.

2. Maken van de zakelijke en technische activa - maken van de bedrijfsmiddelen (juridische documenten en marketingmaterialen) en de technische activa voor de gekoppelde oplossing.

3. Maken van de SKU - maken van de SKU('s) die zijn gekoppeld aan de aanbieding. Een unieke SKU is vereist voor elke afbeelding die u van plan bent om te publiceren.

4. Certificeren en publiceren van de aanbieding - nadat de aanbieding en de technische activa zijn voltooid, kunt u de aanbieding indienen. Deze verzending van het publicatieproces te starten. Tijdens dit proces wordt is de oplossing getest, gevalideerd, gecertificeerd, klikt u vervolgens "meteen live' op Azure Marketplace.

## <a name="next-steps"></a>Volgende stappen

Voordat u deze stappen, moet u voldoen aan de [technische en zakelijke vereisten](./cpp-prerequisites.md) voor het publiceren van een beheerde toepassing in de Microsoft Azure Marketplace.