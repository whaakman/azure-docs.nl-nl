---
title: Azure Relay-poortinstellingen | Microsoft Docs
description: Meer informatie over Azure Relay-poort waarden.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 055f04d496b56a5e8542911aa78292d7746ae80b
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-relay-port-settings"></a>Azure Relay-poortinstellingen

De volgende tabel beschrijft de vereiste configuratie voor poortwaarden voor Azure Relay.

## <a name="hybrid-connections"></a>Hybride verbindingen
Hybride verbindingen WebSockets gebruikt als onderliggend transportmechanisme, dat gebruikmaakt van **HTTPS** alleen. 

## <a name="wcf-relays"></a>WCF-relays
  
|Binding|Transportbeveiliging|Poort|  
|-------------|------------------------|----------|  
|[Klasse BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (client)|Ja|HTTPS| 
| |" |Nee|HTTP|  
|[Klasse BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (service)|Beide|9351/HTTP|  
|[Klasse NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (client)|Ja|9351/HTTPS|  
||" |Nee|9350/HTTP|  
|[Klasse NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (service)|Beide|9351/HTTP|  
|[NetTcpRelayBinding klasse](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (client/service)|Beide|9352-5671/HTTP (9352/9353 als hybride)|  
|[Klasse NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (client)|Ja|9351/HTTPS|  
||" |Nee|9350/HTTP|  
|[Klasse NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (service)|Beide|9351/HTTP|  
|[Klasse WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (client)|Ja|HTTPS|  
||" |Nee|HTTP|  
|[Klasse WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (service)|Beide|9351/HTTP|  
|[Klasse WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (client)|Ja|HTTPS|  
||" |Nee|HTTP|  
|[Klasse WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (service)|Beide|9351/HTTP|

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Veelgestelde vragen over Relay](relay-faq.md)