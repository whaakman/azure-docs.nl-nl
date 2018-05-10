---
title: Azure App Service op de implementatie van de Linux - afbeelding van aangepaste, meerdere container of ingebouwde platforminstallatiekopie?  | Microsoft Docs
description: U moet kiezen tussen aangepaste Docker-container-implementatie, meerdere container en een ingebouwde toepassingsframework voor App-Service op Linux
keywords: Azure app service, web-app, linux, oss
services: app-service
documentationCenter: ''
authors: msangapu
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.openlocfilehash: 012f78fc07f237e8ed532246c81a3c86bb6ab4ac
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Aangepaste installatiekopie, meerdere container of ingebouwde platforminstallatiekopie?

[App-Service op Linux](app-service-linux-intro.md) biedt drie verschillende paden voor het uitvoeren van uw toepassing op het web gepubliceerd:

- **Implementatie van de aangepaste installatiekopie**: 'Dockerize' uw app in een Docker-installatiekopie die alle uw bestanden en afhankelijkheden in een kant-en-klaar-pakket bevat.
- **Implementatie van meerdere container**: 'Dockerize' uw app via meerdere containers met een Docker Compose of een configuratiebestand Kubernetes. Zie voor meer informatie [meerdere container app](#multi-container-apps-supportability).
- **App-implementatie met een ingebouwde platforminstallatiekopie**: onze ingebouwde platform-installatiekopieën bevatten algemene web app runtimes en afhankelijkheden, zoals knooppunt en PHP. Gebruik een van de [methoden voor het implementeren van Azure App Service](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) naar uw app implementeren in uw web-app-opslag en gebruik vervolgens een ingebouwde platforminstallatiekopie uit te voeren.

##<a name="which-method-is-right-for-your-app"></a>Welke methode is geschikt voor uw app? 

De primaire factoren zijn:

- **Beschikbaarheid van Docker in uw werkstroom ontwikkeling**: aangepaste installatiekopie ontwikkeling vereist basiskennis van de werkstroom Docker-ontwikkeling. Implementatie van een aangepaste installatiekopie in een web-app moet de publicatie van uw aangepaste installatiekopie naar een host opslagplaats zoals Docker-Hub. Als u bekend bent met Docker en Docker taken kunt toevoegen aan uw werkstroom build, of als u al uw app als een Docker-installatiekopie publiceert, wordt een aangepaste installatiekopie is bijna zeker de beste keuze.
- **Architectuur met meerdere lagen**: meerdere containers, zoals een toepassing weblaag en een API-laag mogelijkheden gescheiden door middel van meerdere container implementeren. 
- **De prestaties van toepassingen**: verbeteren de prestaties van uw app met behulp van een cache-laag, zoals Redis in meerdere container. Selecteer meerdere container om dit te bereiken.
- **Unieke runtime-vereisten**: de installatiekopieën van het ingebouwde platform zijn ontworpen om te voldoen aan de behoeften van de meeste web-apps, maar zijn beperkt in hun aanpassingsmogelijkheden. Uw app hebt unieke afhankelijkheden of andere runtime-vereisten die groter is dan wat de installatiekopieën van het ingebouwde geschikt zijn.
- **Vereisten bouwen**: met [continue implementatie](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), kunt u uw app actief en werkend op Azure rechtstreeks vanuit de broncode. Er is geen externe build of publicatie proces vereist. Er is echter een limiet aan de aanpassingsmogelijkheden en beschikbaarheid van build-hulpprogramma's binnen de [Kudu](https://github.com/projectkudu/kudu/wiki) implementatie-engine. Uw app kan de mogelijkheden van Kudu langzamerhand wanneer deze in de bijbehorende afhankelijkheden of vereisten voor aangepaste build logica groeit.
- **Schijfruimte die lezen/schrijven**: alle web-apps een opslagvolume toegewezen voor webinhoud. Dit volume, ondersteund door Azure Storage is gekoppeld aan `/home` in de app-bestandssysteem. In tegenstelling tot bestanden in het bestandssysteem container bestanden in het volume van de inhoud toegankelijk zijn in alle exemplaren van de schaal van een app en wijzigingen persistent is voor de app opnieuw wordt opgestart. De latentie van de schijf van het volume van de inhoud hoger is echter en meer variabele dan de latentie van de lokale container bestandssysteem en de toegang kan worden beïnvloed door het platform upgrades, niet-geplande uitvaltijd en problemen met de netwerkverbinding. Apps waarvoor zware alleen-lezen toegang tot de inhoudsbestanden kunnen profiteren van de implementatie van de aangepaste installatiekopie, die bestanden worden geplaatst in het bestandssysteem van de afbeelding in plaats van op het volume van de inhoud.
- **Resourcegebruik bouwen**: wanneer een app wordt geïmplementeerd van bron, de implementatiescripts uitgevoerd door Kudu gebruik dezelfde App Service-Plan berekenings- en bronnen als de actieve app. Grote app-implementaties mogelijk meer bronnen of tijd dan het gewenste verbruiken. In het bijzonder veel implementatiewerkstromen genereren zware schijfactiviteit op de app inhoud volume niet is geoptimaliseerd voor deze activiteit. Een aangepaste installatiekopie biedt alle bestanden en afhankelijkheden van uw app naar Azure in een enkel pakket met extra bestandsoverdrachten of acties voor de implementatie niet vereist.
- **Nodig voor snelle herhaling**: een app Dockerizing build extra stappen vereist. Wijzigingen van kracht te laten, moet u de nieuwe installatiekopie naar een opslagplaats bij elke update push. Deze updates worden vervolgens opgehaald voor de Azure-omgeving. Implementeren vanaf de bron kan als een van de ingebouwde containers voldoet aan de behoeften van uw app, een snellere ontwikkeling werkstroom bieden.

## <a name="multi-container-apps-supportability"></a>Ondersteuning voor meerdere container apps

### <a name="supported-docker-compose-configuration-options"></a>Ondersteunde configuratieopties van Docker Compose
- opdracht
- entrypoint
- omgeving
- Afbeelding
- Poorten
- opnieuw opstarten
- Services
- Volumes

### <a name="unsupported-docker-compose-configuration-options"></a>Niet-ondersteunde configuratieopties van Docker Compose
- build (niet toegestaan)
- depends_on (genegeerd)
- netwerken (genegeerd)
- geheimen (genegeerd)
- andere poorten dan 80 en 8080 (genegeerd)

> [!NOTE]
> Andere opties niet expliciet wordt genoemd, worden ook openbare preview genegeerd.

### <a name="supported-kubernetes-configuration-options"></a>Ondersteunde Kubernetes configuratieopties
- argumenten
- opdracht
- containers
- Afbeelding
- naam
- Poorten
- specificatie

> [!NOTE]
>Andere Kubernetes-opties niet expliciet wordt genoemd, worden niet ondersteund in de openbare Preview.
>