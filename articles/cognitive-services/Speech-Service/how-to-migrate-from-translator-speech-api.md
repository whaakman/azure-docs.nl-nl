---
title: Migreren van de Translator Speech-API naar de Speech-Service
titleSuffix: Azure Cognitive Services
description: Gebruik dit onderwerp voor het migreren van uw toepassingen uit de Translator Speech-API met de spraak-Service.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: b50cdc6978519c0ec9da447c324237c00577d9fd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885269"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migreren van de Translator Speech-API naar de Speech-Service

In dit artikel gebruiken voor het migreren van uw toepassingen uit de Microsoft Translator Speech-API aan de [Spraakservice](index.yml). Deze handleiding bevat een overzicht van de verschillen tussen de Translator Speech-API en de Service voor spraak en stelt strategieën voor het migreren van uw toepassingen.

> [!NOTE]
> Uw abonnementssleutel Translator Speech-API wordt niet geaccepteerd door de Speech-Service. U moet een nieuwe Speech Service-abonnement.

## <a name="comparison-of-features"></a>Vergelijking van functies

| Functie                                           | Translator Speech-API                                  | Speech Service | Details                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Converteren naar tekst                               | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Converteren naar spraak                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Globaal eindpunt                                   | : heavy_check_mark:                                              | : heavy_minus_sign:                 | De Speech-Service biedt momenteel geen een globaal eindpunt. Een globaal eindpunt kunt automatisch instellen dat verkeer naar het dichtstbijzijnde regionale eindpunt, waardoor de latentie in uw toepassing.                                                    |
| Regionale eindpunten                                | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tijdslimiet voor verbinding                             | 90 minuten                                               | Onbeperkte met de SDK. 10 minuten met een websocket-verbinding                                                                                                                                                                                                                                                                                   |
| Verificatiesleutel in kop                                | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Meerdere talen vertaald in een enkele aanvraag | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK's beschikbaar                                    | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Zie de [Spraakservice documentatie](index.yml) voor beschikbare SDK's.                                                                                                                                                    |
| Websocket-verbindingen                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Talen API                                     | : heavy_check_mark:                                              | : heavy_minus_sign:                 | De spraak-Service ondersteunt de dezelfde reeks talen die worden beschreven in de [naslaginformatie over de talen van Translator-API voor](../translator-speech/languages-reference.md) artikel. |
| Filter voor scheldwoorden en markering                       | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM als invoer                                 | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Andere bestandstypen als invoer                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Gedeeltelijke resultaten                                   | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Timing Info                                       | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Correlatie-id                                    | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Aangepaste Spraakmodellen                              | : heavy_minus_sign:                                              | : heavy_check_mark:                 | De spraak-Service biedt aangepaste spraakmodellen waarmee u kunt aanpassen spraakherkennings op het vocabulaire van uw organisatie.                                                                                                                                           |
| Aangepaste Omzettingsmodellen                         | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Kunt u zich abonneert op de Spraakomzettings-API van Microsoft tekst [aangepaste Translator](https://www.microsoft.com/translator/business/customization/) (momenteel in preview) met uw eigen gegevens voor nauwkeurigere vertalingen.                                                 |

## <a name="migration-strategies"></a>Migratiestrategieën

Als u of uw organisatie hebt toepassingen in de ontwikkeling of productie die gebruikmaken van de Translator Speech-API, moet u ze voor het gebruik van de Spraakservice bijwerken. Zie de [Spraakservice](index.yml) documentatie voor beschikbare SDK's, codevoorbeelden en zelfstudies. Hieronder volgen enkele overwegingen bij het migreren van:

* De Speech-Service biedt momenteel geen een globaal eindpunt. U moet bepalen als uw toepassing werkt efficiënt gebruik van één regionale eindpunt voor al het verkeer. Als deze wordt niet, moet u geolocatie gebruiken om te bepalen van het eindpunt van de meest efficiënte.

* Als uw toepassing maakt gebruik van lange levensduur hebben verbindingen en de beschikbare SDK's kan niet worden gebruikt, kunt u een websocket-verbinding gebruiken en beheren van de 10 minuten time-outlimiet door opnieuw verbinding maken op de juiste tijden.

* Als uw toepassing gebruikmaakt van de Translator Text-API en de Translator Speech-API om in te schakelen omzettingsmodellen van aangepaste, wordt u mogelijk zijn om toe te voegen 'Categorie'-id's die rechtstreeks met de Speech-Service.

* Vertalingen in meerdere talen in één aanvraag, in tegenstelling tot de Translator Speech-API kunt voltooid met de spraak-Service.

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md)
* [Snelstartgids: Herkennen gesproken tekst in een UWP-app met behulp van de spraak-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Zie ook

* [Wat is de Speech-service](overview.md)
* [Spraakservice en de SDK-documentatie](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
