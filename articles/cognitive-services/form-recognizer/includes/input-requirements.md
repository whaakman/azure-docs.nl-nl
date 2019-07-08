---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 816fa4b0df2fab77bcde261f30d8bad584257fd7
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592618"
---
Formulier herkenning werkt op invoer van de documenten die aan deze vereisten voldoen:

* Indeling moet JPG, PNG- of een PDF-bestand (tekst of gescand). Tekst ingesloten PDF-bestanden zijn aanbevolen, omdat er geen mogelijkheid om de fout in de extractie van tekens en de locatie.
* Bestandsgrootte moet minder dan 4 megabyte (MB).
* Voor installatiekopieën van moet dimensies tussen 50 x 50 pixels en 4200 x 4200 pixels.
* Als gescand van papierdocumenten, moeten de formulieren scans van hoge kwaliteit zijn.
* Tekst moet het Latijnse alfabet (Engelse tekens) gebruiken.
* Gegevens moeten worden afgedrukt (niet geschreven).
* Gegevens moeten sleutels en waarden bevatten.
* Sleutels kunnen worden weergegeven boven of links van de waarden, maar niet minder dan of aan de rechterkant.

Formulier-herkenning ondersteunt momenteel geen dergelijke van ingevoerde gegevens:

* Complexe tabellen (geneste tabellen, samengevoegde koptekst of cellen, enzovoort).
* Selectievakjes of radio knoppen.
* PDF-documenten langer is dan 50 pagina's.