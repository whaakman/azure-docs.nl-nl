---
title: Ik gebruik Mobile Services. Welke voordelen heeft App Service voor mijn bestaande projecten?
description: Lees welke voordelen App Service heeft voor uw bestaande Mobile Services-projecten.
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 22397b6b448b418d5b54a457c3bafaf5c68ecc7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started"> </a>Ik gebruik Mobile Services. Welke voordelen heeft App Service voor mijn bestaande projecten?
## <a name="overview"></a>Overzicht
Uw bestaande Mobile Service-projecten zijn veilig en blijven ondersteund. Er zijn echter een aantal voordelen die het *Azure App Service*-platform biedt voor uw mobiele app, die nu niet beschikbaar zijn in Mobile Services:

* Eenvoudigere en efficiëntere aanbieding voor apps die zowel web- als mobiele clients bevat
* Nieuwe hostfuncties, waaronder WebJobs, aangepaste CNames en betere bewaking
* Directe integratie met Traffic Manager
* Verbinding met uw on-premises resources en VPN-netwerken met VNet naast hybride verbindingen
* Bewaking, waarschuwingen en probleemoplossing voor uw app met NewRelic of AppInsights
* Uitgebreider spectrum van de onderliggende rekenresources en prijzen
* Ingebouwde automatische schaling, taakverdeling en prestatiebewaking
* Ingebouwde mogelijkheden voor fasering, terugdraaien en testen in productieomgeving

## <a name="new-hosting-features"></a>Nieuwe hostingfuncties
In *Azure App Service* wordt de code voor de back-end van *mobiele apps* in dezelfde container uitgevoerd als web-apps en API-apps. U kunt dus profiteren van alle functies in deze container, inclusief een aantal functies dat momenteel niet aanwezig is in Mobile Services:

* Continu actieve back-endlogica toevoegen via WebJobs
* Ervoor zorgen dat uw back-endcode altijd wordt uitgevoerd
* Aangepaste CNames opgeven, zodat u beschrijvende en stabiele namen voor uw mobiele back-endeindpunten kunt gebruiken
* Geo-schaling van uw app met Traffic Manager
* Alle gewenste bibliotheken en pakketten opnemen
* (Voor .NET) Gebruikmaken van alle functies van ASP.NET, waaronder MVC
* (Voor Node.js) Gebruikmaken van alle pure JavaScript-bibliotheken van het Node-ecosysteem, waaronder algemene MVC-bibliotheken

## <a name="access-on-premises-data-using-vnet"></a>Toegang tot on-premises gegevens met behulp van VNet
Met Mobile Services kunt u hybride verbindingen gebruiken voor toegang tot on-premises resources. Er zijn echter situaties waarbij een VPN-oplossing de voorkeur verdient. Met *Azure App Service* kunt u Azure VNet voor de back-endcode van uw mobiele app gebruiken.

## <a name="use-your-favorite-backend-language"></a>Uw favoriete taal voor back-ends gebruiken
*De Azure App Service* biedt bredere en uitgebreidere ondersteuning voor ASP.NET- en Node.js-platforms, waaronder toegang tot de meest recente runtimes.

## <a name="set-up-automatic-scale"></a>Automatische schaling instellen
Met Mobile Services werden alle exemplaren van uw back-endcode op kleine virtuele machines uitgevoerd. Met *Azure App Service* hebt u veel meer keuze bij het kiezen van de grootte van de virtuele machines. Daarnaast kunt u snel omhoog of omlaag schalen om in te spelen op de inkomende belasting van klanten, gebaseerd op verschillende maatstaven voor prestaties.

## <a name="be-in-the-know"></a>Blijf op de hoogte
U kunt realtime op problemen reageren met bewaking en waarschuwingen, waarmee u en uw team automatisch op de hoogte worden gebracht van problemen. Integreer geavanceerde app-analyses met de bewakingsfunctionaliteit van New Relic en AppInsights, zodat u nog meer inzicht krijgt in de prestaties van uw mobiele app. Met *Azure App Service* kunt u nu waarschuwingen instellen op basis van verschillende maatstaven voor prestaties. Dit kunt u via programmacode of via Azure Portal doen.

## <a name="keep-your-assets-safe"></a>Houd uw activa veilig
U kunt automatisch een back-up maken van uw back-end en database. Uw code en gegevens zijn veilig na noodgevallen en kunnen eenvoudig worden hersteld, zodat u uw bedrijf met vertrouwen kunt runnen.

## <a name="ready-stage-go"></a>Klaar voor de start, testen en af!
Met *Azure App Service* kunt u nu meerdere persoonlijke test- en faseringsomgevingen maken voor uw mobiele apps. Gebruik deze omgevingen om apps te testen voordat u ze implementeert. U kunt zonder uitvaltijd wisselen naar de productieomgeving. Web-apps worden vooraf geladen, zodat de beste gebruikerservaring wordt geboden.

Volg deze [zelfstudie](app-service-mobile-migrating-from-mobile-services.md) als u ook wilt profiteren van alle voordelen die *App Service* biedt voor uw bestaande Mobile Service-projecten.
