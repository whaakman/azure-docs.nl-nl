---
title: Azure en AppSource-Marketplace-aanbiedingen | Microsoft Docs
description: Het maken en beheren van aanbiedingen van de Azure- en marktplaatsen van AppSource
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pbutlerm
ms.openlocfilehash: f13d49fde7f0e40f6dcb026fcb20cb11c028c64b
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100879"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure en AppSource-Marketplace-aanbiedingen

In dit eerste deel van deze sectie bevat de algemene bewerkingen gebruikt voor het maken en beheren van aanbiedingen voor Azure en AppSource-marktplaatsen.  In dit gedeelte kunt de achtergrond moet u begrijpen voor het beheren van specifieke aanbiedingstypen, evenals de technische informatie die geldt voor alle typen bieden.  De meeste van deze sectie bevat gedetailleerde instructies voor het maken en beheren van specifieke aanbiedingstypen.  

De volgende video introduceert de verschillende mogelijkheden en verschillende aanbiedingen die beschikbaar zijn in Azure Marketplace of AppSource.  Het behandelt ook belangrijke technische en zakelijke aspecten van een toepassing of service in deze marktplaatsen publiceren.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Het bouwen van Apps en Services voor Azure Marketplace en AppSource - Build 2018**

Zie voor meer informatie over deze marktplaatsen, [Azure Marketplace en AppSource publicatiehandleiding voor](../marketplace-publishers-guide.md).


## <a name="common-offer-operations"></a>Algemene bewerkingen van de aanbieding

Het proces voor het maken van een nieuwe aanbieding verschilt aanzienlijk aanbieding typen, bijvoorbeeld een [aanbieding van Azure-toepassing](./azure-applications/cpp-azure-app-offer.md) en een [advies service-aanbieding](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md).  Daarentegen veel van de andere bewerkingen uitvoeren op een aanbieding in de [Cloud Partner-Portal](https://cloudpartner.azure.com) vrij zijn gestandaardiseerd op aanbiedingstypen.  Deze algemene bewerkingen, met inbegrip van publiceren, status weergeven, bijwerken en verwijderen, worden behandeld in de sectie [beheren van aanbiedingen](./manage-offers/cpp-manage-offers.md)


## <a name="test-drive"></a>Test Drive

*U kunt uitproberen* is een marketplace-functie waarmee klanten een demonstratie "proberen voordat u kopen" optie voor elke aanbieding dus ingeschakeld.  De Test Drive-mogelijkheid is beperkt tot de volgende typen: [Azure-toepassingen](./azure-applications/cpp-azure-app-offer.md), [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md), [Dynamics 365 voor Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md), [Dynamics 365 voor Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md), [ SaaS-toepassingen](./saas-app/cpp-saas-offer.md), en [virtuele machines](./virtual-machine/cpp-virtual-machine-offer.md).  Deze mogelijkheid is vereist voor de uitgever om een Test Drive-sjabloon, die is afgestemd op de aanbieding te maken.  Zie voor meer informatie de sectie [Test Drive](../cloud-partner-portal-orig/what-is-test-drive.md).

Kunt u de bestaande marketplace-aanbiedingen met Test Drive demonstraties door toe te passen de [test drive filter](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=test-drive). 


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Marketplace en AppSource bieden

De volgende tabel bevat de huidige aanbieding die worden ondersteund door de [Cloud Partner-Portal](https://cloudpartner.azure.com).  Voor elk aanbiedingtype geeft de marketplace(s) waar de aanbieding kan worden weergegeven, evenals een algemene beschrijving van de aanbieding oplossing-technologie.

|                Type aanbieding                |  Marketplace  |   Description                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure-toepassing](./azure-applications/cpp-azure-app-offer.md) | Azure | Oplossing bestaat uit een of meer virtuele machines (VM's), optionele aangepaste Azure code geïmplementeerd via een Azure Resource Manager-sjabloon.  Implementatie kan door de klant via een oplossingssjabloon of beheerd door de uitgever. Dit type wordt gebruikt om u te bieden meer flexibiliteit dan het aanbiedingtype opgegeven virtuele machine.  |
| [Consulting Services](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Beide | Microsoft gekwalificeerde consultants kunnen hun domeinspecifieke services op Azure Marketplace of AppSource vermelden.  Hun ervaring helpt klanten hun problemen evalueren en het maken en implementeren van de juiste oplossingen om te voldoen aan hun zakelijke doelstellingen.  |
| [Container](./containers/cpp-containers-offer.md)  | Azure | Oplossing is een Docker-containerinstallatiekopie ingericht als een service op basis van Kubernetes of Azure Container instances. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Een pakket die uitgebreider is dan deze enterprise resourceplanning (ERP) en de business management-systeem. |
| [Dynamics 365 voor Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Een pakket dat de klant is een uitbreiding resource klantrelatiebeheer (CRM) systeem, via de sales, service, project-service en veld-servicemodules.  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Een pakket dat deze enterprise resource planning (ERP-)-service die ondersteuning biedt voor geavanceerde is een uitbreiding voor Financiën, bewerkingen, productie en toeleveringsbeheer. |
| [IoT Edge-module](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Een compatibel is met Docker-container die wordt uitgevoerd op een IoT Edge-apparaat.  Het bevat kleine rekenkundige modules die gebruikmaken van een combinatie van aangepaste code, andere Azure-services en services 3rd derden. |
| [Power BI App](./power-bi/cpp-power-bi-offer.md) | AppSource | Een pakket die gebruikmaakt van gegevensstromen verbinding te maken van rapporten en dashboards met gegevens in de opslag van veelgebruikte gegevens. |
| [SaaS-app](./saas-app/cpp-saas-offer.md) | Azure | Oplossing is een software-as-a-service-abonnement, beheerd door de uitgever, welke gebruikers zich aanmelden via een aangepaste interface die gebruikmaakt van Azure Active Directory. |
| [Virtuele machine](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Oplossing is opgenomen in een enkele virtuele machine is geïmplementeerd op van de klant-abonnement.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Zie voor meer informatie, [Publicatiehandleiding voor door het aanbiedingtype](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Volgende stappen

U leert over de algemene bewerkingen die u op de marketplace-aanbiedingen en hun algemene technische kenmerken en elementen in het onderwerp uitvoeren kunt [beheren van aanbiedingen](./manage-offers/cpp-manage-offers.md).
