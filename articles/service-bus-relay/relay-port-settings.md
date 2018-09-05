---
title: Azure Relay-poortinstellingen | Microsoft Docs
description: Als u meer informatie over Azure Relay-poortwaarden.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: spelluru
ms.openlocfilehash: 3ef08cfc94a029f97250578e9b0366a18770c809
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696733"
---
# <a name="azure-relay-port-settings"></a>Azure Relay-poortinstellingen

De volgende tabel beschrijft de vereiste configuratie voor poortwaarden voor de Azure Relay.

## <a name="hybrid-connections"></a>Hybride verbindingen

Hybride verbindingen gebruikmaakt van WebSockets op poort 443 met SSL als onderliggend transportmechanisme, dat gebruikmaakt van **HTTPS** alleen. 

## <a name="wcf-relays"></a>WCF-relays
  
|Binding|Transport Security|Poort|  
|-------------|------------------------|----------|  
|[Klasse BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (client)|Ja|HTTPS| 
|" |Nee|HTTP|  
|[Klasse BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (service)|Een van beide|9351/HTTP|  
|[Klasse NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (client)|Ja|9351/HTTPS|  
|" |Nee|9350/HTTP|  
|[Klasse NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (service)|Een van beide|9351/HTTP|  
|[NetTcpRelayBinding klasse](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (client/service)|Een van beide|5671/9352/HTTP (9352/9353 als hybride)|  
|[Klasse NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (client)|Ja|9351/HTTPS|  
|" |Nee|9350/HTTP|  
|[Klasse NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (service)|Een van beide|9351/HTTP|  
|[Klasse WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (client)|Ja|HTTPS|  
|" |Nee|HTTP|  
|[Klasse WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (service)|Een van beide|9351/HTTP|  
|[Klasse WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (client)|Ja|HTTPS|  
|" |Nee|HTTP|  
|[Klasse WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (service)|Een van beide|9351/HTTP|

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Veelgestelde vragen over Relay](relay-faq.md)