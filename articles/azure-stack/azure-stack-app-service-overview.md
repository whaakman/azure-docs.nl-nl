---
title: App Service op Azure Stack-overzicht | Microsoft Docs
description: Overzicht van App Service op Azure Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: a638d5cdfbd3af46335cfb8e4970306534fc1c3b
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56445981"
---
# <a name="app-service-on-azure-stack-overview"></a>Overzicht App Service op Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure App Service in Azure Stack is een platform-as-a-service (PaaS) aanbieding van Microsoft Azure beschikbaar voor Azure Stack. De service kan uw interne of externe klanten te maken van web-API en Azure Functions-toepassingen voor elk platform of apparaat. Ze kunnen uw apps integreren met on-premises toepassingen en hun bedrijfsprocessen automatiseren. Azure Stack-cloudoperators kunnen apps van klanten uitvoeren op volledig beheerde virtuele machines (VM's), waarbij u kunt kiezen uit gedeelde VM-resources of toegewezen VM's.

Azure App Service kunt u bedrijfsprocessen automatiseren en hosten van cloud-API's. Als een geïntegreerde service kunt Azure App Service u diverse onderdelen, zoals websites, REST-API's en bedrijfsprocessen, in één oplossing combineert.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Waarom bieden Azure App Service in Azure Stack?

Hierna ziet u een paar belangrijke functies en mogelijkheden van App Service:

- **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, Node.js, Java, PHP en Python. U kunt ook Windows PowerShell en andere scripts of uitvoerbare bestanden uitvoeren op virtuele machines van App Service.
- **DevOps-optimalisatie**: Instellen van continue integratie en implementatie met GitHub-, lokale Git- of BitBucket. U kunt Verhoog updateniveaus via test- en faseringsomgevingen en beheer uw apps in App Service met behulp van Azure PowerShell of de platformoverschrijdende opdrachtregelinterface (CLI).
- **Visual Studio-integratie**: Specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van toepassingen.

## <a name="app-types-in-app-service"></a>App-typen in App Service

App Service biedt verschillende app-typen, die elk zijn bedoeld voor een specifieke workload:

- [Web-Apps](../app-service/overview.md) voor het hosten van websites en webtoepassingen.
- [API Apps](../app-service/overview.md) voor het hosten van REST-API's.
- Azure Functions voor het hosten van op gebeurtenissen gebaseerde, serverloze werkbelastingen.

Het woord *app* verwijst naar de hostresources die worden toegewezen aan het uitvoeren van een werkbelasting. Duurt *web-app* bijvoorbeeld u waarschijnlijk gewend bent aan denkt aan een web-app als de compute-resources en de toepassing die samen bieden functionaliteit aan een browser code. In App Service wordt een web-app de compute-resource die Azure Stack biedt voor het hosten van code van uw toepassing.

Uw toepassing kan bestaan uit meerdere App Service-apps van verschillende typen. Bijvoorbeeld, als uw toepassing uit een webfront-end bestaat en een REST-API terug beëindigen, kunt u:

- Beide (front-end en API) implementeren in één web-app
- Uw front-endcode implementeren in een web-app en uw back-endcode implementeren in een API-app.

   [ ![App Service-overzicht met gegevens te controleren](media/azure-stack-app-service-overview/image01.png "App Service-overzicht met gegevens te controleren") ](media/azure-stack-app-service-overview/image01.png#lightbox)

## <a name="what-is-an-app-service-plan"></a>Wat is een App Service-plan?

De resourceprovider App Service gebruikt dezelfde code die maakt gebruik van Azure Appservice en deelt daarom enkele algemene concepten toegelicht. In App Service, de prijzen container voor toepassingen is de naam de *App Service-plan*. Hiermee geeft u de set toegewezen virtuele machines die worden gebruikt voor het opslaan van uw apps. U kunt meerdere App Service-abonnementen hebben binnen een bepaald abonnement.

In Azure zijn er gedeelde en toegewijde werknemers. Een gedeelde worker ondersteunt het hosten van hoge dichtheid app met meerdere tenants en er is slechts één set gedeelde werknemers. Speciale servers worden gebruikt door slechts één tenant en zijn verkrijgbaar in drie grootten: klein, gemiddeld en groot. De behoeften van on-premises klanten kunnen niet altijd worden beschreven met behulp van deze voorwaarden. In App Service in Azure Stack kunnen resource provider beheerders definiëren de werkrolniveaus die ze beschikbaar wilt maken. Op basis van uw unieke behoeften van de host, kunt u meerdere sets met gedeelde werknemers of verschillende sets van specifieke werknemers definiëren. Met behulp van deze definities werknemer-laag, kunnen ze vervolgens definiëren hun eigen prijzen van voorraadeenheden.

## <a name="portal-features"></a>Portal-functies

App Service in Azure Stack maakt gebruik van dezelfde gebruikersinterface die gebruikmaakt van Azure App Service. Hetzelfde geldt voor de back-end. Sommige functies zijn echter uitgeschakeld in Azure Stack. De verwachtingen van de Azure-specifieke of de services waarvoor deze functies zijn momenteel niet beschikbaar in Azure Stack.

## <a name="next-steps"></a>Volgende stappen

- [Voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md)
- [Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md)

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md) en de [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md).
