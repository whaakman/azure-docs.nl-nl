---
title: Filteren op scheld woorden-Translator Text-API
titleSuffix: Azure Cognitive Services
description: Gebruik filters voor scheld woorden in de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 114dea098db5c824a1235ba1635f547383bf6743
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595188"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Filters voor scheld woorden toevoegen met de Translator Text-API

Normaal gesp roken behoudt de Translator-service woorden die aanwezig zijn in de bron in de vertaling. De mate van scheld woorden en de context die het ongepaste woord is, verschillen per cultuur. Als gevolg hiervan kan de mate van scheld woorden in de doel taal worden versterkt of gereduceerd.

Als u wilt voor komen dat er grove taal in de vertaling wordt weer gegeven, zelfs als scheld woorden in de bron tekst aanwezig zijn, gebruikt u de filter optie voor scheld woorden die beschikbaar is in de methode trans late (). Met deze optie kunt u kiezen of u ongepaste woorden wilt verwijderen, gemarkeerd met de juiste Tags of geen actie ondernomen.

De methode relate () gebruikt de para meter ' Options ', die het nieuwe element ' ProfanityAction ' bevat. De geaccepteerde waarden van ProfanityAction zijn ' geen actie ', ' gemarkeerd ' en ' verwijderd '.

## <a name="accepted-values-of-profanityaction-and-examples"></a>Geaccepteerde waarden van ProfanityAction en voor beelden
|Waarde ProfanityAction | Action | Voorbeeld: Bron-Japans | Voorbeeld: Doel-Engels|
| :---|:---|:---|:---|
| NoAction | Standaard. Hetzelfde als de optie niet instellen. Gods gang van de bron naar het doel. | 彼は変態です。 | Hij is een schokkerige. |
| Duidelijke | Ongepaste woorden zijn omgeven door XML \<-codes >... \</profanity >. | 彼は変態です。 | Hij is een \<Grove\<>e/profanity->. |
| Verwijderd | Ongepaste woorden worden verwijderd uit de uitvoer zonder vervanging. | 彼は。 | Hij is een. |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Filters voor scheld woorden Toep assen met de API-aanroep van Translator](reference/v3-0-translate.md)
