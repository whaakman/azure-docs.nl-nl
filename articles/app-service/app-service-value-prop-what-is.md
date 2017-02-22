---
title: Azure App Service voor web-apps, mobiele apps en API-apps | Microsoft Docs
description: Lees hoe u met Azure App Service web-apps en mobiele apps kunt ontwikkelen, implementeren en beheren.
keywords: app service, azure app service, kosten app service, schaal, schaalbaar, implementatie app, implementatie azure app, paas, platform as a service, website, web site, web, azure mobiel
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
translationtype: Human Translation
ms.sourcegitcommit: 40dd75832302d7d88e852e2ea93821750675607e
ms.openlocfilehash: 4deb60c25bf13d1f31b58f002a7edea0672eca25


---
# <a name="what-is-azure-app-service"></a>Wat is Azure App Service?
*App Service* is een [Platform as a Service](https://en.wikipedia.org/wiki/Platform_as_a_service)-product (PaaS) van Microsoft Azure. Maak web-apps en mobiele apps voor alle platforms en apparaten. Integreer uw apps met SaaS-oplossingen, maak verbinding met on-premises toepassingen en automatiseer uw bedrijfsprocessen. Met Azure worden uw apps uitgevoerd op volledig beheerde virtuele machines (VM's), waarbij u kunt kiezen uit gedeelde VM-resources of toegewezen VM's.

App Service bevat de functionaliteit voor web-apps en mobiele apps die eerder afzonderlijk verkrijgbaar waren in Azure Websites en Azure Mobile Services. Deze service bevat ook nieuwe mogelijkheden voor het automatiseren van bedrijfsprocessen en het hosten van cloud-API's. Omdat App Service één geïntegreerde service is, kunt u diverse onderdelen, waaronder websites, back-ends van mobiele apps, RESTful API's en bedrijfsprocessen, samenvoegen in één oplossing.

De volgende video van 4 minuten bevat een korte uitleg van de relatie van App Service met eerdere Azure-producten en van nieuwe functionaliteit.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Waarom App Service gebruiken?
Hierna ziet u een paar belangrijke functies en mogelijkheden van App Service:

* **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, Node.js, Java, PHP en Python. U kunt ook [Windows PowerShell en andere scripts of uitvoerbare bestanden](../app-service-web/web-sites-create-web-jobs.md) uitvoeren op virtuele machines van App Service.
* **DevOps-optimalisatie**: stel [continue integratie en implementatie](../app-service-web/app-service-continuous-deployment.md) in met Visual Studio Team Services, GitHub of BitBucket. Verhoog updateniveaus via [test- en faseringsomgevingen](../app-service-web/web-sites-staged-publishing.md). Voer [A/B-tests](../app-service-web/app-service-web-test-in-production-get-start.md) uit. Beheer uw apps in App Service met [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de [platformoverschrijdende opdrachtregelinterface (CLI)](../xplat-cli-install.md).
* **Globale schaling met hoge beschikbaarheid**: u kunt handmatig of automatisch [omhoog](../app-service-web/web-sites-scale.md) schalen of [uit](../monitoring-and-diagnostics/insights-how-to-scale.md)schalen. U kunt uw apps overal in de globale datacenterinfrastructuur van Microsoft hosten; de [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) van App Service belooft hoge beschikbaarheid.
* **Verbindingen met SaaS-platforms en on-premises gegevens**: u kunt kiezen uit meer dan 50 [connectors](../connectors/apis-list.md) voor bedrijfssystemen (zoals SAP, Siebel en Oracle), SaaS-services (zoals Salesforce en Office 365) en internetservices (zoals Facebook en Twitter). Toegang tot on-premises gegevens met [hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md).
* **Beveiliging en naleving**: App Service voldoet aan de vereisten van [ISO, SOC en PCI](https://www.microsoft.com/TrustCenter/).
* **Toepassingssjablonen**: u kunt kiezen uit een uitgebreide lijst met sjablonen in [Azure Marketplace](https://azure.microsoft.com/marketplace/) waarmee u een wizard kunt gebruiken voor de installatie van populaire open-source software zoals WordPress, Joomla en Drupal.
* **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van apps en het opsporen van fouten.

## <a name="app-types-in-app-service"></a>App-typen in App Service
App Service bevat verschillende *app-typen*, die zijn bedoeld voor specifieke typen werkbelastingen:

* [**Web Apps**](../app-service-web/app-service-web-overview.md): voor het hosten van websites en webtoepassingen.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md): voor het hosten van de back-end van mobiele apps.
* [**API-apps**](../app-service-api/app-service-api-apps-why-best-platform.md): voor het hosten van RESTful-API's.
* [**Logische apps**](../logic-apps/logic-apps-what-are-logic-apps.md): voor het automatiseren van bedrijfsprocessen en het integreren van systemen en gegevens tussen clouds zonder code te schrijven.

Het woord *app* verwijst hier naar de hostresources die worden toegewezen voor het uitvoeren van een werkbelasting. Neem bijvoorbeeld een 'web-app': u ziet een web-app waarschijnlijk als de rekenresources en toepassingscode die samen functionaliteit bieden aan een browser. Maar in App Service bestaat een *web-app* uit de rekenresources die door Azure worden verstrekt voor het hosten van uw toepassingscode. 

Uw toepassing kan bestaan uit meerdere App Service-apps van verschillende typen. Als uw toepassing bijvoorbeeld bestaat uit een webfront-end en een RESTful-API-back-end, kunt u:

- Beide (front-end en API) implementeren in één web-app  
- Uw front-endcode implementeren in een web-app en uw back-endcode implementeren in een API-app. 



## <a name="app-service-plans"></a>App Service-abonnementen
[App Service-plannen](azure-web-sites-web-hosting-plans-in-depth-overview.md) bestaan uit een verzameling van fysieke resources die worden gebruikt voor het hosten van uw apps.

In App Service-plannen wordt het volgende gedefinieerd:

- **Regio** (VS - west, VS - oost, etc.)
- **Schaal** (een, twee, drie exemplaren, etc.)
- **Exemplaargrootte** (klein, normaal, groot)
- **SKU** (Free, Shared, Basic, Standard, Premium)

Alle toepassingen die zijn toegewezen aan een **App Service-plan** delen de gedefinieerde resources zodat u kosten kunt besparen als u meerdere apps host.

Uw **App Service-plan** kan worden geschaald van de SKU's **Free** en **Shared** naar de SKU's **Basic**, **Standard** en **Premium**, zodat u toegang hebt tot meer resources en functies. Als uw App Service-plan is ingesteld op **Basic** of hoger, kunt u de **grootte** van VM’s en het aantal VM’s bepalen.

De kosten voor het App Service-plan worden bepaald op basis van de **SKU** en de **schaal** van het plan. Het aantal apps dat wordt gehost, is niet belangrijk. 

Als u meer schaalbaarheid en netwerkisolatie nodig hebt, kunt u uw apps uitvoeren in een [App Service-omgeving](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Prijzen
Zie [Prijzen van App Service](https://azure.microsoft.com/pricing/details/app-service/) voor meer informatie over de prijzen van App Service.

## <a name="test-drive-app-service"></a>App Service uitproberen
[Maak een voorbeeld-web-app, mobiele app of logische app](https://azure.microsoft.com/try/app-service/) en speel hier een uur mee, zonder dat u een creditcard nodig hebt, zonder verbintenissen en zonder gedoe.

Of open een [gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/) en probeer een van onze zelfstudies Aan de slag:

* [Zelfstudie: een web-app maken](../app-service-web/app-service-web-get-started.md)
* [Zelfstudie: een mobiele app maken](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Zelfstudie: een API-app maken](../app-service-api/app-service-api-dotnet-get-started.md)
* [Zelfstudie: een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO3-->


