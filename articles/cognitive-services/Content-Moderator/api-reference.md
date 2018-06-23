---
title: Azure Content beheerder API-referentiemateriaal | Microsoft Docs
description: Informatie over de inhoud toezicht en API's voor de beheerder van de inhoud te controleren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 94de9b91cc242e8b7e5479cacab8380adac551f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344468"
---
# <a name="content-moderator-api-reference"></a>Referentie voor de Content Moderator-API

U aan de slag met Azure inhoud beheerder API's in de volgende manieren: (Zie ook [Referentiebeheer](review-tool-user-guide/credentials.md).)

- In de Azure portal [abonneren op de inhoud beheerder-API's](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Aanmelden voor de [inhoud beheerder revisie hulpprogramma](http://contentmoderator.cognitive.microsoft.com/). Zie [Quick Start](quick-start.md).

## <a name="moderation-apis"></a>Beheer-API's

U kunt de volgende inhoud beheerder API's gebruiken voor het instellen van uw werkstromen na controle.

| Beschrijving | Referentie |
| -------------------- |-------------|
| **Afbeelding toezicht API**<br /><br />Afbeeldingen scannen en potentiële inhoud voor volwassenen en mooie detecteren met behulp van tags, vertrouwen scores en andere uitgepakte informatie. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud in uw werkstroom na controle. <br /><br />| [Afbeelding van toezicht API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "installatiekopie toezicht API-referentiemateriaal")   |
| **Tekst toezicht API**<br /><br />Scan tekstinhoud. Taalgebruik voorwaarden en persoonsgegevens (PII) worden geretourneerd. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud in uw werkstroom na controle.<br /><br /> | [Tekst toezicht API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "tekst toezicht API-referentiemateriaal")   |
| **Video toezicht API**<br /><br />Scannen van video's en potentiële inhoud voor volwassenen en mooie detecteren. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud in uw werkstroom na controle.<br /><br /> | [Video toezicht API-overzicht](video-moderation-api.md "Video toezicht API-overzicht")   |
| **Lijst van beheer-API**<br /><br />Maak en beheer van aangepaste uitsluitings- of insluitingsfilters lijsten van afbeeldingen en tekst. Indien ingeschakeld, de **afbeelding: overeen met** en **tekst - scherm** bewerkingen uitvoeren fuzzy overeenkomst met de ingediende inhoud op basis van uw aangepaste lijsten. <br /><br />Voor efficiëntie, kunt u de machine learning gebaseerde controle stap overslaan.<br /><br /> | [Lijst van Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "lijst Management API-referentiemateriaal")   |

## <a name="review-api"></a>Controle-API

De controle-API heeft de volgende onderdelen:

| Beschrijving | Referentie |
| -------------------- |-------------|
| **Taken**<br /><br /> Scan en revisie toezicht werkstromen voor de afbeelding en de tekst inhoud initiëren. Een taak toezicht scant uw inhoud met behulp van de API van de installatiekopie van toezicht en de tekst toezicht-API. Toezicht taken gebruiken de gedefinieerde en werkstromen voor het genereren van beoordelingen standaard. <br /><br />Nadat een menselijke beheerder heeft de labels automatisch toegewezen en de gegevens van de voorspelling gecontroleerd en een beslissing inhoud toezicht verzonden, verzendt de API Controleer alle gegevens naar uw API-eindpunt.<br /><br /> | [Taak verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "taak verwijzing")   |
| **Beoordelingen**<br /><br />Gebruik het hulpprogramma revisie rechtstreeks afbeelding of tekst beoordelingen voor menselijke moderators maken.<br /><br /> Nadat een menselijke beheerder heeft de labels automatisch toegewezen en de gegevens van de voorspelling gecontroleerd en een beslissing inhoud toezicht verzonden, verzendt de API Controleer alle gegevens naar uw API-eindpunt.<br /><br /> | [Verwijzing bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "verwijzing bekijken")   |
| **Werkstromen**<br /><br />Maken, bijwerken en meer informatie over de aangepaste werkstromen die uw team maakt. U definiëren werkstromen met behulp van het hulpprogramma voor beoordeling. <br /> <br />Werkstromen doorgaans inhoud beheerder, maar ook bepaalde andere API's die beschikbaar als connectors in het hulpprogramma voor beoordeling zijn kunnen gebruiken.<br /><br /> | [Werkstroom verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "werkstroom verwijzing")   |


