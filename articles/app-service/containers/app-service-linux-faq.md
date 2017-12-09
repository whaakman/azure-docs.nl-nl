---
title: Op Linux FAQ-Azure App Service | Microsoft Docs
description: Azure App Service op Linux Veelgestelde vragen over.
keywords: Azure app service, web-app, veelgestelde vragen over, linux, oss
services: app-service
documentationCenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: b22d5f3497c388192764aa6b4ee8c95fec568bd8
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-app-service-on-linux-faq"></a>Op Linux FAQ-Azure App Service

Met het uitbrengen van App-Service op Linux, momenteel niets over het toevoegen van functies en er verbeteringen aangebracht in ons platform. In dit artikel vindt u antwoorden op vragen of onze klanten is aangevraagd door ons onlangs.

Als u een vraag, opmerking bij het artikel hebt en we deze zo snel mogelijk moet beantwoorden.

## <a name="built-in-images"></a>Ingebouwde afbeeldingen

**Ik wil de ingebouwde Docker-containers die het platform biedt vertakken. Waar vind ik die bestanden**

U vindt alle Docker-bestanden op [GitHub](https://github.com/azure-app-service). U vindt alle Docker-containers op [Docker Hub](https://hub.docker.com/u/appsvc/).

**Wat zijn de verwachte waarden voor het starten van de sectie voor het configureren van de runtime-stack?**

Voor Node.js geeft u het configuratiebestand PM2 of het scriptbestand. Geef de naam van uw gecompileerde DLL voor .NET Core. Voor Ruby, kunt u het Ruby script dat u wilt uw app met initialiseren.

## <a name="management"></a>Beheer

**Wat gebeurt er wanneer ik de schakelaar opnieuw opstarten in de Azure portal?**

Deze actie is hetzelfde als een herstart Docker.

**Kan ik Secure Shell (SSH) om verbinding met de app-container virtuele machine (VM) te gebruiken?**

Ja, kunt u dat doen via de bronsite van de besturingselement-management (SCM).

**Hoe kan ik een Linux-App Service-abonnement via een SDK of een Azure Resource Manager-sjabloon maken?**

U moet instellen de **gereserveerde** veld van de app-service naar *true*.

## <a name="continuous-integration-and-deployment"></a>Continue integratie en implementatie

**Mijn web-app nog steeds de installatiekopie van een oude Docker-container gebruikt nadat ik de afbeelding op Docker-Hub hebt bijgewerkt. Ondersteund continue integratie en implementatie van aangepaste containers?**

Continue integratie/implementatie instellen voor Azure Container register of DockerHub door het controleren van het volgende artikel afbeeldingen [continue implementatie met Web-App voor Containers](./app-service-linux-ci-cd.md). U kunt de container voor persoonlijke registers vernieuwen door te stoppen en vervolgens uw web-app te starten. Of u kunt wijzigen of toevoegen van een dummy toepassingsinstelling geforceerd vernieuwen van de container.

**Testomgevingen ondersteund?**

Ja.

**Kan ik gebruiken *webimplementatie* mijn web-app implementeren?**

Ja, moet u een app instelling instellen `WEBSITE_WEBDEPLOY_USE_SCM` naar *false*.

**GIT-implementatie van mijn toepassing mislukt wanneer u Linux-web-app. Hoe kan ik tijdelijke oplossing van het probleem?**

Als uw Linux-web-app niet Git-implementatie, kunt u de volgende alternatieve opties om de toepassingscode van uw te implementeren:

- Gebruik de functie continue levering (Preview): U kunt de broncode van uw app opslaan in een Team Services Git-opslagplaats of GitHub-repo-continue levering van Azure gebruiken. Zie voor meer informatie [continue levering configureren voor Linux-web-app](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/).

- Gebruik de [ZIP API implementeren](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file): Gebruik deze API [SSH in uw web-app](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support#making-a-client-connection) en Ga naar de map waar u uw code te implementeren. Voer het volgende uit:

   ```
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Als u een foutmelding dat krijgt de `curl` opdracht is niet gevonden, zorg ervoor dat u curl installeert met behulp van `apt-get install curl` voordat u de vorige uitvoert `curl` opdracht.

## <a name="language-support"></a>Taalondersteuning

**Ik wil websockets gebruiken in mijn Node.js-toepassing, eventuele speciale instellingen of -configuraties in te stellen?**

Ja, uitschakelen `perMessageDeflate` in uw servercode voor Node.js. Als u socket.io gebruikt, doet u er bijvoorbeeld het volgende:
```
var io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Niet-gecompileerde .NET Core apps ondersteund?**

Ja.

**Ondersteund Composer als een afhankelijkheid manager voor PHP-apps?**

Ja. Tijdens een Git-implementatie moet Kudu detecteren dat u een PHP-toepassing (dankzij de aanwezigheid van een bestand composer.lock) implementeert en Kudu vervolgens een installatie composer geactiveerd voor u.

## <a name="custom-containers"></a>Aangepaste containers

**Ik gebruik mijn eigen aangepaste container. Ik wil het platform voor het koppelen van een SMB-share op de `/home/` directory.**

U kunt dit doen door het instellen van de `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app-instelling op *waar* of door het verwijderen van de app instellen volledig. Houd er rekening mee dat dit container wordt opnieuw opgestart wordt wanneer de opslag platform een wijziging doorloopt. 

>[!NOTE]
>Als de `WEBSITES_ENABLE_APP_SERVICE_STORAGE` instelling is *false*, wordt de `/home/` directory wordt niet gedeeld met exemplaren van de schaal en bestanden die zijn geschreven er wordt niet worden vastgehouden in opnieuw wordt opgestart.

**Mijn aangepaste container lang duurt om te starten en het platform de container opnieuw wordt opgestart voordat is gestart.**

U kunt de hoeveelheid tijd die het platform wordt gewacht voordat opnieuw wordt gestart van de container. Stel om dit te doen de `WEBSITES_CONTAINER_START_TIME_LIMIT` app-instelling op de gewenste waarde. De standaardwaarde is 230 seconden en de maximumwaarde is 600 seconden.

**Wat is de indeling voor de URL van de persoonlijke register?**

Geef de URL van de volledige register, met inbegrip van `http://` of `https://`.

**Wat is de indeling voor de naam van de installatiekopie in de optie persoonlijke register?**

De naam van de volledige installatiekopie, met inbegrip van de persoonlijke register-URL (bijvoorbeeld myacr.azurecr.io/dotnet:latest) toevoegen. Afbeelding van namen die een aangepaste poort gebruiken [kan niet worden opgegeven via de portal](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Om in te stellen `docker-custom-image-name`, gebruiken de [ `az` opdrachtregelprogramma](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set).

**Kan ik meer dan één poort worden weergegeven op de installatiekopie van mijn aangepaste container?**

Wij ondersteunen geen momenteel blootstellen van meer dan één poort.

**Kan ik mijn eigen opslag brengen?**

Wij ondersteunen geen momenteel door uw eigen opslag te plaatsen.

**Waarom kan ik mijn aangepaste container bestand system of actief processen van de site SCM niet doorzoeken?**

De SCM-site wordt uitgevoerd in een afzonderlijke container. U kunt het bestand systeem of met de processen van de app-container niet controleren.

**Mijn aangepaste container luistert naar een andere poort dan poort 80. Hoe kan ik mijn app voor het routeren van aanvragen naar deze poort configureren?**

We hebben poort automatisch detecteren. U kunt ook een app instelling opgeven *WEBSITES_PORT* en wijs hieraan de waarde van het verwachte poortnummer. Voorheen werd door het platform gebruikt de *poort* app-instelling. We bij het plannen van deze appinstelling als vervangen markeren en gebruik *WEBSITES_PORT* uitsluitend.

**Heb ik nodig voor het implementeren van HTTPS in Mijn aangepaste container?**

Nee, zorgt het platform voor HTTPS-beëindiging aan de gedeelde-front-ends.

## <a name="pricing-and-sla"></a>Prijzen en SLA

**Wat is de prijzen nu dat de service algemeen beschikbaar is?**

U in rekening worden gebracht de normale prijzen Azure App Service voor het aantal uren dat uw app wordt uitgevoerd.

## <a name="other-questions"></a>Andere vragen

**Wat zijn de ondersteunde tekens in toepassingsnamen instellingen?**

U kunt alleen letters (A-Z, a-z), cijfers (0-9) en het onderstrepingsteken (_) gebruiken voor toepassingsinstellingen.

**Waar kan ik de nieuwe functies aanvragen?**

U kunt uw idee op indienen de [forum met feedback van Web-Apps](https://aka.ms/webapps-uservoice). '[Linux]' toevoegen aan de titel van uw idee.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure App Service op Linux?](app-service-linux-intro.md)
* [Faseringsomgevingen in Azure App Service instellen](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continue implementatie met Web-App voor Containers](./app-service-linux-ci-cd.md)
