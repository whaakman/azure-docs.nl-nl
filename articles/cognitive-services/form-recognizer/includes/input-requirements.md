---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 690219347782aad2140b0a1b0541590c5426c568
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459734"
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