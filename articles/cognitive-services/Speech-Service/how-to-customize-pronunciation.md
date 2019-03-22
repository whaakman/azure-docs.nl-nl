---
title: Uitspraak van - Speech Services aanpassen
titlesuffix: Azure Cognitive Services
description: Informatie over het aanpassen van de uitspraak met de Speech-Service. U kunt de fonetische vorm en de weergave van een woord of een term definiëren met aangepaste uitspraak. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig hebt om te beginnen is een uitspraak van bestand--een eenvoudige txt-bestand.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 139c5d47fe6ea82148e2d5e1cf2f5fcb72d4020e
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339394"
---
# <a name="enable-custom-pronunciation"></a>Aangepaste uitspraak inschakelen

U kunt met behulp van aangepaste uitspraak van de fonetische vorm en de weergave van een woord of een term (acroniem) definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig hebt om te beginnen is een uitspraak van bestand--een eenvoudige txt-bestand.

Hier ziet u hoe het werkt. U kunt verschillende aangepaste uitspraak van vermeldingen invoeren in een enkele txt-bestand. De structuur is als volgt:

```
Display form <Tab> Spoken form <Newline>
```

Enkele voorbeelden worden weergegeven in de volgende tabel:

| Formulier weergeven | Gesproken formulier |
|----------|-------|
| 3CPO | Zie drie oor o |
| L8R | laat zijn |
| CNTK | c n t k|

## <a name="requirements-for-the-spoken-form"></a>Vereisten voor het gesproken formulier

Het formulier gesproken moet een kleine letter, die u kunt afdwingen dat tijdens het importeren. U moet ook controles in de gegevensimport opgeven. Er is geen tabblad in de gesproken formulier of het formulier weergeven is toegestaan. Echter, er mogelijk meer verboden tekens in het formulier weergeven (bijvoorbeeld, ~ en ^).

Elke txt-bestand kan verschillende vermeldingen, hebben, zoals wordt weergegeven in de volgende afbeelding:

![Voorbeelden van acroniem uitspraak van](media/stt/custom-speech-pronunciation-file.png)

Het formulier gesproken is de fonetische opeenvolging van het formulier weergeven. Deze bestaat uit letters, woorden of lettergrepen. Op dit moment is er geen verdere richtlijnen of reeks standaarden om u te helpen u bij het formuleren van de gesproken vorm.

## <a name="supported-pronunciation-characters"></a>Ondersteunde uitspraak van tekens

Aangepaste uitspraak wordt momenteel ondersteund voor Engels (en-US) en Duits (nl-nl). De tekensets die u gebruiken kunt om de gesproken vorm van een term die (in het bestand aangepaste uitspraak) worden weergegeven in de volgende tabel:

| Taal | Tekens |
|---------- |----------|
| Engels (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Duits (nl-nl) | ä, ö, ü,?, a, b, c, d, e, f, g, h, i, "j", k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> Weergaveformulier van een term die (in een bestand uitspraak) moet dezelfde manier worden geschreven in een taal aanpassing van woordenlijsten gegevensset.

## <a name="requirements-for-the-display-form"></a>Vereisten voor het formulier weergeven

Een weergaveformulier mag alleen een aangepast woord, een acroniem of samengestelde woorden die een combinatie van bestaande woorden.

>[!NOTE]
>U kunt beter geen deze functie te reformulate veelvoorkomende woorden of te wijzigen van de gesproken vorm. Is het beter om uit te voeren van de decoder om te zien of sommige ongebruikelijke woorden (zoals afkortingen, technische woorden of woorden in vreemde talen) onjuist worden gedecodeerd. Als ze zijn, kunt u ze aan het aangepaste uitspraak van bestand toevoegen. In het taalmodel, altijd en alleen gebruikt u de vorm van de weergave van een woord.

## <a name="requirements-for-the-file-size"></a>Vereisten voor de bestandsgrootte
De grootte van de txt-bestand dat de uitspraak van vermeldingen bevat die is beperkt tot 1 MB (1KB voor de gratis laag sleutels). U hoeft normaal gesproken te uploaden van grote hoeveelheden gegevens via dit bestand. De meeste aangepaste uitspraak van bestanden zijn waarschijnlijk slechts een aantal kilobytes (kB) groot. De codering van de txt-bestand voor alle landinstellingen moet UTF-8 stuklijst. Voor de Engelse landinstelling is ook ANSI acceptabel.

## <a name="next-steps"></a>Volgende stappen
* De nauwkeurigheid van de verbeteren door het maken van een [aangepast akoestisch model](how-to-customize-acoustic-models.md).
* De nauwkeurigheid van de verbeteren door het maken van een [aangepast taalmodel](how-to-customize-language-model.md).
