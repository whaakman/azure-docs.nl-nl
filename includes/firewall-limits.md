---
title: bestand opnemen
description: bestand opnemen
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 5/3/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8709d4d903bd31ff94d04ec61e226857e4190407
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150301"
---
| Resource | Standaardlimiet |
| --- | --- |
| Doorvoer van gegevens |30 Gbps<sup>1</sup> |
|Regels|10.000, regel alle typen gecombineerd.|
|Minimumgrootte AzureFirewallSubnet |/26|
|Poortbereik in netwerk- en toepassingsregels|0-64,000. Werk wordt uitgevoerd op deze beperking te versoepelen.|
|Routetabel|AzureFirewallSubnet heeft standaard een route 0.0.0.0/0 met de waarde NextHopType is ingesteld op **Internet**.<br><br>Firewall van Azure moet directe verbinding met Internet hebben. Als uw AzureFirewallSubnet een standaardroute naar uw on-premises netwerk via BGP achterhaalt, moet u deze overschrijven met een UDR 0.0.0.0/0 met de **NextHopType** waarde ingesteld als **Internet** direct onderhouden Verbinding met Internet. Standaard, dienen de Firewall van Azure biedt geen ondersteuning voor een geforceerde tunneling naar een on-premises netwerk.<br><br>Echter, als uw configuratie geforceerde tunneling naar een on-premises netwerk vereist, Microsoft wordt hiervoor ondersteuning bieden op basis van per geval. Neem contact op met ondersteuning voor zodat we uw aanvraag kunt controleren. Als geaccepteerd, we whitelist uw abonnement en zorg ervoor dat de internetverbinding vereist firewall wordt onderhouden.|

<sup>1</sup>als u deze limieten verhogen wilt, neem dan contact op met de ondersteuning van Azure.
