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
ms.openlocfilehash: 4f26f3a31dc6303e991c39fc7f85d9bf254d57bc
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885134"
---
# <a name="point-in-time-snapshot"></a>Point-in-time-snapshot

Azure-appconfiguratie houdt records bij van de tijden waarop een nieuw sleutel-waardepaar is gemaakt en vervolgens wordt gewijzigd. Deze records vormen een volledige tijdlijn in sleutel-waardewijzigingen. Een app-configuratiearchief kan op elk gewenst moment de geschiedenis van een sleutel-waarde reconstrueren en de laatste waarde herhalen, tot aan de huidige. Met deze functie kunt u "tijdreizen" en een oude sleutel-waarde ophalen. U kunt bijvoorbeeld de configuratie-instellingen voor gisteren ophalen, net vóór de meest recente implementatie, zodat u een vorige configuratie kunt herstellen in het geval dat u de toepassing wilt terugdraaien.

## <a name="key-value-retrieval"></a>Sleutel/waarde ophalen

Om vorige sleutel-waarden terug te krijgen, moet u een tijdstip waarop een momentopname van sleutel-waarden is gemaakt opgeven in de HTTP-header van een REST-API-aanroep. Bijvoorbeeld:

        GET /revisions HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

De app-configuratie behoudt momenteel 7 dagen van de wijzigingsgeschiedenis.

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een ASP.NET-web-app maken](quickstart-aspnet-core-app.md)  
