---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 065cb4daa9501ee658d364dad43b9e03798e4083
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269694"
---
De taak levert een JSON-uitvoer-bestand met metagegevens over gezichten gedetecteerd en bijgehouden. De metagegevens bevatten die wijzen op de locatie van gezichten, evenals een face id-nummer die wijzen op het volgen van deze persoon coördinaten. Face id-nummers zijn gevoelig zijn voor het opnieuw instellen onder omstandigheden wanneer de voorzijde gezicht is zoekgeraakt of overlappende in het kader, wat resulteert in bepaalde personen meerdere id's ophalen van toegewezen.

De JSON-uitvoer bevat de volgende elementen:

### <a name="root-json-elements"></a>Hoofdmap JSON-elementen

| Element | Beschrijving |
| --- | --- |
| versie |Dit verwijst naar de versie van de Video-API. |
| tijdschaal |'Tikken"per seconde van de video. |
| offset |Dit is de time-offset voor tijdstempels. Versie 1.0 van Video-API's, wordt dit altijd 0 zijn. In de toekomst scenario's die wordt ondersteund, deze waarde kan worden gewijzigd. |
| breedte, met veel |De breedte en met veel van de video frame van uitvoer in pixels.|
| framesnelheid |Aantal frames per seconde video. |
| [fragmenten](#fragments-json-elements) |De metagegevens wordt gesegmenteerde overdrachtscodering omhoog in verschillende segmenten fragmenten genoemd. Elk fragment bevat een start, een duur, een intervalnummer en een of meer gebeurtenissen. |

### <a name="fragments-json-elements"></a>Fragmenten JSON-elementen

|Element|Beschrijving|
|---|---|
| start |De begintijd van de eerste gebeurtenis in "tikken." |
| duur |De lengte van het fragment, in "tikken." |
| index | (Alleen van toepassing op Azure Media Redactor) definieert de index van het kader van de huidige gebeurtenis. |
| interval |Het interval van elke vermelding in het fragment, in "tikken." |
| events |Elke gebeurtenis bevat de gezichten gedetecteerd en bijgehouden binnen deze tijdsduur. Er is een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De binnenste matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden gedurende de interval. Een lege haakje [] betekent dat er geen gezichten gedetecteerd. |
| id |De ID van het oppervlak dat wordt bijgehouden. Dit nummer kan per ongeluk worden gewijzigd als een gezicht niet gevonden wordt. Een bepaalde persoon moet dezelfde ID in de algehele video hebben, maar dit kan niet worden gegarandeerd vanwege beperkingen in de detectie-algoritme (bedekking, enzovoort). |
| x, y |De linkerbovenhoek X en Y-coördinaten van de face-vak in een genormaliseerde schaal van 0,0 en 1,0. <br/>-X- en Y coördinaten zijn ten opzichte van Liggend altijd, dus als u een staande video (of omlaag, in het geval van iOS) hebt, hebt u om te zetten de coördinaten dienovereenkomstig. |
| breedte, hoogte |De breedte en hoogte van het selectiekader in een genormaliseerde schaal van 0,0 en 1,0 gezicht. |
| facesDetected |Dit is gevonden aan het einde van de JSON-resultaten en bevat een overzicht van het aantal gezichten die het algoritme gedetecteerd tijdens de video. Omdat de id's kunnen opnieuw worden ingesteld per ongeluk als een gezicht niet gevonden wordt (bijvoorbeeld het gezicht scherm ziet er direct), gaat dit aantal kan niet altijd gelijk zijn aan het echte nummer van gezichten wordt uitgevoerd in de video. |
