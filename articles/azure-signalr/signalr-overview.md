---
title: Wat is Azure SignalR
description: Een overzicht van de service Azure SignalR.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: e66326c6c4d93a92c579255cb00b6614ecc03b8c
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255173"
---
# <a name="what-is-azure-signalr-service"></a>Wat is de service Azure SignalR

Met de service Azure SignalR wordt het proces van het toevoegen van realtimewebfunctionaliteit aan toepassingen via HTTP vereenvoudigd. Dankzij deze realtimefunctionaliteit kunnen via de service inhoudsupdates worden gepusht naar verbonden clients, zoals een enkele webpagina of mobiele toepassing. Zo worden clients bijgewerkt zonder de server te hoeven pollen of nieuwe HTTP-bijwerkaanvragen te hoeven verzenden.

In dit artikel vindt u een overzicht van de service Azure SignalR.

## <a name="what-is-azure-signalr-service-used-for"></a>Waarvoor wordt de service Azure SignalR gebruikt?

Er zijn allerlei soorten toepassingen waarvoor inhoud in realtime moet worden bijgewerkt. De volgende voorbeelden zijn bij uitstek geschikt voor het gebruik van de service Azure SignalR:

* Toepassingen die regelmatig moeten worden bijgewerkt vanaf de server. Voorbeelden hiervan zijn apps voor gamen, stemmen, veilingen, kaarten en GPS.
* Dashboards en controletoepassingen. Voorbeelden hiervan zijn bedrijfsdashboards en updates over directe verkopen.
* Toepassingen voor samenwerking. Voorbeelden van toepassingen voor samenwerking zijn whiteboard-apps en vergadersoftware voor teams.
* Toepassingen waarvoor meldingen vereist zijn. Voor sociale netwerken, e-mail, chats, games, reiswaarschuwingen en veel andere apps worden meldingen gebruikt.

SignalR biedt een aantal verschillende technieken voor het bouwen van realtimewebtoepassingen. [WebSockets](https://wikipedia.org/wiki/WebSocket) is de optimale transportmethode, maar als bepaalde opties niet beschikbaar zijn, worden ook andere methoden zoals [SSE (Server-Sent Events)](https://wikipedia.org/wiki/Server-sent_events) en Long Polling gebruikt. In SignalR wordt automatisch de juiste transportmethode gedetecteerd en geïnitialiseerd, afhankelijk van welke functies op de server en de client worden ondersteund.

Daarnaast biedt SignalR een programmeermodel voor realtimetoepassingen dat de server in staat stelt om berichten te verzenden naar alle verbindingen of naar een subset met verbindingen die horen bij een specifieke gebruiker of die in een willekeurige groep zijn geplaatst.

## <a name="how-to-use-azure-signalr-service"></a>De service Azure SignalR gebruiken

Er zijn momenteel drie manieren om de service Azure SignalR te gebruiken:

- **[Een ASP.NET Core SignalR-app schalen](signalr-overview-scale-aspnet-core.md)**: integreer de service Azure SignalR met een ASP.NET Core SignalR-toepassing om uit te schalen naar duizenden verbindingen.
- **[Serverloze realtimeapps bouwen](signalr-overview-azure-functions.md)**: gebruik de integratie van Azure Functions met de service Azure SignalR om serverloze realtimetoepassingen te bouwen in talen zoals JavaScript, C# en Java.
- **[Berichten van de server verzenden naar clients via REST API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)**: de service Azure SignalR biedt REST API waarmee via toepassingen berichten kunnen worden verzonden naar clients die zijn verbonden met de service SignalR, in elke programmeertaal die compatibel is met REST.