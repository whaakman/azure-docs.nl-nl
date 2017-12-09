---
title: 'Overzicht van App Service: Azure Stack | Microsoft Docs'
description: Overzicht van App-Service op Azure Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: fd2d355b2556faddb06acf2998b54ffcc9aa7919
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="app-service-on-azure-stack-overview"></a>Overzicht App Service op Azure Stack

Azure App Service op Azure-Stack is een platform-as-a-service (PaaS)-oplossing van Microsoft Azure beschikbaar zijn voor Azure-Stack. De service kunt u uw klanten - interne of externe - maken web-API en Azure Functions toepassingen voor alle platforms en apparaten. Ze kunnen uw apps integratie met on-premises toepassingen en hun bedrijfsprocessen automatiseren. Azure Stack cloudoperators kunnen klant apps uitvoeren op volledig beheerde virtuele machines (VM's) met hun keuze uit gedeelde VM-resources of toegewezen virtuele machines.

Azure App Service bevat mogelijkheden voor het automatiseren van bedrijfsprocessen en hosten van cloud-API's. Als een geïntegreerde service, Azure App Service kunt u diverse onderdelen--websites, RESTful-API's en bedrijfsprocessen--in één oplossing.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Waarom Azure App Service op Azure-Stack bieden?

Hierna ziet u een paar belangrijke functies en mogelijkheden van App Service:
- **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, Node.js, Java, PHP en Python. U kunt ook Windows PowerShell en andere scripts of uitvoerbare programma's uitvoeren op virtuele machines van App Service.
- **DevOps-optimalisatie**: continue integratie en implementatie met GitHub, Git lokale of BitBucket instellen. Verhoog updateniveaus via test- en faseringsomgevingen. Beheer uw apps in App Service met behulp van Azure PowerShell of de platformoverschrijdende opdrachtregelinterface (CLI).
- **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van toepassingen.

## <a name="app-types-in-app-service"></a>App-typen in App Service

App Service biedt verschillende typen Apps, die zijn bedoeld voor een specifieke werkbelasting:

- [Web-Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) voor het hosten van websites en webtoepassingen.
- [API-Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-apps-why-best-platform) voor het hosten van RESTful-API's.
- Azure Functions voor het hosten van gebeurtenissen, zonder server werkbelastingen.

De word-app verwijst naar de hostresources die specifiek zijn voor het uitvoeren van een werkbelasting. Neem bijvoorbeeld een 'web-app': u ziet een web-app waarschijnlijk als de rekenresources en toepassingscode die samen functionaliteit bieden aan een browser. Maar in App Service een web-app is de rekenresources die Azure-Stack biedt voor het hosten van uw toepassingscode.

Uw toepassing kan bestaan uit meerdere App Service-apps van verschillende typen. Bijvoorbeeld, als uw toepassing uit een webfront-end bestaat en een RESTful-API back-end, kunt u:
- Beide (front-end en API) implementeren in één web-app
- Uw front-endcode implementeren in een web-app en uw back-endcode implementeren in een API-app.

   ![](media/azure-stack-app-service-overview/image01.png)

## <a name="what-is-an-app-service-plan"></a>Wat is een App Service-plan?

De provider van de bron-App Service gebruikt dezelfde code die gebruikmaakt van Azure App Service. Als gevolg hiervan worden enkele algemene concepten waard beschrijven zijn. De prijscategorie container voor toepassingen wordt het App Service-abonnement genoemd in App Service. Hiermee geeft u de reeks toegewezen virtuele machines die worden gebruikt om uw apps op te slaan. U kunt meerdere App Service-abonnementen hebben binnen een bepaald abonnement.

In Azure zijn er gedeelde en specifieke werknemers. Een gedeelde worker ondersteunt high-density multitenant app hosten en er is slechts één set met gedeelde werknemers. Specifieke servers worden gebruikt door slechts één tenant en zijn beschikbaar in drie grootten: klein, gemiddeld en groot. De behoeften van klanten lokale kunnen niet altijd worden beschreven met behulp van deze voorwaarden. In App Service op Azure-Stack kunnen resource provider beheerders definiëren de worker-lagen die ze beschikbaar wilt maken. Op basis van uw unieke behoeften hosting, kunt u meerdere sets met gedeelde werknemers of verschillende sets van specifieke werknemers definiëren. Met behulp van deze definities worker-laag, kunnen ze vervolgens definiëren hun eigen prijzen SKU's.

## <a name="portal-features"></a>Portal-onderdelen

App-Service op Azure-Stack gebruikt dezelfde gebruikersinterface die Azure App Service gebruikt, zoals geldt voor de back-end. Sommige functies zijn uitgeschakeld en niet werken in Azure-Stack. De verwachtingen van het Azure-specifieke of de services waarvoor deze functies zijn nog niet beschikbaar in Azure-Stack.

## <a name="next-steps"></a>Volgende stappen


- [Voordat u aan de slag met App-Service op Azure-Stack](azure-stack-app-service-before-you-get-started.md)
- [Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md)

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), bijvoorbeeld de [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md) en de [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md).
