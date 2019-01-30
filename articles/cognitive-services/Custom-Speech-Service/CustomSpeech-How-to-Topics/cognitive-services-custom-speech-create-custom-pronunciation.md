---
title: Gebruik aangepaste uitspraak met Custom Speech Service op Azure | Microsoft Docs
description: Informatie over het maken van een taalmodel met de Custom Speech Service in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: panosper
ms.openlocfilehash: 0459fdb63134598e65657e5e26020c2ee0dc0efc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209788"
---
# <a name="enable-custom-pronunciation"></a>Aangepaste uitspraak inschakelen

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Aangepaste uitspraak kan gebruikers de fonetische formulier en de weergave van een woord of een term definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig is een uitspraak van bestand (een eenvoudige txt-bestand).

Hier ziet u hoe het werkt. U kunt verschillende aangepaste uitspraak van vermeldingen invoeren in een enkele txt-bestand. De structuur is als volgt:

```
Display form <Tab> Spoken form <Newline>
```

*Voorbeelden*:

| Formulier weergeven | Gesproken formulier |
|----------|-------|
| C3PO | Zie drie oor o |
| L8R | laat zijn |
| CNTK | Zie n thee k|

## <a name="requirements-for-the-spoken-form"></a>Vereisten voor het gesproken formulier
Het formulier gesproken moet een kleine letter, die kan worden afgedwongen tijdens het importeren. Bovendien moet u controles in de gegevensimport opgeven. Er is geen tabblad in de gesproken formulier of het formulier weergeven is toegestaan. Er kan echter, meer verboden tekens in het formulier weergeven (bijvoorbeeld, ~ en ^).

Elke txt-bestand kan verschillende vermeldingen hebben. Zie bijvoorbeeld de volgende schermafbeelding:

![Schermafbeelding van Kladblok met meerdere vermeldingen voor acroniem uitspraak van](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

Het formulier gesproken is de fonetische opeenvolging van het formulier weergeven. Deze bestaat uit letters, woorden of lettergrepen. Op dit moment is er geen verdere richtlijnen of reeks standaarden om u te helpen u bij het formuleren van de gesproken vorm. 

## <a name="supported-pronunciation-characters"></a>Ondersteunde uitspraak van tekens
Aangepaste uitspraak wordt momenteel ondersteund voor Engels (en-US) en Duits (nl-nl). De tekenset die kan worden gebruikt om de gesproken vorm van een term die (in het bestand aangepaste uitspraak) wordt weergegeven in de volgende tabel: 

| Taal | Tekens |
|---------- |----------|
| Engels (en-US) | a, b, c, d, e, f, g, h, i, "j", k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Duits (nl-nl) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, "j", k, l, o, p, q, r, s, t, u, v, w, x, y, z |

>[OPMERKING] Weergaveformulier van een term die (in een bestand uitspraak) moet dezelfde manier worden geschreven in een gegevensset voor aanpassing van taal.

## <a name="requirements-for-the-display-form"></a>Vereisten voor het formulier weergeven
Een weergaveformulier mag alleen een aangepast woord, term, acroniem of samengestelde woorden die een combinatie van bestaande woorden. U kunt ook alternatieve uitspraak voor veelvoorkomende woorden invoeren. 

>[!NOTE]
We raden niet met behulp van deze functie te reformulate veelvoorkomende woorden of te wijzigen van de gesproken vorm. Is het beter om uit te voeren van de decoder om te zien als een ongebruikelijke woorden (zoals afkortingen, technische woorden en woorden in vreemde talen) niet correct worden ontsleuteld. Als ze zijn, kunt u ze aan het aangepaste uitspraak van bestand toevoegen. In het taalmodel, altijd en alleen gebruikt u de vorm van de weergave van een woord. 

## <a name="requirements-for-the-file-size"></a>Vereisten voor de bestandsgrootte
De grootte van de txt-bestand met de vermeldingen voor de uitspraak is beperkt tot 1 MB. Normaal gesproken hoeft u niet voor het uploaden van grote hoeveelheden gegevens via dit bestand. De meeste aangepaste uitspraak van bestanden zijn waarschijnlijk slechts een paar kB groot. De codering van de txt-bestand voor alle landinstellingen moet UTF-8 stuklijst. Voor de Engelse landinstelling is ook ANSI acceptabel.

## <a name="next-steps"></a>Volgende stappen
* De nauwkeurigheid van de verbeteren door het maken van uw [aangepast akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md).
* [Maak een aangepaste spraak-naar-tekst-eindpunt](cognitive-services-custom-speech-create-endpoint.md), die u kunt gebruiken vanuit een app.
