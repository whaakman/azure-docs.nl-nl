---
title: Gebruik aangepaste uitspraak met aangepaste spraak-Service op Azure | Microsoft Docs
description: Informatie over het maken van een taalmodel met de aangepaste spraak-Service in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: panosper
ms.openlocfilehash: a74b69b84cc80809a25f18b580a18abb5721b8b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344508"
---
# <a name="enable-custom-pronunciation"></a>Aangepaste uitspraak inschakelen
Aangepaste uitspraak kan gebruikers de fonetische formulier en de weergave van een woord of term definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of acroniemen. Alles wat u nodig hebt is een uitspraak-bestand (een eenvoudige .txt-bestand).

Hier ziet u hoe het werkt. U kunt meerdere aangepaste uitspraak posten invoeren in een enkel txt-bestand. De structuur is als volgt:

```
Display form <Tab> Spoken form <Newline>
```

*Voorbeelden*:

| Formulier weergeven | Gesproken formulier |
|----------|-------|
| C3PO | Zie drie pea o |
| L8R | laat zijn |
| CNTK | Zie n thee k|

## <a name="requirements-for-the-spoken-form"></a>Vereisten voor de gesproken vorm
Het formulier gesproken moet een kleine letter, die kan worden afgedwongen tijdens het importeren. Bovendien moet u controles in de gegevensimport opgeven. Er is geen tabblad in de vorm gesproken of het weergaveformulier toegestaan. Er kan echter, meer verboden tekens in het formulier weergeven (bijvoorbeeld ~ en ^).

Elke .txt-bestand kan verschillende vermeldingen hebben. Bijvoorbeeld, Zie de volgende schermafbeelding:

![Schermopname van Kladblok met meerdere vermeldingen voor acroniem uitspraak](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

Het gesproken formulier is de fonetische volgorde van het formulier weergeven. Het letters, woorden of lettergrepen bestaat. Er is momenteel geen verdere richtlijnen of verzameling standaarden kunt u het formulier gesproken formuleren. 

## <a name="supported-pronunciation-characters"></a>Ondersteunde uitspraak tekens
Aangepaste uitspraak wordt momenteel ondersteund voor Engels (en-US) en Duits (nl-nl). De tekenset die kan worden gebruikt om de gesproken vorm van een term (in het bestand aangepaste uitspraak) express wordt weergegeven in de volgende tabel: 

| Taal | Tekens |
|---------- |----------|
| Engels (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Duits (nl-nl) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

>[OPMERKING] Formulier van een term weergeven (in een bestand uitspraak) moet dezelfde manier worden geschreven in een taal aanpassing-gegevensset.

## <a name="requirements-for-the-display-form"></a>Vereisten voor het formulier weergeven
Een formulier weergeven mag alleen een aangepaste woord, term, acroniem of samengestelde woorden die de bestaande woorden combineren. U kunt ook alternatieve uitspraak voor veelvoorkomende woorden invoeren. 

>[!NOTE]
We raden niet reformulate veelvoorkomende woorden of wijzigen van het formulier gesproken met deze functie. Is het beter om uit te voeren van de decoder om te zien als sommige ongebruikelijke woorden (zoals afkortingen technische woorden en vreemde woorden) niet correct worden gedecodeerd. Als ze zijn, kunt u deze naar het bestand aangepaste uitspraak toevoegen. In het Model taal alleen en altijd gebruikt u het formulier weergeven van een woord. 

## <a name="requirements-for-the-file-size"></a>Vereisten voor de bestandsgrootte
De grootte van het txt-bestand met de vermeldingen voor uitspraak is beperkt tot 1 MB. Normaal gesproken hoeft u niet voor het uploaden van grote hoeveelheden gegevens via dit bestand. De meeste aangepaste uitspraakbestanden zijn waarschijnlijk slechts enkele kB groot. De codering van het txt-bestand voor alle landinstellingen moet UTF-8 stuklijst. Voor de Engelstalige landinstelling is ook ANSI acceptabel.

## <a name="next-steps"></a>Volgende stappen
* De nauwkeurigheid van de verbeteren door het maken van uw [aangepaste akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md).
* [Maak een aangepaste spraak naar tekst eindpunt](cognitive-services-custom-speech-create-endpoint.md), dat u uit een app kunt gebruiken.
