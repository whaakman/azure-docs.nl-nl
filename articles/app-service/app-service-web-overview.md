---
title: Overzicht van Web Apps | Microsoft Docs
description: Lees hoe Azure App Service u helpt bij het ontwikkelen en hosten van web-apps
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Overzicht van Web Apps

*Azure App Service Web Apps* (of alleen Web-Apps) is een service voor het hosten van webtoepassingen, REST-API's en mobiele terug eindigt. U kunt ontwikkelen in uw favoriete taal, worden deze .NET, .NET Core, Java, Ruby, Node.js, PHP of Python. U kunt uitvoeren en apps op Windows of Linux-machines probleemloos schalen (Zie [op Linux-App Service](containers/app-service-linux-intro.md)). 

Web-Apps niet alleen de kracht van Microsoft Azure toegevoegd aan uw toepassing, zoals beveiliging, taakverdeling, automatisch schalen, en geautomatiseerd beheer. U kunt ook profiteren van de DevOps-mogelijkheden, zoals continue implementatie van VSTS, GitHub, Docker-Hub en andere bronnen, het beheer van pakket staging-omgevingen, aangepast domein en SSL-certificaten. 

Met App Service betaalt u voor de Azure-rekenresources die u gebruikt. De rekenresources die u gebruikt, wordt bepaald door de _App Service-abonnement_ uw Web-Apps uit te voeren op. Zie voor meer informatie [App Service-plannen in Azure Web Apps](azure-web-sites-web-hosting-plans-in-depth-overview.md).

In de volgende 5 minuten durende video maakt u kennis met Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Waarom Web Apps gebruiken?
Hier volgen enkele belangrijke functies van App Service Web Apps:

* **Meerdere talen en frameworks** -Web-Apps biedt uitstekende ondersteuning voor ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP of Python. U kunt ook uitvoeren [PowerShell en andere scripts of het uitvoerbare bestanden](web-sites-create-web-jobs.md) als in de services op de achtergrond.
* **DevOps-optimalisatie** -instellen [continue integratie en implementatie](app-service-continuous-deployment.md) met Visual Studio Team Services, GitHub, BitBucket, Docker-Hub of Azure Container Service. Verhoog updateniveaus via [test- en faseringsomgevingen](web-sites-staged-publishing.md). Uw apps in Web-Apps beheren met behulp van [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de [platformoverschrijdende opdrachtregelinterface (CLI)](/cli/azure/install-azure-cli).
* **Globale schaling met hoge beschikbaarheid**: u kunt handmatig of automatisch [omhoog](web-sites-scale.md) schalen of [uit](../monitoring-and-diagnostics/insights-how-to-scale.md)schalen. U kunt uw apps overal in de globale datacenterinfrastructuur van Microsoft hosten; de [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) van App Service belooft hoge beschikbaarheid.
* **Verbindingen met SaaS-platforms en on-premises gegevens** -kiezen uit meer dan 50 [connectors](../connectors/apis-list.md) voor bedrijfssystemen (zoals SAP), SaaS-services (zoals Salesforce) en internetservices (zoals Facebook). Toegang tot on-premises gegevens met [hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Beveiliging en naleving**: App Service voldoet aan de vereisten van [ISO, SOC en PCI](https://www.microsoft.com/TrustCenter/). Verificatie van gebruikers met [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) of met aanmelden bij sociale media ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md), en [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Maak [IP-adresbeperkingen](app-service-ip-restrictions.md) en [service-identiteiten beheren](app-service-managed-service-identity.md).
* **Toepassingssjablonen** -kiezen uit een uitgebreide lijst met Toepassingssjablonen in de [Azure Marketplace](https://azure.microsoft.com/marketplace/), zoals WordPress, Joomla en Drupal.
* **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van apps en het opsporen van fouten.
* **API en functies voor mobiele** -Web-Apps directe CORS-ondersteuning biedt voor RESTful-API's en mobiele app-scenario's door in te schakelen voor verificatie, offline synchroniseren van gegevens en pushmeldingen vereenvoudigt.
* **Zonder server code** : uitvoeren van een codefragment of op aanvraag script zonder expliciet inrichten of infrastructuur beheren en betaalt alleen voor de compute-tijd uw code daadwerkelijk gebruikt (Zie [Azure Functions](/azure/azure-functions/)).

Naast Web Apps in App Service biedt Azure nog andere services die kunnen worden gebruikt voor het hosten van websites en web-apps. Voor de meeste scenario's is Web Apps de beste keuze.  Voor microservice-architectuur, kunt u overwegen [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Als u meer controle over de virtuele machines die uw code wordt uitgevoerd op nodig hebt, overweeg dan [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Zie [Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services](choose-web-site-cloud-service-vm.md) voor meer informatie over hoe u moet kiezen tussen deze Azure-services.

## <a name="next-steps"></a>Volgende stappen

Uw eerste web-app maken.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

