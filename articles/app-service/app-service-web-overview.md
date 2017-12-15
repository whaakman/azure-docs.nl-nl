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
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Overzicht van Web Apps

*Azure App Service Web Apps* (of kortweg Web Apps) is een service voor het hosten van webtoepassingen, REST-API's en mobiele back-ends. U kunt er in uw favoriete taal programmeren, of het nu .NET, .NET Core, Java, Ruby, Node.js, PHP of Python is. U kunt eenvoudig apps uitvoeren en schalen op virtuele Windows- of Linux-machines (zie [App Service op Linux](containers/app-service-linux-intro.md)). 

Web Apps voegen niet alleen de kracht van Microsoft Azure aan uw toepassing toe, zoals beveiliging, taakverdeling, automatisch schalen en geautomatiseerd beheer. U kunt ook profiteren van de DevOps-mogelijkheden, zoals continue implementatie van VSTS, GitHub, Docker Hub en andere bronnen, pakketbeheer, faseringsomgevingen, aangepast domein en SSL-certificaten. 

Met App Service betaalt u voor de Azure-rekenresources die u gebruikt. De rekenresources die u gebruikt, zijn vastgesteld door het _App Service-plan_ waarmee u uw Web-Apps uitvoert. Zie [Overzicht van Azure App Service-plan](azure-web-sites-web-hosting-plans-in-depth-overview.md) voor meer informatie.

In de volgende 5 minuten durende video maakt u kennis met Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Waarom Web Apps gebruiken?
Hier volgen enkele belangrijke functies van App Service Web Apps:

* **Meerdere talen en frameworks**: Web Apps biedt uitstekende ondersteuning voor ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP of Python. U kunt ook [PowerShell en andere scripts of uitvoerbare bestanden](web-sites-create-web-jobs.md) als achtergrondservices uitvoeren.
* **DevOps-optimalisatie**: stel [continue integratie en implementatie](app-service-continuous-deployment.md) in met Visual Studio-teamservices, GitHub, BitBucket, Docker Hub of Azure Container Service. Verhoog updateniveaus via [test- en faseringsomgevingen](web-sites-staged-publishing.md). Beheer uw apps in Web Apps met [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de [platformoverschrijdende opdrachtregelinterface (CLI)](/cli/azure/install-azure-cli).
* **Globale schaling met hoge beschikbaarheid**: u kunt handmatig of automatisch [omhoog](web-sites-scale.md) schalen of [uit](../monitoring-and-diagnostics/insights-how-to-scale.md)schalen. U kunt uw apps overal in de globale datacenterinfrastructuur van Microsoft hosten; de [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) van App Service belooft hoge beschikbaarheid.
* **Verbindingen met SaaS-platforms en on-premises gegevens**: u kunt kiezen uit meer dan 50 [connectors](../connectors/apis-list.md) voor bedrijfssystemen (zoals SAP), SaaS-services (zoals Salesforce) en internetservices (zoals Facebook). Toegang tot on-premises gegevens met [hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Beveiliging en naleving**: App Service voldoet aan de vereisten van [ISO, SOC en PCI](https://www.microsoft.com/TrustCenter/). Verifieer gebruikers met [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) of via sociaal aanmelden ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) en [Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Maak [IP-adresbeperkingen](app-service-ip-restrictions.md) en [ beheer service-identiteiten](app-service-managed-service-identity.md).
* **Toepassingssjablonen**: kies uit een uitgebreide lijst met toepassingssjablonen in [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/), zoals WordPress, Joomla en Drupal.
* **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van apps en het opsporen van fouten.
* **API en mobiele functies**: Web Apps biedt direct CORS-ondersteuning voor RESTful-API-scenario's en vereenvoudigt scenario's met mobiele apps door verificatie, offline gegevenssynchronisatie en pushmeldingen in te schakelen.
* **Serverloze code**: voer een codefragment of script op aanvraag uit zonder dat u expliciet infrastructuur hoeft in te richten of te beheren. En u betaalt slechts voor de rekentijd die feitelijk door de code wordt gebruikt (zie [Azure Functions](/azure/azure-functions/)).

Naast Web Apps in App Service biedt Azure nog andere services die kunnen worden gebruikt voor het hosten van websites en web-apps. Voor de meeste scenario's is Web Apps de beste keuze.  Overweeg [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric) voor een architectuur op basis van microservices. Als u meer controle wilt over de virtuele machines waarop uw code wordt uitgevoerd, kunt u [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) overwegen. Zie [Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services](choose-web-site-cloud-service-vm.md) voor meer informatie over hoe u moet kiezen tussen deze Azure-services.

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

