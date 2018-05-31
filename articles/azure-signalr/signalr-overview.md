---
title: Wat is Azure SignalR | Microsoft Docs
description: Een overzicht van de service Azure SignalR.
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/17/2018
ms.author: wesmc
ms.openlocfilehash: bc144fb1d7db9251871e7e181b012417a32de7e6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33868106"
---
# <a name="what-is-azure-signalr-service"></a>Wat is de service Azure SignalR

De Microsoft Azure SignalR-service is momenteel beschikbaar in de [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

De service Azure SignalR is een Azure-service op basis van [ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction). ASP.NET Core SignalR is een [open-source-bibliotheek](https://github.com/aspnet/signalr) die het proces van het toevoegen van realtime webfunctionaliteit aan toepassingen via HTTP vereenvoudigt. Met deze functionaliteit kan bijgewerkte inhoud op de webserver in realtime naar verbonden clients worden gepusht. Zo worden clients bijgewerkt zonder de server te hoeven pollen of nieuwe HTTP-bijwerkaanvragen te hoeven verzenden.

In dit artikel vindt u een overzicht van de service Azure SignalR. Als u aan de slag wilt met deze service, begint u met de [snelstart voor ASP.NET Core](signalr-quickstart-dotnet-core.md).

## <a name="what-is-signalr-service-used-for"></a>Waarvoor wordt SignalR gebruikt? 

Er zijn allerlei soorten toepassingen waarvoor inhoud in realtime moet worden bijgewerkt. De volgende soorten toepassingen zijn bij uitstek geschikt voor het gebruik van de service Azure SignalR:

* Toepassingen die regelmatig moeten worden bijgewerkt vanaf de server. Voorbeelden hiervan zijn games, sociale netwerken, toepassingen voor stemmen, veilingen, kaarten en GPS-apps.
* Dashboards en controletoepassingen. Voorbeelden hiervan zijn bedrijfsdashboards en toepassingen voor direct bijgewerkte verkopen of waarschuwingen op reis.
* Toepassingen voor samenwerking. Voorbeelden van toepassingen voor samenwerking zijn whiteboard-apps en vergadersoftware voor teams.
* Toepassingen waarvoor meldingen vereist zijn. Voor sociale netwerken, e-mail, chats, games, reiswaarschuwingen en veel andere apps worden meldingen gebruikt.

In SignalR wordt een aantal verschillende technieken gebruikt voor het bouwen van realtime-webtoepassingen. [WebSockets](https://wikipedia.org/wiki/WebSocket) is de optimale transportmethode, maar als bepaalde opties niet beschikbaar zijn, worden ook andere methoden zoals [SSE (Server-Sent Events)](https://wikipedia.org/wiki/Server-sent_events) en Long Polling gebruikt. In SignalR wordt automatisch de juiste transportmethode gedetecteerd en geïnitialiseerd, afhankelijk van welke functies op de server en de client worden ondersteund.

## <a name="developing-signalr-apps"></a>SignalR-apps ontwikkelen

Er zijn momenteel twee versies van SignalR die u kunt gebruiken voor uw webtoepassingen: SignalR voor ASP.NET en ASP.NET Core SignalR, de nieuwste versie. De service Azure SignalR is een beheerde Azure-service op basis van ASP.NET Core SignalR. 

ASP.NET Core SignalR is gebaseerd op de vorige versie, maar is herschreven. ASP.NET Core SignalR is daardoor niet achterwaarts compatibel met de oudere versie van SignalR. De API's en het gedrag ervan zijn anders. De SDK van ASP.NET Core SignalR is .NET Standard zodat u deze nog steeds kunt gebruiken in combinatie met .NET Framework. U moet dan wel de nieuwe API's gebruiken in plaats van de oude. Als u SignalR gebruikt en wilt overstappen op ASP.NET Core SignalR of de service Azure SignalR, moet u uw programmacode aanpassen aan de verschillen in de API's.

Bij de service Azure SignalR wordt het servergedeelte van ASP.NET Core SignalR gehost in Azure. Maar omdat de technologie is gebaseerd op ASP.NET Core, hebt u de mogelijkheid om uw werkelijke webtoepassing uit te voeren op meerdere platforms (Windows, Linux en Mac OS) terwijl deze wordt gehost in [Azure App Service](../app-service/app-service-web-overview.md), [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache) of [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index). U kunt de toepassing ook zelf hosten in uw eigen proces.

Als u met uw toepassing ondersteuning wilt bieden voor de meest recente functionaliteit voor het bijwerken van webclients met inhoud in realtime, en u de toepassing wilt uitvoeren op meerdere platforms (Azure, Windows, Linux en Mac OS) en wilt hosten in verschillende omgevingen, is Azure SignalR de beste keuze.


## <a name="why-not-deploy-signalr-myself"></a>Waarom zou ik SignalR niet zelf implementeren?

U kunt nog steeds uw eigen Azure-web-app met ondersteuning voor ASP.NET Core SignalR implementeren als onderdeel van de back-end van uw algemene webtoepassing.

Een van de belangrijkste redenen om de service Azure SignalR te gebruiken, is de eenvoud. Met de service Azure SignalR hoeft u zich niet te bekommeren om zaken als prestaties, schaalbaarheid en beschikbaarheid. Deze zaken worden voor u geregeld met een SLA van 99,9%.

WebSockets is ook de voorkeursmethode voor het bijwerken van inhoud in realtime. De taakverdeling van een groot aantal persistente WebSocket-verbindingen wordt echter een complexe puzzel als u de schaal wilt aanpassen. Veelgebruikte oplossingen hiervoor zijn taakverdeling via DNS, hardware en software. Dit probleem wordt automatisch voor u afgehandeld met de service Azure SignalR.

Een andere reden kan zijn dat u eigenlijk helemaal geen webtoepassing hoeft te hosten. De logica van uw webtoepassing is wellicht geschikt voor [serverloze computing](https://azure.microsoft.com/overview/serverless-computing/). Mogelijk wordt uw code alleen gehost en uitgevoerd op aanvraag met [Azure Functions](https://docs.microsoft.com/azure/azure-functions/)-triggers. Dit scenario kan lastig zijn omdat de code alleen op aanvraag wordt uitgevoerd en er geen lange verbindingen met clients worden onderhouden. Azure SignalR is geknipt voor deze situatie, omdat verbindingen al voor u worden beheerd via deze service.

## <a name="how-does-it-scale"></a>Hoe werkt het schalen?

Het is gebruikelijk om SignalR te schalen met SQL Server, Azure Service Bus of Redis Cache. De schaalmethode wordt voor u beheerd met de service Azure SignalR. De prestaties en kosten van deze service zijn vergelijkbaar met die van de andere services, maar dan zonder de complexiteit van het beheer van die services. U hoeft alleen maar het aantal eenheden voor uw service bij te werken. Elke service-eenheid ondersteunt maximaal 1000 clientverbindingen.

## <a name="next-steps"></a>Volgende stappen
* [Snelstart: Een chatruimte maken met Azure SignalR](signalr-quickstart-dotnet-core.md)  
  

