---
title: Azure App Service op de implementatie van de Linux - platform ingebouwde of aangepaste afbeelding?  | Microsoft Docs
description: U moet kiezen tussen aangepaste implementatie voor Docker-container en een ingebouwde toepassingsframework voor App-Service op Linux
keywords: Azure app service, web-app, linux, oss
services: app-service
documentationCenter: 
authors: nickwalk
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: nickwalk
ms.openlocfilehash: 4a04bba2375b5a107bc3cb8cdc1a75d037c50af6
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="custom-image-or-built-in-platform-image"></a>Aangepaste installatiekopie of ingebouwde platforminstallatiekopie?

[App-Service op Linux](app-service-linux-intro.md) biedt twee verschillende paden voor het uitvoeren van uw toepassing op het web gepubliceerd:

- **Implementatie van de aangepaste installatiekopie**: 'Dockerize' uw app in een Docker-installatiekopie die alle uw bestanden en afhankelijkheden in een kant-en-klaar-pakket bevat.
- **App-implementatie met een ingebouwde platforminstallatiekopie**: onze ingebouwde platform-installatiekopieën bevatten algemene web app runtimes en afhankelijkheden, zoals knooppunt en PHP. Gebruik een van de [methoden voor het implementeren van Azure App Service](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) naar uw app implementeren in uw web-app-opslag en gebruik vervolgens een ingebouwde platforminstallatiekopie uit te voeren.

Welke methode is geschikt voor uw app? De primaire factoren zijn:

- **Beschikbaarheid van Docker in uw werkstroom ontwikkeling**: aangepaste installatiekopie ontwikkeling vereist basiskennis van de werkstroom Docker-ontwikkeling. Implementatie van een aangepaste installatiekopie in een web-app moet de publicatie van uw aangepaste installatiekopie naar een host opslagplaats zoals Docker-Hub. Als u bekend bent met Docker en Docker taken kunt toevoegen aan uw werkstroom build, of als u al uw app als een Docker-installatiekopie publiceert, wordt een aangepaste installatiekopie is bijna zeker de beste keuze.
- **Unieke runtime-vereisten**: de installatiekopieën van het ingebouwde platform zijn ontworpen om te voldoen aan de behoeften van de meeste web-apps, maar zijn beperkt in hun aanpassingsmogelijkheden. Uw app hebt unieke afhankelijkheden of andere runtime-vereisten die groter is dan wat de installatiekopieën van het ingebouwde geschikt zijn.
- **Vereisten bouwen**: met [continue implementatie](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), kunt u uw app actief en werkend op Azure rechtstreeks vanuit de broncode. Er is geen externe build of publicatie proces vereist. Er is echter een limiet aan de aanpassingsmogelijkheden en beschikbaarheid van build-hulpprogramma's binnen de [Kudu](https://github.com/projectkudu/kudu/wiki) implementatie-engine. Uw app kan de mogelijkheden van Kudu langzamerhand wanneer deze in de bijbehorende afhankelijkheden of vereisten voor aangepaste build logica groeit.
- **Schijfruimte die lezen/schrijven**: alle web-apps een opslagvolume toegewezen voor webinhoud. Dit volume, ondersteund door Azure Storage is gekoppeld aan `/home` in de app-bestandssysteem. In tegenstelling tot bestanden in het bestandssysteem container bestanden in het volume van de inhoud toegankelijk zijn in alle exemplaren van de schaal van een app en wijzigingen persistent is voor de app opnieuw wordt opgestart. De latentie van de schijf van het volume van de inhoud hoger is echter en meer variabele dan de latentie van de lokale container bestandssysteem en de toegang kan worden beïnvloed door het platform upgrades, niet-geplande uitvaltijd en problemen met de netwerkverbinding. Apps waarvoor zware alleen-lezen toegang tot de inhoudsbestanden kunnen profiteren van de implementatie van de aangepaste installatiekopie, die bestanden worden geplaatst in het bestandssysteem van de afbeelding in plaats van op het volume van de inhoud.
- **Resourcegebruik bouwen**: wanneer een app wordt geïmplementeerd van bron, de implementatiescripts uitgevoerd door Kudu gebruik dezelfde App Service-Plan berekenings- en bronnen als de actieve app. Grote app-implementaties mogelijk meer bronnen of tijd dan het gewenste verbruiken. In het bijzonder veel implementatiewerkstromen genereren zware schijfactiviteit op de app inhoud volume niet is geoptimaliseerd voor deze activiteit. Een aangepaste installatiekopie biedt alle bestanden en afhankelijkheden van uw app naar Azure in een enkel pakket met extra bestandsoverdrachten of acties voor de implementatie niet vereist.
- **Nodig voor snelle herhaling**: een app Dockerizing build extra stappen vereist. Wijzigingen van kracht te laten, moet u de nieuwe installatiekopie naar een opslagplaats bij elke update push. Deze updates worden vervolgens opgehaald voor de Azure-omgeving. Implementeren vanaf de bron kan als een van de ingebouwde containers voldoet aan de behoeften van uw app, een snellere ontwikkeling werkstroom bieden.