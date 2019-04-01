---
title: Informatie over beoordeling hulpprogramma concepten - Content Moderator
titlesuffix: Azure Cognitive Services
description: Meer informatie over het hulpprogramma Content Moderator bekijken, een website die een gecombineerde AI en menselijke beoordeling toezicht moeite coördineert.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: sajagtap
ms.openlocfilehash: b7ec997fd3e9bfe294050893d80fd57a96a47aae
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755885"
---
# <a name="content-moderator-review-tool"></a>Content Moderator-controlehulpprogramma

Azure Content Moderator biedt services voor het combineren van machine learning-inhoudstoezicht met onlinebeoordelingen door mensen, en de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com) website is een gebruiksvriendelijke front-end met uitgebreide toegang tot deze services.

![Het dashboard van het hulpprogramma controleren in een browser](./images/0-dashboard.png)

## <a name="what-it-does"></a>Wat het doet

De [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com), wanneer gebruikt in combinatie met het geautomatiseerd beheer-API's, kunt u de volgende taken in het proces inhoudstoezicht:

- Gebruik één set hulpprogramma's voor het beheren van inhoud in verschillende indelingen (tekst, afbeeldingen en video).
- Het maken van menselijke automatiseren [beoordeelt](../review-api.md#reviews) wanneer afbeeldingstoezicht-API komen resulteert in.
- Toewijzen of escaleren inhoud beoordelingen aan meerdere revisie teams, onderverdeeld naar categorie of ervaring niveau van inhoud.
- Gebruik standaard of aangepaste logica filters ([werkstromen](../review-api.md#workflows)) voor het sorteren en bijhouden van inhoud, zonder eventuele code te schrijven.
- Gebruik [connectors](./configure.md#connectors) voor het verwerken van inhoud met Microsoft PhotoDNA, Tekstanalyse en Face-API's, naast de inhoud Moderator-API's.
- Bouw uw eigen connector voor het maken van werkstromen API's of zakelijke processen.
- Prestatie metrische gegevens op uw processen inhoudstoezicht ophalen.

## <a name="review-tool-dashboard"></a>Beoordeling hulpprogramma dashboard

Op de **Dashboard** tabblad ziet u belangrijke metrische gegevens voor inhoud door in het hulpprogramma wordt uitgevoerd. Raadpleeg het nummer van totaal, voltooid, en in afwachting van beoordelingen van afbeeldingen, tekst en video-inhoud. U ziet ook de uitsplitsing van gebruikers en teams die hebt voltooid, beoordelingen, evenals de beheer-labels die zijn toegepast.

![Bekijk het dashboard](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Hulpprogramma referenties controleren

Wanneer u zich aanmeldt met de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com), u wordt gevraagd om te selecteren van een Azure-regio voor u. Dit komt doordat de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com) genereert een gratis proefversie sleutel voor Azure Content Moderator-services, moet u deze sleutel voor toegang tot alle van de services van een REST-aanroep of client-SDK. U kunt uw sleutel en de API-eindpunt-URL weergeven door het selecteren van **instellingen** > **referenties**.

![Content Moderator-referenties](images/settings-6-credentials.png)

## <a name="next-steps"></a>Volgende stappen

Zie [configureren van het beoordelingsprogramma](./configure.md) voor meer informatie over hoe u toegang tot revisie hulpprogramma bronnen en instellingen te wijzigen.