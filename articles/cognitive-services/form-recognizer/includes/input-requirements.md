---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 17dc32f8948387b90229d3c4c07102cff98e3018
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562702"
---
Formulier herkenning werkt op invoer documenten die aan de volgende vereisten voldoen:

* Indeling moet JPG, PNG of PDF zijn (tekst of gescand). Tekst-Inge sloten Pdf's zijn het beste omdat er geen mogelijke fout is in teken extractie en locatie.
* Als uw Pdf's met een wacht woord zijn vergrendeld, moet u de vergren deling verwijderen voordat u ze verzendt.
* De bestands grootte moet kleiner zijn dan 4 MB.
* Voor afbeeldingen moeten de afmetingen tussen 50 x 50 pixels en 4200 x 4200 pixels liggen.
* Als u scant vanuit papieren documenten, moeten formulieren kwalitatief hoogwaardige scans zijn.
* Tekst moet het Latijnse alfabet (Engelse tekens) gebruiken.
* De gegevens moeten sleutels en waarden bevatten.
* Sleutels kunnen boven of links van de waarden worden weer gegeven, maar niet onder of rechts.

Dit type invoer gegevens wordt momenteel niet door de formulier herkenning ondersteund:

* Complexe tabellen (geneste tabellen, samengevoegde kopteksten of cellen, enzovoort).
* Selectie vakjes of keuze rondjes.
* PDF-documenten die langer zijn dan 50 pagina's.