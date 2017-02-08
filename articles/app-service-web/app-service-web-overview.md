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
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 18959934c53e2e1c719cc627ffa286acbdcaa967


---
# <a name="web-apps-overview"></a>Overzicht van Web Apps
*App Service Web Apps* is een volledig beheerd rekenplatform dat is geoptimaliseerd voor het hosten van websites en web-apps. Met deze PaaS-aanbieding ([Platform-as-a-Service](https://en.wikipedia.org/wiki/Platform_as_a_service)) van Microsoft Azure kunt u zich concentreren op uw bedrijfslogica terwijl Azure zorgt voor de infrastructuur die nodig is om uw apps uit te voeren en te schalen.

In de volgende 5 minuten durende video maakt u kennis met Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>Wat is een web-app in App Service?
In App Service is een *web-app* het geheel van rekenresources die door Azure worden verstrekt voor het hosten van een website of web-app.  

Afhankelijk van de gekozen prijscategorie kunnen de rekenresources zich bevinden op gedeelde of toegewezen virtuele machines. De toepassingscode wordt uitgevoerd op een beheerde virtuele machine die is geïsoleerd van andere klanten.

De code kan zijn geschreven in elke taal die of elk framework dat wordt ondersteund door [Azure App Service](../app-service/app-service-value-prop-what-is.md), zoals ASP.NET, Node.js, Java, PHP of Python. U kunt in een web-app ook scripts uitvoeren die gebruikmaken van [PowerShell en andere scripttalen](web-sites-create-web-jobs.md#acceptablefiles).

Voor voorbeelden van typische toepassingsscenario's waarvoor u Web Apps kunt gebruiken, raadpleegt u [Web-app-scenario's](https://azure.microsoft.com/documentation/scenarios/web-app/) en de sectie **Scenario's en aanbevelingen** van [Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Waarom Web Apps gebruiken?
Hier volgen enkele belangrijke functies van Azure App Service die van toepassing zijn op Web Apps:

* **Meerdere talen en frameworks**: App Service biedt uitstekende ondersteuning voor ASP.NET, Node.js, Java, PHP en Python. U kunt ook [PowerShell en andere scripts of uitvoerbare bestanden](web-sites-create-web-jobs.md) uitvoeren op virtuele machines van App Service.
* **DevOps-optimalisatie**: stel [continue integratie en implementatie](app-service-continuous-deployment.md) in met Visual Studio Team Services, GitHub of BitBucket. Verhoog updateniveaus via [test- en faseringsomgevingen](web-sites-staged-publishing.md). Voer [A/B-tests](app-service-web-test-in-production-get-start.md) uit. Beheer uw apps in App Service met [Azure PowerShell](/powershell/azureps-cmdlets-docs) of de [platformoverschrijdende opdrachtregelinterface (CLI)](../xplat-cli-install.md).
* **Globale schaling met hoge beschikbaarheid**: u kunt handmatig of automatisch [omhoog](web-sites-scale.md) schalen of [uit](../monitoring-and-diagnostics/insights-how-to-scale.md)schalen. U kunt uw apps overal in de globale datacenterinfrastructuur van Microsoft hosten; de [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) van App Service belooft hoge beschikbaarheid.
* **Verbindingen met SaaS-platforms en on-premises gegevens**: u kunt kiezen uit meer dan 50 [connectors](../connectors/apis-list.md) voor bedrijfssystemen (zoals SAP, Siebel en Oracle), SaaS-services (zoals Salesforce en Office 365) en internetservices (zoals Facebook en Twitter). Toegang tot on-premises gegevens met [hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Beveiliging en naleving**: App Service voldoet aan de vereisten van [ISO, SOC en PCI](https://www.microsoft.com/TrustCenter/).
* **Toepassingssjablonen**: u kunt kiezen uit een uitgebreide lijst met toepassingssjablonen in [Azure Marketplace](https://azure.microsoft.com/marketplace/), waarmee u een wizard kunt gebruiken voor de installatie van populaire open-source software zoals WordPress, Joomla en Drupal.
* **Visual Studio-integratie**: specifieke hulpprogramma's in Visual Studio stroomlijnen het maken en implementeren van apps en het opsporen van fouten.

Bovendien kan een web-app gebruikmaken van functies die worden aangeboden door [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (zoals CORS-ondersteuning) en [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (zoals pushmeldingen). Zie [Overzicht van Azure App Service](../app-service/app-service-value-prop-what-is.md) voor meer informatie over soorten apps in App Service.

Naast Web Apps in App Service biedt Azure nog andere services die kunnen worden gebruikt voor het hosten van websites en web-apps. Voor de meeste scenario's is Web Apps de beste keuze.  Voor microservice-architectuur kunt u wellicht [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric) gebruiken. Hebt u meer controle nodig over de virtuele machines waarop uw code wordt uitgevoerd, dan kunt u [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) overwegen. Zie [Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services](choose-web-site-cloud-service-vm.md) voor meer informatie over hoe u moet kiezen tussen deze Azure-services.

## <a name="getting-started"></a>Aan de slag
Als u aan de slag wilt gaan door in App Service voorbeeldcode voor een nieuwe web-app te implementeren, volgt u een van de zelfstudies uit de onderstaande vervolgkeuzelijst. U hebt een gratis Azure-account nodig.

> [!div class="op_single_selector"]
> * [In vijf minuten uw eerste HTML-site implementeren in Azure](app-service-web-get-started-html-cli-nodejs.md)
> * [In vijf minuten uw eerste ASP.NET-web-app implementeren in Azure](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [In vijf minuten uw eerste PHP-web-app implementeren in Azure](app-service-web-get-started-php-cli-nodejs.md)
> * [In vijf minuten uw eerste Node.js-web-app implementeren in Azure](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [In vijf minuten uw eerste Python-web-app implementeren in Azure](app-service-web-get-started-python-cli-nodejs.md)
> * [In vijf minuten uw eerste Java-web-app implementeren in Azure](app-service-web-get-started-java.md)
> 
> 

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 



<!--HONumber=Feb17_HO1-->


