---
title: Overzicht van App Service - Azure | Microsoft Docs
description: Lees hoe Azure App Service u helpt bij het ontwikkelen en hosten van web-apps
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 5ce8abd08f82a2a8b69cc996bfd8f7f060bb8c23
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205033"
---
# <a name="app-service-overview"></a>Overzicht van App Service

*Azure App Service* is een HTTP-gebaseerde service voor het hosten van webtoepassingen, REST-API's en mobiele back-ends. U kunt er in uw favoriete taal programmeren, of het nu .NET, .NET Core, Java, Ruby, Node.js, PHP of Python is. Toepassingen kunnen eenvoudig worden uitgevoerd en geschaald in op Windows en Linux gebaseerde omgevingen. Voor op Linux gebaseerde omgevingen bekijkt u [App Service in Linux](containers/app-service-linux-intro.md). 

App Service voegt niet alleen de kracht van Microsoft Azure aan uw toepassing toe, zoals beveiliging, taakverdeling, automatisch schalen en geautomatiseerd beheer. U kunt ook profiteren van de DevOps-mogelijkheden, zoals continue implementatie van Azure DevOps, GitHub, Docker Hub en andere bronnen, pakketbeheer, faseringsomgevingen, aangepast domein en SSL-certificaten. 

Met App Service betaalt u voor de Azure-rekenresources die u gebruikt. De rekenresources die u gebruikt, zijn vastgesteld door het _App Service-plan_ waarmee u uw apps uitvoert. Zie [Overzicht van Azure App Service-plannen](overview-hosting-plans.md) voor meer informatie.

## <a name="why-use-app-service"></a>Waarom App Service gebruiken?

Hier volgen enkele belangrijke functies van App Service:

* **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP of Python. U kunt ook [PowerShell en andere scripts of uitvoerbare bestanden](webjobs-create.md) als achtergrondservices uitvoeren.
* **DevOps-optimalisatie**: stel [continue integratie en implementatie](deploy-continuous-deployment.md) in met Azure DevOps, GitHub, BitBucket, Docker Hub of Azure Container Registry. Verhoog updateniveaus via [test- en faseringsomgevingen](deploy-staging-slots.md). Beheer uw apps in App Service met [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de [platformoverschrijdende opdrachtregelinterface (CLI)](/cli/azure/install-azure-cli).
* **Globale schaling met hoge beschikbaarheid**: u kunt handmatig of automatisch [omhoog](web-sites-scale.md) schalen of [uit](../monitoring-and-diagnostics/insights-how-to-scale.md)schalen. U kunt uw apps overal in de globale datacenterinfrastructuur van Microsoft hosten; de [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) van App Service belooft hoge beschikbaarheid.
* **Verbindingen met SaaS-platforms en on-premises gegevens**: u kunt kiezen uit meer dan 50 [connectors](../connectors/apis-list.md) voor bedrijfssystemen (zoals SAP), SaaS-services (zoals Salesforce) en internetservices (zoals Facebook). Toegang tot on-premises gegevens met [hybride verbindingen](app-service-hybrid-connections.md) en [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Beveiliging en naleving**: App Service voldoet aan de vereisten van [ISO, SOC en PCI](https://www.microsoft.com/en-us/trustcenter). Verifieer gebruikers met [Azure Active Directory](configure-authentication-provider-aad.md) of via sociaal aanmelden ([Google](configure-authentication-provider-google.md), [Facebook](configure-authentication-provider-facebook.md), [Twitter](configure-authentication-provider-twitter.md) en [Microsoft](configure-authentication-provider-microsoft.md)). Maak [IP-adresbeperkingen](app-service-ip-restrictions.md) en [ beheer service-identiteiten](overview-managed-identity.md).
* **Toepassingssjablonen**: kies uit een uitgebreide lijst met toepassingssjablonen in [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/), zoals WordPress, Joomla en Drupal.
* **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van apps en het opsporen van fouten.
* **API en mobiele functies**: App Service biedt direct CORS-ondersteuning voor RESTful-API-scenario's en vereenvoudigt scenario's met mobiele apps door verificatie, offline gegevenssynchronisatie en pushmeldingen in te schakelen.
* **Serverloze code**: voer een codefragment of script op aanvraag uit zonder dat u expliciet infrastructuur hoeft in te richten of te beheren. En u betaalt slechts voor de rekentijd die feitelijk door de code wordt gebruikt (zie [Azure Functions](/azure/azure-functions/)).

Naast App Service biedt Azure nog andere services die kunnen worden gebruikt voor het hosten van websites en webtoepassingen. Voor de meeste scenario's is App Service de beste keuze.  Overweeg [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric) voor een architectuur op basis van microservices. Als u meer controle wilt over de virtuele machines waarop uw code wordt uitgevoerd, kunt u [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) overwegen. Zie [Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services](overview-compare.md) voor meer informatie over hoe u moet kiezen tussen deze Azure-services.

## <a name="next-steps"></a>Volgende stappen

Uw eerste web-app maken.

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Ruby (in Linux)](containers/quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python (on Linux)](containers/quickstart-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)
