---
title: Interne werking van Azure Service Fabric betrouwbaar status Manager en betrouwbare verzameling | Microsoft Docs
description: Deep dive op betrouwbare verzameling concepten en ontwerp in Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Interne werking van Azure Service Fabric betrouwbaar status Manager en betrouwbare verzameling
Dit document delves in een betrouwbare statusbeheer en betrouwbare verzamelingen om te zien hoe de basisonderdelen die achter de schermen werken.

> [!NOTE]
> Dit document is werk wordt uitgevoerd. Opmerkingen toevoegen aan dit artikel voor Vertel ons wat u meer informatie wilt over onderwerp.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Lokale persistentie model: logboek- en controlepunt
De betrouwbare statusbeheer en betrouwbare verzamelingen Volg een model voor persistentie logboek- en controlepunt wordt aangeroepen.
In dit model wordt elke wijziging in de status op de schijf voor het eerst geregistreerd en vervolgens toegepast in het geheugen.
De status van de volledige zelf is alleen sporadisch persistent (ook Controlepunt).
Het voordeel is dat de delta's zijn ingeschakeld in opeenvolgende alleen toevoegen schrijfbewerkingen op de schijf voor betere prestaties.

Om het logboek en controlepunt model beter te begrijpen we eerst kijken naar het scenario voor oneindige schijven.
Statusbeheer voor het betrouwbare registreert elke bewerking voordat deze worden gerepliceerd.
Logboekregistratie kan de betrouwbare verzamelingen om toe te passen van de bewerking alleen in het geheugen.
Omdat logboeken zijn persistent hebt gemaakt, zelfs wanneer de replica is mislukt en moet opnieuw worden opgestart, heeft de betrouwbare status Manager voldoende informatie in het logboek voor de replay van alle bewerkingen van die de replica is verbroken.
Als de schijf oneindig is, logboekrecords nooit moeten worden verwijderd en de betrouwbare verzameling moet de status van de in het geheugen te beheren.

Nu bekijken we het scenario voor eindig schijven.
Zoals logboekrecords verzamelen, voert de betrouwbare statusbeheer onvoldoende schijfruimte.
Voordat dit gebeurt, moet betrouwbare statusbeheer voor het afkappen van het logboek ruimte te maken voor de nieuwere records.
Statusbeheer voor betrouwbare aanvragen de betrouwbare verzamelingen controlepunt hun status in het geheugen naar schijf.
Op dit moment zou de betrouwbare verzamelingen blijven behouden de status in het geheugen.
Zodra de betrouwbare verzamelingen hun controlepunten hebt voltooid, kan de betrouwbare status Manager het logboek aan schijfruimte vrij afkappen.
Als de replica moet opnieuw worden opgestart, betrouwbare verzamelingen hun controlepunt herstellen en statusbeheer voor het betrouwbare herstelt en alle wijzigingen die sinds het laatste controlepunt afspeelt.

Een andere waarde toevoegen van controlepunten is dat het verbetert de hersteltijden in algemene scenario's. Het logboek bevat alle bewerkingen die zich hebben voorgedaan sinds het laatste controlepunt.
Deze kan dus meerdere versies van een item, zoals meerdere waarden voor een bepaalde rij in betrouwbare woordenlijst bevatten.
Een betrouwbare verzameling controlepunten daarentegen alleen de meest recente versie van elke waarde voor een sleutel.

## <a name="next-steps"></a>Volgende stappen
* [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

