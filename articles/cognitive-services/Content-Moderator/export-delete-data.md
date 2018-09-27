---
title: Exporteren of verwijderen van uw gegevens - Content Moderator
titlesuffix: Azure Cognitive Services
description: Informatie over het exporteren of verwijderen van uw gegevens in de Content Moderator.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e671fa9735406ec425b105bb121d3a029ef19dc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227228"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exporteren of verwijderen van gebruikersgegevens in de Content Moderator

Content Moderator verzamelt gebruikersgegevens als de service wilt gebruiken, maar klanten hebben volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens met behulp van de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) en de [API's](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Zie de volgende tabel voor meer informatie over het exporteren en verwijderen van gebruikersgegevens in de Content Moderator.

| Gegevens | De exportbewerking | Verwijderbewerking |
| ---- | ---------------- | ---------------- |
| Accountgegevens (Abonnementssleutels) | N/A | Verwijderen met behulp van de Azure portal (Azure-abonnementen). Of gebruik de **Team verwijderen** knop in de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) pagina Team-instellingen. |
| Afbeeldingen voor aangepaste koppeling | [Installatiekopie-id's ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Installatiekopieën worden opgeslagen in een one-way hash van de oorspronkelijke indeling en is er geen manier om op te halen van de werkelijke installatiekopieën. | [Verwijdert alle installatiekopieën](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Of verwijder de Content Moderator-resource met behulp van de Azure-portal. |
| Voorwaarden voor het aangepaste vergelijken | [Ophalen van alle termen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Verwijderen van alle voorwaarden](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Of verwijder de Content Moderator-resource met behulp van de Azure-portal. |
| Tags | N/A | Gebruik de **verwijderen** pictogram beschikbaar voor elke tag op in de pagina van de instellingen voor controle UI-Tag. Of gebruik de **Team verwijderen** knop in de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) pagina Team-instellingen. |
| Beoordelingen | [Revisie ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Gebruik de **Team verwijderen** knop in de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) pagina Team-instellingen.
| Gebruikers | N/A | Gebruik de **verwijderen** pictogram beschikbaar voor elke gebruiker in de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) pagina Team-instellingen. Of gebruik de **Team verwijderen** knop in de [revisie gebruikersinterface](http://contentmoderator.cognitive.microsoft.com/) pagina Team-instellingen. |

