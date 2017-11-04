---
title: Implementatie-Veelgestelde vragen over Azure-web-apps | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de implementatie voor de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 318a236652229c4e093ca33886ac1831686aed73
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Veelgestelde vragen implementatie voor Web-Apps in Azure

In dit artikel bevat de antwoorden op veelgestelde vragen (FAQ's) over problemen bij de implementatie voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Ik ben net aan de slag met App Service-web-apps. Hoe kan ik mijn code publiceren?

Hier zijn enkele mogelijkheden voor het publiceren van uw web-app-code:

*   Implementeren met behulp van Visual Studio. Als u de Visual Studio-oplossing hebt, met de rechtermuisknop op het webtoepassingsproject en selecteer vervolgens **publiceren**.
*   Implementeren met behulp van een FTP-client. Download het publicatieprofiel voor de web-app die u wilt uw code te implementeren in de Azure portal. Vervolgens de bestanden uploaden naar \site\wwwroot met behulp van dezelfde referenties van de FTP-profiel publiceren.

Zie voor meer informatie [implementeren van uw app in App Service](app-service-deploy-local-git.md).

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a>Er wordt een foutbericht weergegeven wanneer ik wil implementeren vanuit Visual Studio. Hoe kan ik dit oplossen?

Als u het volgende bericht ziet, kunt u een oudere versie van de SDK: ' Fout tijdens de implementatie voor resource 'YourResourceName' in de resourcegroep 'YourResourceGroup': MissingRegistrationForLocation: het abonnement is niet geregistreerd voor de brontype 'onderdelen' in de locatie 'VS-midden'. Registreer opnieuw voor deze provider om toegang te krijgen tot deze locatie." 

U lost deze fout door een upgrade uitvoert naar de [nieuwste SDK](https://azure.microsoft.com/downloads/). Als dit bericht wordt weergegeven en u de nieuwste SDK hebt, een verzoek om ondersteuning te verzenden.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Hoe implementeer ik een ASP.NET-toepassing vanuit Visual Studio in App Service
<a id="deployasp"></a>

De zelfstudie [uw eerste ASP.NET-web-app maken in Azure binnen vijf minuten](app-service-web-get-started-dotnet.md) ziet u hoe ASP.NET-webtoepassing in een WebApp in App Service implementeren met Visual Studio 2017.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Wat zijn de verschillende soorten referenties voor implementatie?

App Service ondersteunt twee typen van referenties voor de FTP-/ S-implementatie en lokale Git-implementatie. Zie voor meer informatie over het configureren van referenties voor implementatie [referenties voor implementatie van App Service configureert](app-service-deployment-credentials.md).

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>Wat is de structuur van het bestand of map van mijn App Service-web-app?

Zie voor meer informatie over de structuur van uw App Service-app [structure-bestand in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Hoe los ik 'FTP-fout 550 - er is niet genoeg ruimte op de schijf' wanneer ik wil mijn bestanden van de FTP?

Als u dit bericht ziet, is het waarschijnlijk dat u in een schijfquotum in het service-abonnement voor uw web-app uitvoert. Mogelijk moet u opschalen naar een hogere servicelaag op basis van uw behoeften schijf. Zie voor meer informatie over prijzen voor plannen en limieten voor [App Service-prijzen](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Hoe stel ik continue implementatie in voor mijn App Service-web-app?

U kunt continue implementatie van verschillende bronnen, met inbegrip van Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox en andere Git-opslagplaatsen instellen. Deze opties zijn beschikbaar in de portal. [Continue implementatie naar App Service](app-service-continuous-deployment.md) is een handig zelfstudie waarin wordt uitgelegd hoe u continue implementatie instelt.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Hoe los ik problemen met continue implementatie van GitHub en Bitbucket

Zie voor meer informatie over het onderzoeken van problemen met continue implementatie van GitHub of Bitbucket [onderzoeken continue implementatie](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a>Ik kan geen FTP naar Mijn site en publiceren van mijn code. Hoe kan ik dit oplossen?

FTP-problemen oplossen:

1. Controleer of dat u de juiste hostnaam en de referenties invoert. Zie voor gedetailleerde informatie over de verschillende soorten referenties en het gebruik ervan, [implementatiereferenties](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. Controleer of de FTP-poorten worden niet geblokkeerd door een firewall. De poorten moeten deze instellingen hebben:
    * Verbindingspoort voor FTP-besturingselement: 21
    * FTP-gegevenspoort voor verbinding: 989, 10001 10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Hoe kan ik mijn code publiceren in App Service?

De Azure Quickstart is ontworpen om u te helpen bij het implementeren van uw app met behulp van de implementatie-stack en de methode van uw keuze. Als u de Quick Start, in de Azure-portal, gaat u naar **instellingen** > **App-implementatie**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Waarom wordt mijn app soms opnieuw opgestart na de implementatie voor App Service?

Zie voor meer informatie over de omstandigheden waaronder de implementatie van een toepassing tot opnieuw opstarten leiden kan, [implementatie versus runtime problemen](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Zoals in het artikel wordt beschreven, worden bestanden in App Service geïmplementeerd op de wwwroot-map. Het start rechtstreeks nooit uw app.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Hoe ik code van de Visual Studio Team Services integreren met App Service?

U hebt twee opties voor het gebruik van continue implementatie met Visual Studio Team Services:

*   Gebruik een Git-project. Verbinding maken via App Service met behulp van de implementatie-opties voor deze opslagplaats.
*   Een project Team Foundation versie besturingselement (TFVC) gebruiken. Implementeren met behulp van de build-agent voor App Service.

Code continue implementatie voor deze beide opties zijn afhankelijk van bestaande developer-werkstromen en in-procedures. Zie voor meer informatie in deze artikelen: 

*   [Continue implementatie van uw app naar een Azure-website](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Een Visual Studio Team Services-account instellen zodat deze op een web-app implementeren kunt](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Hoe gebruik ik FTP of FTPS mijn app implementeren in App Service?

Zie voor meer informatie over het implementeren van uw web-app in App Service met FTP of FTPS [uw app in App Service implementeren met behulp van FTP/S](app-service-deploy-ftp.md).
