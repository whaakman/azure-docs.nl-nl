---
title: 'App Service-Overzicht: Azure Stack | Microsoft Docs'
description: Overzicht van App Service op Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: cf2d65e7e2927aee99e533ea0bca0818f3ab98f6
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079154"
---
# <a name="app-service-on-azure-stack-overview"></a>Overzicht App Service op Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure App Service in Azure Stack is een platform-as-a-service (PaaS) aanbieding van Microsoft Azure beschikbaar voor Azure Stack. De service kan uw klanten - interne of externe - maken van web-API en Azure Functions-toepassingen voor elk platform of apparaat. Ze kunnen uw apps integreren met on-premises toepassingen en hun bedrijfsprocessen automatiseren. Azure Stack-cloud-operators kunnen klant apps uitvoeren op volledig beheerde virtuele machines (VM's), met hun eigen keuze aan gedeelde VM-resources of toegewezen virtuele machines.

Azure App Service kunt u voor het automatiseren van bedrijfsprocessen en hosting cloud-API's. Als een geïntegreerde service, Azure App Service kunt u diverse onderdelen, websites, RESTful-API's en bedrijfsprocessen--combineren in één oplossing.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Waarom bieden Azure App Service in Azure Stack?

Hierna ziet u een paar belangrijke functies en mogelijkheden van App Service:

- **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, Node.js, Java, PHP en Python. U kunt ook Windows PowerShell en andere scripts of uitvoerbare bestanden uitvoeren op virtuele machines van App Service.
- **DevOps-optimalisatie**: instellen van continue integratie en implementatie met GitHub-, lokale Git- of BitBucket. Verhoog updateniveaus via test- en faseringsomgevingen. Beheer uw apps in App Service met behulp van Azure PowerShell of de platformoverschrijdende opdrachtregelinterface (CLI).
- **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van toepassingen.

## <a name="app-types-in-app-service"></a>App-typen in App Service

App Service biedt verschillende app-typen, die elk zijn bedoeld voor een specifieke workload:

- [Web-Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) voor het hosten van websites en webtoepassingen.
- [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-apps-why-best-platform) voor het hosten van RESTful-API's.
- Azure Functions voor het hosten van op gebeurtenissen gebaseerde, serverloze werkbelastingen.

De word-app verwijst naar de hostresources die worden toegewezen aan het uitvoeren van een werkbelasting. Neem bijvoorbeeld een 'web-app': u ziet een web-app waarschijnlijk als de rekenresources en toepassingscode die samen functionaliteit bieden aan een browser. Maar in App Service een web-app de rekenresources die Azure Stack biedt voor het hosten van code van uw toepassing is.

Uw toepassing kan bestaan uit meerdere App Service-apps van verschillende typen. Bijvoorbeeld, als uw toepassing uit een webfront-end bestaat en een RESTful-API back-end, kunt u:

- Beide (front-end en API) implementeren in één web-app
- Uw front-endcode implementeren in een web-app en uw back-endcode implementeren in een API-app.

   ![App Service-overzicht met gegevens te controleren](media/azure-stack-app-service-overview/image01.png)

## <a name="what-is-an-app-service-plan"></a>Wat is een App Service-plan?

De resourceprovider App Service maakt gebruik van dezelfde code die gebruikmaakt van Azure App Service. Als gevolg hiervan worden enkele algemene concepten zijn het noemen waard waarin wordt beschreven. De prijzen container voor toepassingen heet in App Service, de App Service-plan. Hiermee geeft u de set toegewezen virtuele machines die worden gebruikt voor het opslaan van uw apps. U kunt meerdere App Service-abonnementen hebben binnen een bepaald abonnement.

In Azure zijn er gedeelde en toegewijde werknemers. Een gedeelde worker ondersteunt het hosten van hoge dichtheid app met meerdere tenants en er is slechts één set gedeelde werknemers. Speciale servers worden gebruikt door slechts één tenant en zijn verkrijgbaar in drie grootten: klein, gemiddeld en groot. De behoeften van on-premises klanten kunnen niet altijd worden beschreven met behulp van deze voorwaarden. In App Service in Azure Stack kunnen resource provider beheerders definiëren de werkrolniveaus die ze beschikbaar wilt maken. Op basis van uw unieke behoeften van de host, kunt u meerdere sets met gedeelde werknemers of verschillende sets van specifieke werknemers definiëren. Met behulp van deze definities werknemer-laag, kunnen ze vervolgens definiëren hun eigen prijzen van voorraadeenheden.

## <a name="portal-features"></a>Portal-functies

App Service in Azure Stack maakt gebruik van dezelfde gebruikersinterface die gebruikmaakt van Azure App Service, hetzelfde geldt voor de back-end. Sommige functies zijn echter zijn uitgeschakeld en worden niet werken in Azure Stack. De verwachtingen van de Azure-specifieke of de services waarvoor deze functies niet op dit moment beschikbaar zijn in Azure Stack.

## <a name="next-steps"></a>Volgende stappen

- [Voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md)
- [Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md)

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md) en de [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md).
