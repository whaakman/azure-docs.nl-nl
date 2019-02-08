---
title: Migreren van de Translator Speech-API naar de Speech-Service
titleSuffix: Azure Cognitive Services
description: Informatie over het migreren van uw toepassingen uit de Translator Speech-API met de spraak-Service.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: de9727df9255fb880403e0409055b73db240e882
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868107"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migreren van de Translator Speech-API naar de Speech-Service

In dit artikel gebruiken voor het migreren van uw toepassingen uit de Microsoft Translator Speech-API aan de [Spraakservice](index.yml). Deze handleiding bevat een overzicht van de verschillen tussen de Translator Speech-API en de Service voor spraak en stelt strategieën voor het migreren van uw toepassingen.

> [!NOTE]
> Uw abonnementssleutel Translator Speech-API wordt niet geaccepteerd door de Speech-Service. U moet een nieuwe Speech Service-abonnement.

## <a name="comparison-of-features"></a>Vergelijking van functies

| Functie                                           | Translator Speech-API                                  | Speech Service | Details                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Converteren naar tekst                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Converteren naar spraak                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Globaal eindpunt                                   | :heavy_check_mark:                                              | : heavy_minus_sign:                 | De Speech-Service niet wordt momenteel een globaal eindpunt voor bieden. Een globaal eindpunt kunt automatisch instellen dat verkeer naar het dichtstbijzijnde regionale eindpunt, waardoor de latentie in uw toepassing.                                                    |
| Regionale eindpunten                                | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tijdslimiet voor verbinding                             | 90 minuten                                               | Onbeperkte met de SDK. 10 minuten met een WebSockets-verbinding.                                                                                                                                                                                                                                                                                   |
| Verificatiesleutel in kop                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Meerdere talen vertaald in een enkele aanvraag | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK's beschikbaar                                    | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Zie de [Spraakservice documentatie](index.yml) voor beschikbare SDK's.                                                                                                                                                    |
| WebSockets verbindingen                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Talen API                                     | :heavy_check_mark:                                              | : heavy_minus_sign:                 | De spraak-Service ondersteunt de dezelfde reeks talen die worden beschreven in de [naslaginformatie over de talen van Translator-API voor](../translator-speech/languages-reference.md) artikel. |
| Filter voor scheldwoorden en markering                       | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM als invoer                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Andere bestandstypen als invoer                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Gedeeltelijke resultaten                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Timing info                                       | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Correlatie-id                                    | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Aangepaste spraakmodellen                              | : heavy_minus_sign:                                              | :heavy_check_mark:                 | De spraak-Service biedt aangepaste spraakmodellen waarmee u kunt aanpassen spraakherkennings op het vocabulaire van uw organisatie.                                                                                                                                           |
| Aangepaste omzettingsmodellen                         | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Kunt u zich abonneert op de Spraakomzettings-API van Microsoft tekst [aangepaste Translator](https://www.microsoft.com/translator/business/customization/) met uw eigen gegevens voor nauwkeurigere vertalingen.                                                 |

## <a name="migration-strategies"></a>Migratiestrategieën

Als u of uw organisatie hebt toepassingen in de ontwikkeling of productie die gebruikmaken van de Translator Speech-API, moet u ze voor het gebruik van de Spraakservice bijwerken. Zie de [Spraakservice](index.yml) documentatie voor beschikbare SDK's, codevoorbeelden en zelfstudies. Overweeg het volgende wanneer u migreert:

* De Speech-Service niet wordt momenteel een globaal eindpunt voor bieden. Als uw toepassing efficiënt functioneert als één regionale eindpunt wordt gebruikt voor al het verkeer bepalen Als dat niet het geval is, geolocatie gebruiken om te bepalen van het eindpunt van de meest efficiënte.

* Als uw toepassing maakt gebruik van lange levensduur hebben verbindingen en de beschikbare SDK's kan niet worden gebruikt, kunt u een verbinding WebSockets. De 10 minuten time-outlimiet beheren door op de juiste tijden verbinding.

* Als uw toepassing gebruikmaakt van de Translator Text-API en de Translator Speech-API om in te schakelen omzettingsmodellen van aangepaste, kunt u categorie-id's toevoegen via de Speech-Service.

* In tegenstelling tot de Translator Speech-API, kan de Spraakservice vertalingen in meerdere talen in één aanvraag voltooien.

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md)
* [Snelstart: Herkent de gesproken tekst in een UWP-app met behulp van de spraak-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Zie ook

* [Wat is de Speech-Service](overview.md)
* [Spraakservice en de SDK-documentatie](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
