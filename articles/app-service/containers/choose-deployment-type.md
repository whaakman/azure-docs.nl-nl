---
title: Azure App Service onder Linux implementatie - aangepaste installatiekopie, meerdere containers of ingebouwde platform-installatiekopie?  | Microsoft Docs
description: U moet kiezen tussen implementatie van een aangepaste Docker-containers, meerdere containers en een ingebouwde toepassingsframework voor App Service on Linux
keywords: Azure appservice, web-app, linux, oss
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
ms.openlocfilehash: c619ae164f8f8b6e94d9061c4346de58bd6cb795
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319435"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Aangepaste installatiekopie, meerdere containers of ingebouwde platform-installatiekopie?

[App Service on Linux](app-service-linux-intro.md) biedt drie verschillende paden naar de mogelijkheid uw toepassing op Internet gepubliceerd:

- **Implementatie van de aangepaste installatiekopie**: "Containeropslagplaats' uw app in een Docker-installatiekopie die al uw bestanden en afhankelijkheden in een kant-en-klaar-pakket bevat.
- **Implementatie van meerdere containers**: "Containeropslagplaats' uw app in meerdere containers met behulp van een Docker-Compose of een Kubernetes-configuratiebestand. Zie voor meer informatie, [meerdere containers app](#multi-container-apps-supportability).
- **App-implementatie met een ingebouwde platforminstallatiekopie**: onze ingebouwde platforminstallatiekopieën bevatten algemene web-app runtimes en afhankelijkheden, zoals Node en PHP. Gebruik een van de [methoden voor het implementeren van Azure App Service](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) uw app implementeren in uw WebApp-opslag, en vervolgens een ingebouwde platforminstallatiekopie uit te voeren.

## <a name="which-method-is-right-for-your-app"></a>Welke methode is geschikt voor uw app? 

De primaire factoren zijn:

- **Beschikbaarheid van Docker in uw ontwikkelingswerkstroom**: aangepaste installatiekopie ontwikkeling basiskennis hebt van de werkstroom van de ontwikkeling van Docker is vereist. Implementatie van een aangepaste installatiekopie naar een web-app vereist publicatie van uw aangepaste installatiekopie naar een host van de opslagplaats, zoals Docker Hub. Als u bekend bent met Docker en Docker-taken kunt toevoegen aan uw build-werkstroom, of als u al uw app als een Docker-installatiekopie publiceert, wordt een aangepaste afbeelding wordt vrijwel zeker de beste keuze.
- **Architectuur met meerdere lagen**: meerdere containers, zoals het niveau van een web-toepassing en een API-laag voor het scheiden van mogelijkheden met behulp van meerdere containers implementeren. 
- **De prestaties van toepassingen**: verbeteren de prestaties van uw app in meerdere containers met behulp van een cachelaag, zoals Redis. Selecteer meerdere containers om dit te bereiken.
- **De unieke runtime-vereisten**: het ingebouwde platforminstallatiekopieën zijn ontworpen om te voldoen aan de behoeften van de meeste web-apps, maar zijn beperkt in hun aanpassingsmogelijkheden. Uw app hebben unieke afhankelijkheden of andere runtime-vereisten die groter zijn dan wat de ingebouwde installatiekopieën zijn geschikt voor.
- **Bouw vereisten**: met [continue implementatie](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), krijgt u uw app actief en werkend op Azure rechtstreeks vanuit de broncode. Er is geen extern proces voor build of de publicatie is vereist. Er is echter een limiet voor de aanpasbaarheid en de beschikbaarheid van de build-hulpprogramma's in de [Kudu](https://github.com/projectkudu/kudu/wiki) implementatie-engine. Uw app kan de Kudu-mogelijkheden langzamerhand wanneer deze in de afhankelijkheden of vereisten voor het bouwen van aangepaste bedrijfslogica groeit.
- **Schijf lezen/schrijven-vereisten**: alle web-apps een opslagvolume worden toegewezen voor webinhoud. Dit volume, ondersteund door Azure Storage, is gekoppeld aan `/home` in van de app-bestandssysteem. In tegenstelling tot de bestanden in de container-bestandssysteem, bestanden in de inhoud volume toegankelijk zijn voor alle schalingsinstanties van een app en wijzigingen blijven behouden in de app opnieuw wordt opgestart. Echter de latentie van de schijf van het volume van de inhoud hoger is en meer variabele dan de latentie van de container lokaal bestandssysteem en toegang kan worden beïnvloed door het platform upgrades, niet-geplande uitvaltijd en problemen met de netwerkverbinding. Apps waarvoor zware alleen-lezen toegang tot de bestanden met inhoud kunnen van de implementatie van de aangepaste installatiekopie, die bestanden worden geplaatst in het bestandssysteem van de afbeelding in plaats van op het volume van de inhoud profiteren.
- **Bouw Resourcegebruik**: wanneer een app wordt geïmplementeerd vanuit de bron, de implementatiescripts uitgevoerd door Kudu gebruiken dezelfde App Service-Plan reken- en bronnen als de app die wordt uitgevoerd. Meer resources of tijd dan het gewenste mag worden gebruikt voor grote app-implementaties. In het bijzonder genereren veel implementatiewerkstromen zware schijfactiviteit op het volume voor inhoud op app, die niet is geoptimaliseerd voor deze activiteit. Een aangepaste installatiekopie bevat alle van de bestanden en afhankelijkheden van uw app in één pakket hoeft geen extra bestandsoverdrachten of acties voor implementatie naar Azure.
- **Voor snelle iteratie moet**: een app Dockerizing extra build-stappen vereist. Voor de wijzigingen worden doorgevoerd, moet u de nieuwe installatiekopie pushen naar een opslagplaats met elke update. Deze updates worden vervolgens opgehaald voor de Azure-omgeving. Implementeren vanaf de bron kan als een van de ingebouwde containers voldoet aan de behoeften van uw app, een snellere ontwikkelingswerkstroom bieden.

## <a name="multi-container-apps-supportability"></a>Ondersteuning voor meerdere container-apps

### <a name="supported-docker-compose-configuration-options"></a>Ondersteunde configuratieopties van Docker Compose
- command
- entrypoint
- environment
- image
- ports
- restart
- services
- volumes

### <a name="unsupported-docker-compose-configuration-options"></a>Niet-ondersteunde configuratieopties van Docker Compose
- build (niet toegestaan)
- depends_on (genegeerd)
- networks (genegeerd)
- secrets (genegeerd)
- andere poorten dan 80 en 8080 (genegeerd)

> [!NOTE]
> Andere opties die niet expliciet worden genoemd, worden ook genegeerd in de openbare preview.

### <a name="supported-kubernetes-configuration-options"></a>Ondersteunde configuratieopties voor Kubernetes
- argumenten
- command
- containers
- image
- name
- ports
- spec

> [!NOTE]
>Andere Kubernetes-opties die niet expliciet worden genoemd, worden niet ondersteund in de openbare preview.
>
