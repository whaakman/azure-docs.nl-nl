---
title: Berichten en verbindingen in Azure SignalR
description: Een overzicht van de belangrijkste concepten met betrekking tot berichten en verbindingen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: c2348df7a1a55584807a03216e294486ddadfc52
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352594"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Berichten en verbindingen in Azure SignalR Service

Het factureringsmodel van Azure SignalR Service is gebaseerd op het aantal verbindingen en het aantal berichten. Hieronder wordt uitgelegd hoe berichten en verbindingen worden gedefinieerd en geteld voor factureringsdoeleinden.

## <a name="message-formats-supported"></a>Ondersteunde berichtindelingen

Azure SignalR Service ondersteunt dezelfde indelingen als ASP.NET Core SignalR: [JSON](https://www.json.org/) en [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Berichtgrootte

Er is geen groottelimiet voor Azure SignalR Service.

In de praktijk worden grote berichten opgesplitst in kleinere berichten van niet meer dan 2 kB per stuk, die als afzonderlijke berichten worden verzonden. Het splitsen en samenvoegen van berichten wordt afgehandeld via SDK's. Ontwikkelaars hoeven er niets voor te doen.

Grote berichten hebben echter een negatief effect op de berichtprestaties. Gebruik indien mogelijk een kleinere berichtgrootte en voer tests uit om de optimale berichtgrootte voor elk gebruiksscenario te kiezen.

## <a name="how-to-count-messages-for-billing-purpose"></a>Hoe worden berichten geteld voor factureringsdoeleinden?

We tellen alleen de uitgaande berichten van SignalR Service en negeren de ping-berichten tussen clients en servers.

Een bericht groter dan 2 kB wordt beschouwd als meerdere berichten van 2 kB per stuk. De grafiek met het aantal berichten in Azure Portal wordt telkens elke 100 berichten per hub bijgewerkt.

Stel u hebt drie clients en één toepassingsserver. Vanaf één client wordt een bericht van 4 kB verzonden dat door de server moet worden uitgezonden naar alle clients. Het aantal berichten is 8: één bericht van de service naar de toepassingsserver en drie berichten van de service naar clients, waarbij elk bericht wordt geteld als twee berichten van 2 kB.

Het aantal berichten dat in Azure Portal wordt weergegeven blijft 0 totdat het aantal boven de 100 komt.

## <a name="how-to-count-connections"></a>Hoe worden de verbindingen geteld?

Er zijn serververbindingen en clientverbindingen. Standaard heeft elke toepassingsserver per hub vijf verbindingen met SignalR Service en heeft elke client één clientverbinding met SignalR Service.

Het aantal verbindingen dat in Azure Portal wordt weergegeven, betreft zowel server- als clientverbindingen.

Stel dat u twee toepassingsservers hebt en vijf hubs in codes definieert. Het aantal serververbindingen is 50: 2 toepassingsservers * 5 hubs * 5 verbindingen/hub.

ASP.NET SignalR berekent serververbindingen op een andere manier. Het heeft naast door klanten gedefinieerde hubs één standaardhub. Elke toepassingsserver moet standaard vijf extra serververbindingen hebben. Het aantal verbindingen voor de standaardhub blijft in overeenstemming met andere hubs.

## <a name="how-to-count-inbound-traffic--outbound-traffic"></a>Binnenkomend verkeer/uitgaand verkeer tellen

Binnenkomend/uitgaand is vanuit het perspectief van SignalR Service. het verkeer wordt geteld in bytes. Net zoals het aantal berichten, heeft het verkeer ook een samplefrequentie. De grafiek Binnenkomend/uitgaand in de Azure-portal wordt elke 100 kB per hub bijgewerkt.

## <a name="related-resources"></a>Gerelateerde resources

- [Aggregatietype in Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR-configuratie](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)