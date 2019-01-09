---
title: Berichten en verbindingen in Azure SignalR
description: Een overzicht van de belangrijkste concepten met betrekking tot berichten en verbindingen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 5a0430e9ad124319147342c49fc51e11472ac8ff
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53812710"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Berichten en verbindingen in Azure SignalR Service

Het factureringsmodel van Azure SignalR Service is gebaseerd op het aantal verbindingen en het aantal berichten. Hieronder wordt uitgelegd hoe de berichten en verbindingen worden gedefinieerd en geteld voor factureringsdoeleinden.

## <a name="message-formats-supported"></a>Ondersteunde berichtindelingen

Azure SignalR Service ondersteunt dezelfde indelingen als ASP.NET Core SignalR: [JSON](https://www.json.org/) en [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Berichtgrootte

Er is geen groottelimiet voor Azure SignalR Service.

In de praktijk worden grote berichten opgesplitst in kleinere berichten van niet meer dan 2 kB per stuk, die als afzonderlijke berichten worden verzonden. Het splitsen en samenvoegen van berichten wordt afgehandeld via SDK's. Ontwikkelaars hoeven er niets voor te doen.

Grote berichten hebben echter een negatief effect op de berichtprestaties. Gebruik indien mogelijk een kleinere berichtgrootte en voer tests uit om de optimale berichtgrootte voor elk gebruiksscenario te kiezen.

## <a name="how-to-count-messages-for-billing-purpose"></a>Hoe worden berichten geteld voor factureringsdoeleinden?

We tellen alleen de uitgaande berichten van SignalR Service en negeren de ping-berichten tussen clients en servers.

Een bericht groter dan 2 kB wordt beschouwd als meerdere berichten van 2 kB per stuk. De grafiek met het aantal berichten in Azure Portal wordt steeds per 100 berichten per hub bijgewerkt.

Stel dat een gebruiker 3 clients en 1 toepassingsserver heeft. Vanaf één client wordt een bericht van 4 kB verzonden dat door de server moet worden uitgezonden naar alle clients. Het aantal berichten wordt 8: 1 bericht van de service naar de toepassingsserver en 3 berichten van de service naar clients, waarbij elk bericht wordt geteld als 2 berichten van 2 kB.

Het aantal berichten dat in Azure Portal wordt weergegeven blijft 0 totdat het aantal boven de 100 komt.

## <a name="how-to-count-connections"></a>Hoe worden de verbindingen geteld?

Er zijn serververbindingen en clientverbindingen. Standaard heeft elke toepassingsserver per hub 5 verbindingen met SignalR Service en heeft elke client 1 clientverbinding met SignalR Service.

Het aantal verbindingen dat in Azure Portal wordt weergegeven, betreft zowel server- als clientverbindingen.

Stel dat een gebruiker twee toepassingsservers heeft en 5 hubs in codes definieert. Het aantal serversverbindingen dat in Azure Portal wordt weergegeven, is 2 appservers * 5 hubs * 5 verbindingen per hub = 50 serververbindingen.

## <a name="related-resources"></a>Gerelateerde resources

- [ASP.NET Core SignalR-configuratie](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)