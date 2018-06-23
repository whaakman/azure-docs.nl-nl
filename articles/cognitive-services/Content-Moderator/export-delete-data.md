---
title: Exporteren of verwijderen van uw gegevens in de inhoud van beheerder - cognitieve Azure-Services | Microsoft Docs
description: Informatie over het exporteren of verwijderen van uw gegevens in inhoud beheerder.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 1dbb645a033c6db5ffa9003a53f30fd927131298
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345738"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exporteren of gebruikersgegevens in inhoud beheerder verwijderen

Inhoud beheerder verzamelt gebruikersgegevens als de service wilt gebruiken, maar klanten hebben volledige controle over het weergeven en verwijderen van hun gegevens exporteren met behulp van de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) en de [API's](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Zie de volgende tabel voor meer informatie over het exporteren en verwijderen van gebruikersgegevens in inhoud beheerder.

| Gegevens | De exportbewerking | Verwijderbewerking |
| ---- | ---------------- | ---------------- |
| Accountgegevens (abonnement sleutels) | N/A | Verwijderen met de Azure portal (Azure-abonnementen). Of gebruik de **Team verwijderen** knop in de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) Team instellingenpagina. |
| Installatiekopieën voor aangepaste koppeling | [Installatiekopie-id's ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Installatiekopieën worden opgeslagen in een one-way hash van de eigen indeling en er is geen manier om op te halen van de werkelijke installatiekopieën. | [Verwijdert alle installatiekopieën](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Of verwijder de inhoud beheerder-resource met behulp van de Azure-portal. |
| Voorwaarden voor aangepaste koppeling | [Alle termen ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Verwijderen van alle voorwaarden](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Of verwijder de inhoud beheerder-resource met behulp van de Azure-portal. |
| Tags | N/A | Gebruik de **verwijderen** pictogram beschikbaar voor elke tag op de pagina van de instellingen controleren UI-Tag. Of gebruik de **Team verwijderen** knop in de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) Team instellingenpagina. |
| Beoordelingen | [Bekijk ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Gebruik de **Team verwijderen** knop in de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) Team instellingenpagina.
| Gebruikers | N/A | Gebruik de **verwijderen** pictogram beschikbaar voor elke gebruiker in de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) Team instellingenpagina. Of gebruik de **Team verwijderen** knop in de [revisie UI](http://contentmoderator.cognitive.microsoft.com/) Team instellingenpagina. |

