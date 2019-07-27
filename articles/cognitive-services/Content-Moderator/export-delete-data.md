---
title: Gebruikers gegevens exporteren of verwijderen-Content Moderator
titleSuffix: Azure Cognitive Services
description: Meer informatie over het exporteren of verwijderen van uw gegevens in Content Moderator.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: b18bc2907c00ed1424db440f09c914faff6a9863
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565571"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Gebruikers gegevens exporteren of verwijderen in Content Moderator

Content Moderator verzamelt gebruikers gegevens om de service te kunnen gebruiken, maar klanten hebben volledig beheer over het weer geven, exporteren en verwijderen van hun gegevens met behulp van het [hulp programma beoordeling](https://contentmoderator.cognitive.microsoft.com/) en de [api's voor toezicht en controle](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Zie de volgende tabel voor meer informatie over het exporteren en verwijderen van gebruikers gegevens in Content Moderator.

| Data | Export bewerking | Verwijderbewerking |
| ---- | ---------------- | ---------------- |
| Account gegevens (abonnements sleutels) | N/A | Verwijderen met de Azure Portal (Azure-abonnementen). U kunt ook de knop **team verwijderen** gebruiken op de pagina instellingen van [gebruikers interface team controleren](https://contentmoderator.cognitive.microsoft.com/) . |
| Afbeeldingen voor aangepaste overeenkomende | Roep de [API voor het ophalen van installatie kopie-Id's aan](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Installatie kopieën worden opgeslagen in een eigen hash-indeling in één richting en er is geen manier om de daad werkelijke installatie kopieën te extra heren. | Roep de [API voor het verwijderen van alle afbeeldingen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686)aan. Of verwijder de Content Moderator resource met behulp van de Azure Portal. |
| Voor waarden voor aangepaste overeenkomst | CAL de [alle voor waarden-API ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | Roep de [API alle termen verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d)aan. Of verwijder de Content Moderator resource met behulp van de Azure Portal. |
| Labels | N/A | Gebruik het pictogram **verwijderen** dat beschikbaar is voor elke tag op de pagina instellingen van de gebruikers interface voor het controleren van tags. U kunt ook de knop **team verwijderen** gebruiken op de pagina instellingen van [gebruikers interface team controleren](https://contentmoderator.cognitive.microsoft.com/) . |
| Beoordelingen | De API voor het [beoordelen van aanvragen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) aanroepen | Gebruik de knop **team verwijderen** op de pagina instellingen van [gebruikers interface team bekijken](https://contentmoderator.cognitive.microsoft.com/) .
| Gebruikers | N/A | Gebruik het pictogram **verwijderen** dat beschikbaar is voor elke gebruiker op de pagina instellingen van [gebruikers interface team controleren](https://contentmoderator.cognitive.microsoft.com/) . U kunt ook de knop **team verwijderen** gebruiken op de pagina instellingen van [gebruikers interface team controleren](https://contentmoderator.cognitive.microsoft.com/) . |

