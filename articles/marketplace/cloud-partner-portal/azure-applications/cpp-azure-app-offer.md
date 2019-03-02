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
ms.date: 02/06/2019
ms.author: pbutlerm
ms.openlocfilehash: 6d86d4c03a41834322c98438958c0b9e87e6025c
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57213515"
---
# <a name="azure-application-offer"></a>Aanbieding voor Azure-toepassing

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| <div class="body"> In deze sectie wordt uitgelegd hoe u een nieuwe aanbieding voor Azure-toepassing te publiceren de [Azure Marketplace](https://azuremarketplace.microsoft.com).  Elke Azure-toepassing bevat een Azure Resource Manager-sjabloon die de technische activa die worden gebruikt door de toepassing, die normaal gesproken een of meer virtuele machines en andere ondersteunende Azure - of Web-services op basis van bevat definieert. Alle aanbiedingen voor Azure-app-beveiliging via moeten inschakelen [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).  </div> | ![Pictogram van de Azure-apps](./media/azureapp-icon1.png)  |

## <a name="publishing-overview"></a>Overzicht van publiceren

De volgende video [gebouw Oplossingssjablonen en beheerde toepassingen voor Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603), bevat een inleiding: wat bieden typen beschikbaar zijn, wat technische activa zijn vereist, over het ontwerpen van een Azure Resource Manager sjabloon, ontwikkelen en testen van de app-gebruikersinterface, hoe de app-aanbieding en het proces voor de app kunt publiceren.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


## <a name="types-of-azure-applications"></a>Typen Azure-toepassingen

Er zijn twee soorten Azure-toepassingen: beheerde toepassingen en -oplossingssjablonen. 

- Sjablonen voor oplossingen zijn een van de belangrijkste manieren om een oplossing op Marketplace publiceert. Dit aanbiedingtype wordt gebruikt wanneer uw oplossing extra implementatie en automatisering van de configuratie meer dan één virtuele machine (VM vereist). U kunt automatiseren met het leveren van meer dan één virtuele machine met behulp van een oplossingssjabloon. Deze automatisering omvat het inrichten van netwerk- en opslagresources om complexe IaaS-oplossingen te bieden. Zie voor een overzicht van de oplossing sjabloonvereisten en de facturering van maandabonnementen [Azure-toepassingen: oplossingssjablonen](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

- Beheerde toepassingen zijn vergelijkbaar met oplossingssjablonen, met één belangrijk verschil. In een beheerde toepassing worden de resources geïmplementeerd vanuit een resourcegroep die wordt beheerd door de uitgever van de app. De resourcegroep is opgenomen in het abonnement van de consument, maar een identiteit in de tenant van de uitgever heeft toegang tot de resourcegroep. De uitgever bepaalt de kosten voor de voortdurende ondersteuning van de oplossing. Gebruik Azure beheerde toepassingen eenvoudig bouwen en leveren van volledig beheerde, kant-en toepassingen om uw klanten.

U kunt ook beheerde toepassingen in een Servicecatalogus bieden naast de Azure Marketplace. De servicecatalogus is een interne catalogus van goedgekeurde oplossingen voor gebruikers in een organisatie. U kunt de catalogus gebruiken om te voldoen aan de standaarden van de organisatie terwijl oplossingen voor groepen in een organisatie. Werknemers gebruiken de catalogus om eenvoudig toepassingen te vinden die worden aanbevolen en zijn goedgekeurd door hun IT-afdelingen.

>[!Note]
>Cloud Solution Providers (CSP) partner kanaal aanmelden is nu beschikbaar.  Raadpleeg [Cloud Solution Providers](../../cloud-solution-providers.md) voor meer informatie over marketingcontactpersonen van uw aanbieding via de Microsoft CSP partner kanalen.

Zie voor meer informatie over de voordelen en typen van beheerde toepassingen, de [Azure beheerde toepassingen-overzicht](https://docs.microsoft.com/azure/managed-applications/overview).


## <a name="publishing-process-workflow"></a>Werkstromen voor publicatie van proces

Het volgende diagram toont het proces op hoog niveau voor het publiceren van een aanbieding van Azure-toepassing.

![Werkstroom voor de aanbieding publiceren](./media/new-offer-process.png)

De stappen op hoog niveau voor het publiceren van een aanbieding van Azure-toepassing zijn:

0. Voldoen aan de [vereisten](./cpp-prerequisites.md) - (niet weergegeven) Controleer of dat u voldoet aan de zakelijke en technische vereisten voor het publiceren van een Azure-app naar de Azure Marketplace. 

1. [Maken van de aanbieding](./cpp-create-offer.md) -bieden gedetailleerde informatie over de aanbieding. Deze informatie omvat: de beschrijving van de aanbieding, marketingmaterialen, informatie over ondersteuning en assetspecificaties.

2. [Maken of bestaande zakelijke en technische zaken verzamelen](./cpp-create-technical-assets.md) -maken van de bedrijfsmiddelen (juridische documenten en marketingmaterialen) en de technische activa voor de gekoppelde oplossing.

3. [Maken van de SKU](./cpp-skus-tab.md) -maken van de SKU('s) die zijn gekoppeld aan de aanbieding. Een unieke SKU is vereist voor elke afbeelding die u van plan bent om te publiceren.

4. Certificeren en [de aanbieding publiceren](./cpp-publish-offer.md) -nadat de aanbieding en de technische activa zijn voltooid, kunt u de aanbieding indienen. Deze verzending van het publicatieproces te starten. Tijdens dit proces wordt is de oplossing getest, gevalideerd, gecertificeerd, klikt u vervolgens "meteen live' op Azure Marketplace.

## <a name="next-steps"></a>Volgende stappen

Voordat u deze stappen, moet u voldoen aan de [technische en zakelijke vereisten](./cpp-prerequisites.md) voor het publiceren van een beheerde toepassing in de Microsoft Azure Marketplace.
