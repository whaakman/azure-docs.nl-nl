---
title: Grof taalgebruik filteren - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Grof taalgebruik filteren in de Translator Text-API gebruiken.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 87814571e6f1c20b219020651eb798fa49a28deb
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127925"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Grof taalgebruik filteren met de Translator Text-API toevoegen

Normaal gesproken de Translator-service wordt bewaard, grof taalgebruik dat aanwezig is in de bron in de vertaling. De mate van grof taalgebruik en de context waarmee woorden grof verschillen tussen culturen. De mate van grof taalgebruik in de doeltaal mogelijk als gevolg hiervan worden versterkt of worden verlaagd.

Als u wilt om te voorkomen dat grof taalgebruik in de vertaling (zelfs als grof taalgebruik is aanwezig in de brontekst), gebruikt u de grof taalgebruik optie in de methode Translate() filteren. Deze optie kunt u kiezen of u wilt zien grof taalgebruik verwijderd of gemarkeerd met de juiste tags of geen actie ondernomen.

De methode Translate() heeft een parameter 'opties', waarin het nieuwe element 'ProfanityAction'. De geaccepteerde waarden van ProfanityAction zijn "NoAction", "Gemarkeerd" en "Verwijderd."

## <a name="accepted-values-of-profanityaction-and-examples"></a>Geaccepteerde waarden van ProfanityAction en voorbeelden
|ProfanityAction waarde | Bewerking | Voorbeeld: Bron - Japans | Voorbeeld: Target - Engels|
| :---|:---|:---|:---|
| NoAction | Standaard. Hetzelfde als de instelling van de optie. Grof taalgebruik wordt doorgegeven vanuit de bron naar doel. | 彼は変態です。 | Hij is aardig. |
| Gemarkeerd | Grof woorden worden omringd door de XML-tags \<grof taalgebruik >... \</profanity >. | 彼は変態です。 | Hij is een \<grof taalgebruik > jerk\</profanity >. |
| Deleted | Grof woorden worden verwijderd uit de uitvoer zonder vervanging. | 彼は。 | Hij is een. |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Grof taalgebruik filteren met de Translator-API-aanroep van toepassing](reference/v3-0-translate.md)

