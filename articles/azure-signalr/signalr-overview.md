---
title: Wat is Azure SignalR Service?
description: Een overzicht van de Azure SignalR-Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 06/20/2019
ms.author: zhshang
ms.openlocfilehash: dc7ba3585ec49921c0a0e66185fc5550d3d4a006
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303624"
---
# <a name="what-is-azure-signalr-service"></a>Wat is Azure SignalR Service?

Met de service Azure SignalR wordt het proces van het toevoegen van realtimewebfunctionaliteit aan toepassingen via HTTP vereenvoudigd. Dankzij deze realtimefunctionaliteit kunnen via de service inhoudsupdates worden gepusht naar verbonden clients, zoals een enkele webpagina of mobiele toepassing. Zo worden clients bijgewerkt zonder de server te hoeven pollen of nieuwe HTTP-bijwerkaanvragen te hoeven verzenden.


In dit artikel vindt u een overzicht van de service Azure SignalR.

## <a name="what-is-azure-signalr-service-used-for"></a>Waarvoor wordt de service Azure SignalR gebruikt?

Elk scenario waarvoor het pushen van gegevens van server naar de client in realtime, kan Azure SignalR Service kunt gebruiken.

Traditionele realtime functies die vaak polling van server vereisen, kunt ook Azure SignalR Service gebruiken.

Azure SignalR-Service is gebruikt in een groot aantal bedrijfstakken, voor elk toepassingstype waarvoor realtime inhoudsupdates. Wij enkele voorbeelden die geschikt zijn voor het gebruik van Azure SignalR Service zijn in de lijst:

* **Hoge frequentie Gegevensupdates:** spelen van games, stemmen, polling, veiling.
* **Dashboards en -bewaking:** bedrijfsgegevens dashboard, financiële markt, bijwerking van directe verkoop, game ranglijst voor meerdere spelers en bewaking van IoT.
* **Chat:** live chatruimte, chatbot, online klantondersteuning, realtime winkelwagen assistant, messenger, in-game chat, enzovoort.
* **Realtime locatie op de kaart:** logistieke bijhouden, levering status bijhouden, transport statusupdates, GPS-apps.
* **Realtime gerichte advertenties:** gepersonaliseerde lezen tijd push advertenties en aanbiedingen, interactieve advertenties.
* **Apps in samenwerkingsverband:** cocreatie, whiteboard apps en voldoen aan de software-team.
* **Pushmeldingen verzenden:** sociale netwerken, e-mail, game, reizen waarschuwing.
* **Realtime broadcasting:** live uitzenden audio/video, live uitzenden van closed captioning, vertalen, gebeurtenissen/nieuws.
* **IoT en verbonden apparaten:** realtime IoT-metrische gegevens, beheer op afstand, realtimestatussen en locatie bijhouden.
* **Automation:** realtime trigger van de upstream-gebeurtenissen.

## <a name="what-are-the-benefits-using-azure-signalr-service"></a>Wat zijn de voordelen met Azure SignalR Service?

**Standaard op basis van:**

SignalR biedt een aantal verschillende technieken voor het bouwen van realtimewebtoepassingen. [WebSockets](https://wikipedia.org/wiki/WebSocket) is de optimale transportmethode, maar als bepaalde opties niet beschikbaar zijn, worden ook andere methoden zoals [SSE (Server-Sent Events)](https://wikipedia.org/wiki/Server-sent_events) en Long Polling gebruikt. In SignalR wordt automatisch de juiste transportmethode gedetecteerd en geïnitialiseerd, afhankelijk van welke functies op de server en de client worden ondersteund.

**Systeemeigen ondersteuning voor ASP.NET Core:**

SignalR-Service biedt systeemeigen programmeren met ASP.NET Core en ASP.NET. Het ontwikkelen van nieuwe SignalR-toepassing met SignalR-Service of migreren van bestaande SignalR is op basis van de toepassing met SignalR-Service vereist minimale inspanningen.
SignalR-Service biedt ook ondersteuning voor nieuwe functie van ASP.NET Core, Server-side Blazor.

**Clientondersteuning voor brede:**

SignalR-Service werkt met een breed scala aan clients, zoals web en mobiele browsers, bureaublad-apps, mobiele apps, server-proces, IoT-apparaten en spelconsoles. SignalR-Service biedt SDK's in verschillende talen. Naast de systeemeigen ASP.NET Core- of ASP.NET C# SDK's, SignalR-Service biedt ook JavaScript-client-SDK, om in te schakelen webclients en veel JavaScript-frameworks. SDK voor Java-client wordt ook ondersteund voor Java-toepassingen, met inbegrip van systeemeigen Android-apps. REST-API biedt ondersteuning voor SignalR-Service en serverloze via integratie met Azure Functions en Event Grid.

**Verwerken van grootschalige clientverbindingen:**

SignalR-Service is ontworpen voor grootschalig realtime-toepassingen. SignalR-Service kunnen meerdere exemplaren kunnen samenwerken om te schalen naar miljoenen-clientverbindingen. De service ondersteunt ook meerdere wereldwijde regio's voor sharding, hoge beschikbaarheid en noodherstel.

**De overhead voor het hosten van SignalR zelf verwijderen:**

Vergeleken met de zelf-hostend SignalR-toepassingen, wordt overschakelen naar SignalR-Service de noodzaak voor het beheren van vorige vlakken die verwerking van de schaal en -clientverbindingen. De volledig beheerde service ook webtoepassingen vereenvoudigt en worden opgeslagen die als host fungeert kosten. SignalR-Service biedt wereldwijd bereik en eersteklas datacenter en netwerk, kan worden geschaald naar miljoenen verbindingen, garandeert SLA, terwijl alle naleving en beveiliging op Azure standard.

![Beheerde SignalR-Service](./media/signalr-overview/managed-signalr-service.png)

**Bieden rijke API's voor verschillende berichtpatronen:**

SignalR-Service kan de server berichten te verzenden naar een bepaalde verbinding, alle verbindingen, of een subset van verbindingen die deel uitmaken van een specifieke gebruiker of in een willekeurige groep zijn geplaatst.

## <a name="how-to-use-azure-signalr-service"></a>De service Azure SignalR gebruiken

Er zijn veel verschillende manieren om te programmeren met Azure SignalR Service als sommige van de voorbeelden die hier worden vermeld:

- **[Een ASP.NET Core SignalR-app schalen](signalr-concept-scale-aspnet-core.md)** : integreer de service Azure SignalR met een ASP.NET Core SignalR-toepassing om uit te schalen naar duizenden verbindingen.
- **[Serverloze realtimeapps bouwen](signalr-concept-azure-functions.md)** : gebruik de integratie van Azure Functions met de service Azure SignalR om serverloze realtimetoepassingen te bouwen in talen zoals JavaScript, C# en Java.
- **[Berichten van de server verzenden naar clients via REST API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)** : de service Azure SignalR biedt REST API waarmee via toepassingen berichten kunnen worden verzonden naar clients die zijn verbonden met de service SignalR, in elke programmeertaal die compatibel is met REST.