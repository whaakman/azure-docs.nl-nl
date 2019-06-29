---
title: Grof taalgebruik filteren - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Grof taalgebruik filteren in de Translator Text-API gebruiken.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cc6bf9766912aa55d7869d90976d3089cfd6cf4c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448263"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Grof taalgebruik filteren met de Translator Text-API toevoegen

Normaal gesproken de Translator-service wordt bewaard, grof taalgebruik dat aanwezig is in de bron in de vertaling. De mate van grof taalgebruik en de context waarmee woorden grof verschillen tussen culturen. De mate van grof taalgebruik in de doeltaal mogelijk als gevolg hiervan worden versterkt of worden verlaagd.

Als u wilt om te voorkomen dat grof taalgebruik in de vertaling, zelfs als grof taalgebruik in de brontekst aanwezig is, gebruikt u de optie die beschikbaar is in de methode Translate() filteren scheldwoorden. Deze optie kunt u kiezen of u zien grof taalgebruik verwijderd wilt, gemarkeerd met de juiste tags, of geen actie ondernomen.

De methode Translate() neemt de parameter 'opties', waarin het nieuwe element 'ProfanityAction'. De geaccepteerde waarden van ProfanityAction zijn "NoAction", "Gemarkeerd" en "Verwijderd."

## <a name="accepted-values-of-profanityaction-and-examples"></a>Geaccepteerde waarden van ProfanityAction en voorbeelden
|ProfanityAction waarde | Bewerking | Voorbeeld: Bron - Japans | Voorbeeld: Doel - Engels|
| :---|:---|:---|:---|
| NoAction | Standaard. Hetzelfde als de instelling van de optie. Grof taalgebruik wordt doorgegeven vanuit de bron naar doel. | 彼は変態です。 | Hij is aardig. |
| Gemarkeerd | Grof woorden worden omringd door de XML-tags \<grof taalgebruik >... \</profanity >. | 彼は変態です。 | Hij is een \<grof taalgebruik > jerk\</profanity >. |
| Verwijderen | Grof woorden worden verwijderd uit de uitvoer zonder vervanging. | 彼は。 | Hij is een. |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Grof taalgebruik filteren met de Translator-API-aanroep van toepassing](reference/v3-0-translate.md)
