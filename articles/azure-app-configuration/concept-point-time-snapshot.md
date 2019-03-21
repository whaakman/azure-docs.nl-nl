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
ms.openlocfilehash: e833146d05f0c35449915c1d1293873258a7b7eb
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226773"
---
# <a name="point-in-time-snapshot"></a>Point-in-time-snapshot

Azure App-configuratie houdt records van de tijden wanneer een nieuwe sleutel / waarde-paar is gemaakt en vervolgens gewijzigd. Deze records vormen een volledige tijdlijn in sleutel-waardewijzigingen. Een app-configuratiearchief kunt reconstrueren de geschiedenis van een sleutelwaarde en de laatste waarde op elk gewenst moment, tot aan de huidige herhalen. Met deze functie ook 'time-traject' terug en ophalen van de waarde van een oude sleutel. Bijvoorbeeld, krijgt u van gisteren configuratie-instellingen, net vóór de implementatie van de meest recente om te kunnen herstellen van een vorige configuratie en het terugdraaien van de toepassing.

## <a name="key-value-retrieval"></a>Sleutel/waarde ophalen

Om op te halen voorbij sleutelwaarden, Geef een tijd waarop de sleutelwaarden zijn momentopname in de HTTP-header van een REST-API-aanroep. Bijvoorbeeld:

        GET /revisions HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

App-configuratie wordt op dit moment zeven dagen van de wijzigingsgeschiedenis van.

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: een ASP.NET-web-app maken](quickstart-aspnet-core-app.md)  
