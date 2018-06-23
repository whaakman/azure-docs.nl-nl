---
title: Filteren met de API van Microsoft Translator tekst taalgebruik | Microsoft Docs
description: Gebruik taalgebruik filteren in de API van Microsoft Translator tekst.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a7172e1e8aa336c011fb06e93fc5c4b54d26a3cd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344810"
---
# <a name="how-to-add-profanity-filtering-with-the-microsoft-translator-text-api"></a>Het taalgebruik filteren met de API van Microsoft Translator tekst toevoegen

De service conversieprogramma behoudt normaal taalgebruik die aanwezig is in de bron in de vertaling. De mate van taalgebruik en de context waarin woorden schennende verschillen tussen culturen. De mate van taalgebruik in de taal van het doel kan als gevolg hiervan worden versterkt of verminderd.

Als u wilt voorkomen dat dergelijke taalgebruik in de vertaling (onafhankelijk van de aanwezigheid van taalgebruik in de brontekst), kunt u de filteroptie taalgebruik in de methode Translate(). De optie kunt u kiezen of u wilt zien taalgebruik verwijderd of is gemarkeerd met de juiste labels of geen actie ondernomen.

De methode Translate() heeft een parameter 'options', waarin het nieuwe element 'ProfanityAction'. De toegestane waarden van ProfanityAction zijn 'NoAction', 'Gemarkeerd' en 'Deleted'.

## <a name="accepted-values-of-profanityaction-and-examples"></a>Geaccepteerde waarden van ProfanityAction en voorbeelden
|ProfanityAction waarde | Bewerking | Voorbeeld: Bron - Japans | Voorbeeld: Target - Engels|
| :---|:---|:---|:---|
| NoAction | Standaard. Hetzelfde als de optie niet instellen. Taalgebruik geeft van bron naar doel. | 彼は変態です。 | Hij is een jerk. |
| Gemarkeerd | Ongepaste woorden worden omgeven door een XML-labels \<taalgebruik >... \</profanity >. | 彼は変態です。 | Hij een \<taalgebruik > jerk\</profanity >. |
| Deleted | Ongepaste woorden zijn verwijderd uit de uitvoer zonder vervanging. | 彼は。 | Hij een. |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Taalgebruik met het conversieprogramma API-aanroep voor het filteren van toepassing](reference/v3-0-translate.md)

