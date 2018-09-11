---
title: Veelgestelde vragen over de implementatie voor Azure WebApps | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de implementatie voor de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: ab8750e5824cf9f7635d11a6b2be332b2f9a761c
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302585"
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Veelgestelde vragen over de implementatie voor Web-Apps in Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over problemen met de implementatie voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Ik ben net aan de slag met App Service Webapps. Hoe kan ik mijn code publiceren?

Hier zijn enkele opties voor het publiceren van de code van uw web-app:

*   Implementeren met behulp van Visual Studio. Als u de Visual Studio-oplossing hebt, met de rechtermuisknop op het web application-project en selecteer vervolgens **publiceren**.
*   Implementeren met behulp van een FTP-client. Download het publicatieprofiel voor de web-app die u wilt uw code te implementeren in Azure portal. Vervolgens de bestanden uploaden naar \site\wwwroot met behulp van dezelfde referenties van de FTP-profiel publiceren.

Zie voor meer informatie, [uw app implementeren in App Service](app-service-deploy-local-git.md).

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a>Ik zie een foutbericht wanneer ik probeer om vanuit Visual Studio te implementeren. Hoe kan ik dit oplossen?

Als u het volgende bericht ziet, kunt u een oudere versie van de SDK: ' Fout tijdens de implementatie voor de resource 'YourResourceName' in de resourcegroep 'YourResourceGroup': MissingRegistrationForLocation: het abonnement is niet geregistreerd voor de brontype 'onderdelen' op de locatie 'VS-midden'. Opnieuw te registreren voor deze provider om toegang te krijgen tot deze locatie." 

U kunt deze fout oplossen door een upgrade uitvoert naar de [nieuwste SDK](https://azure.microsoft.com/downloads/). Als u dit bericht ziet en u de nieuwste SDK hebt, moet u een ondersteuningsaanvraag indienen.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Hoe implementeer ik een ASP.NET-toepassing vanuit Visual Studio naar App Service?
<a id="deployasp"></a>

De zelfstudie [in vijf minuten uw eerste ASP.NET-web-app in Azure maken](app-service-web-get-started-dotnet.md) ziet u hoe u een ASP.NET-webtoepassing in een WebApp in App Service implementeren met behulp van Visual Studio 2017.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Wat zijn de verschillende soorten referenties voor implementatie?

App Service ondersteunt twee soorten referenties voor de lokale Git-implementatie en implementatie van FTP/S. Zie voor meer informatie over het configureren van de referenties voor implementatie [implementatiereferenties voor App Service configureren](app-service-deployment-credentials.md).

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>Wat is de structuur van het bestand of map van mijn App Service-web-app?

Zie voor meer informatie over de structuur van het bestand van uw App Service-app [structuur-bestand in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Hoe los ik 'FTP-fout 550 - er is niet genoeg ruimte op de schijf' wanneer ik wil mijn bestanden van de FTP?

Als u dit bericht ziet, is het waarschijnlijk dat u een schijfquotum in het service-plan voor uw web-app ondervindt. U moet mogelijk worden uitgebreid naar een hogere servicelaag op basis van uw behoeften schijf. Zie voor meer informatie over de prijzen voor plannen en resourcelimieten [App Service-prijzen](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Hoe stel ik continue implementatie voor mijn App Service-web-app?

U kunt instellen van continue implementatie vanuit verschillende bronnen, met inbegrip van Azure DevOps, OneDrive, GitHub, Bitbucket, Dropbox en andere Git-opslagplaatsen. Deze opties zijn beschikbaar in de portal. [Continue implementatie naar App Service](app-service-continuous-deployment.md) is een nuttig zelfstudie waarin wordt uitgelegd hoe u continue implementatie kunt instellen.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Hoe los ik problemen met continue implementatie vanuit GitHub en Bitbucket?

Zie voor meer informatie over het onderzoeken van problemen met continue implementatie vanuit GitHub of Bitbucket, [doorlopende implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a>Ik kan geen FTP-site en publiceren van mijn code. Hoe kan ik dit oplossen?

FTP-problemen oplossen:

1. Controleer of dat u de correcte hostnaam en referenties invoert. Zie voor gedetailleerde informatie over de verschillende soorten referenties en het gebruik ervan, [implementatiereferenties](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. Controleer of de FTP-poorten worden niet geblokkeerd door een firewall. De poorten moeten deze instellingen hebben:
    * Verbindingspoort voor FTP-besturingselement: 21
    * Verbindingspoort voor FTP-gegevens: 989, 10001-10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Hoe kan ik mijn code publiceren naar App Service?

De Azure-Quickstart is ontworpen om u te helpen bij het implementeren van uw app met behulp van de implementatiestack en de methode van uw keuze. Voor het gebruik van de Quick Start in Azure portal, gaat u naar **instellingen** > **Appimplementatie**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Waarom wordt mijn app soms opnieuw opgestart na de implementatie naar App Service?

Zie voor meer informatie over de omstandigheden waaronder implementatie van een toepassing tot een opnieuw opstarten leiden kan, [implementatie versus runtimeproblemen](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Zoals in het artikel wordt beschreven, worden bestanden naar de wwwroot-map in App Service geïmplementeerd. Er wordt nooit rechtstreeks opnieuw opgestart uw app.

## <a name="how-do-i-integrate-azure-devops-code-with-app-service"></a>Hoe Integreer ik Azure DevOps-code met App Service?

U hebt twee opties voor het gebruik van continue implementatie met Azure DevOps:

*   Gebruik een Git-project. Verbinding maken via App Service met behulp van de implementatie-opties voor deze opslagplaats.
*   Een project Team Foundation Version Control (TFVC) gebruiken. Implementeren met behulp van de build-agent voor App Service.

Implementatie van de continue code voor deze beide opties, is afhankelijk van bestaande werkstromen voor ontwikkelaars en procedures die is ingecheckt. Raadpleeg voor meer informatie de volgende artikelen: 

*   [Continue implementatie van uw app naar een Azure-website implementeren](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Instellen van een Azure DevOps-organisatie, zodat deze op een web-app implementeren kunt](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Hoe gebruik ik FTP- of FTPS mijn app implementeert in App Service?

Zie voor meer informatie over uw web-app implementeren in App Service met behulp van FTP- of FTPS [uw app in App Service implementeren met behulp van FTP/S](app-service-deploy-ftp.md).
