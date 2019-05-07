---
title: Integratie met App Service Mobile Apps
description: Meer informatie over de werking van Azure Notification Hubs met Azure App Service Mobile Apps.
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: 83132dff-a01d-4b31-a426-b57496852b81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: bb39949bc21ece819b7ac6e279390f8bdb0ee00a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158523"
---
# <a name="integration-with-app-service-mobile-apps"></a>Integratie met App Service Mobile Apps

> [!NOTE]
> Microsoft is gericht op de volledige ondersteuning van Azure App Service Mobile Apps, inclusief ondersteuning voor de meest recente versie van het besturingssysteem, oplossingen voor problemen, verbeteringen van documentatie en community pull-aanvraag beoordeelt. Houd er rekening mee dat het productteam niet op dit moment in een nieuwe functie voor Azure Mobile Apps investeert. We waarderen maximaal community bijdragen tot alle gebieden van Azure Mobile Apps.

Voor een naadloze werking van alle Azure-services biedt [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) ingebouwde ondersteuning voor pushmeldingen via Notification Hubs. Met [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) kunnen ontwikkelaars van ondernemingen en systeemintegrators gebruikmaken van een zeer schaalbaar, algemeen beschikbaar ontwikkelplatform voor mobiele toepassingen, dat uitgebreide mogelijkheden biedt voor ontwikkelaars van mobiele apps.

Mobiele apps van ontwikkelaars kunnen gebruikmaken van Notification Hubs in de volgende werkstroom:

1. PNS-ingang van het apparaat ophalen
2. Apparaat registreren bij Notification Hubs via de handige registratie-API uit de Client-SDK van Mobile Apps

    > [!NOTE]
    > Om beveiligingsreden worden alle tags op registraties door mobiele apps verwijderd. Gebruik Notification Hubs om vanuit de back-end rechtstreeks tags aan apparaten koppelen.

3. Meldingen verzenden vanuit de back-end van uw app met Notification Hubs

Hier zijn een aantal voordelen voor ontwikkelaars:

- **Client SDK's voor mobiele apps**: Deze SDK's voor meerdere platforms bieden eenvoudige API's voor de registratie en het contact met de Notification Hub die automatisch aan de mobiele app is gekoppeld. Ontwikkelaars hoeven niet op zoek naar de aanmeldgegevens voor Notification Hubs of met een extra service te werken.
  - *Pushen naar gebruiker*: De SDK's coderen een apparaat automatisch met de geverifieerde gebruikers-id van de mobiele app om het verzenden van pushberichten mogelijk te maken.
  - *Pushen naar apparaat*: De SDK's gebruiken de installatie-id van een mobiele app automatisch als GUID om te registreren bij Notification Hubs. Hierdoor hoeven ze niet meerdere service-GUID's meer bij te houden.
- **Installatiemodel**: Mobiele apps werken met het meest recente pushmodel van Notification Hubs. Hierdoor zijn alle pusheigenschappen beschikbaar die aan een apparaat zijn gekoppeld in een JSON-installatie, die aansluit op de Push Notification Services en eenvoudig te gebruiken is.
- **Flexibiliteit**: Ontwikkelaars kunnen altijd beslissen om rechtstreeks met Notification Hubs te werken, zelfs na integratie.
- **Geïntegreerde ervaring in [Azure-portal](https://portal.azure.com)**: De pushfunctie in mobiele apps is zeer visueel en ontwikkelaars kunnen eenvoudig via mobiele apps met de bijbehorende Notification Hub werken.
