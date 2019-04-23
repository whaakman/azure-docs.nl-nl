---
title: Point-in-time-snapshot voor Azure-appconfiguratie | Microsoft Docs
description: Een overzicht van hoe point-in-time-snapshot werkt in Azure-appconfiguratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 83770e8c5f415670855b5cf2502d02c4d6919440
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998070"
---
# <a name="point-in-time-snapshot"></a>Point-in-time-snapshot

Azure App-configuratie houdt records van de tijden wanneer een nieuwe sleutel / waarde-paar is gemaakt en vervolgens gewijzigd. Deze records vormen een volledige tijdlijn in sleutel-waardewijzigingen. Een app-configuratiearchief kunt reconstrueren de geschiedenis van een sleutelwaarde en de laatste waarde op elk gewenst moment, tot aan de huidige herhalen. Met deze functie ook 'time-traject' terug en ophalen van de waarde van een oude sleutel. Bijvoorbeeld, krijgt u van gisteren configuratie-instellingen, net vóór de implementatie van de meest recente om te kunnen herstellen van een vorige configuratie en het terugdraaien van de toepassing.

## <a name="key-value-retrieval"></a>Sleutel/waarde ophalen

Om op te halen voorbij sleutelwaarden, Geef een tijd waarop de sleutelwaarden zijn momentopname in de HTTP-header van een REST-API-aanroep. Bijvoorbeeld:

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

App-configuratie wordt op dit moment zeven dagen van de wijzigingsgeschiedenis van.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een ASP.NET Core web-app maken](./quickstart-aspnet-core-app.md)  
