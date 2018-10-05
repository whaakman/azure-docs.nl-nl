---
title: Gelijktijdigheidsbeheer | Microsoft Docs
description: Gelijktijdigheid besturingselement strategieën voor het publiceren van API's van Cloud-Partner-Portal.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ecf0bb6ac7fc77e804c9fc8d62aba52810de5640
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809996"
---
<a name="concurrency-control"></a>Gelijktijdigheid beheren
===================

Voor elke aanroep naar de Cloud Partner-Portal publiceren van API's moet expliciet opgeven welke gelijktijdigheid-strategie voor toegangsbeheer te gebruiken. Fout voor de **If-Match** header zal resulteren in reactie op een HTTP-fout 400-fout. We bieden twee strategieën voor het besturingselement voor gelijktijdigheid van taken.

-   **Optimistische** -de client voor het uitvoeren van de update wordt gecontroleerd of de gegevens is gewijzigd nadat deze de gegevens voor het laatst hebt gelezen.
-   **Laatste een wins** -gegevens, ongeacht of een andere toepassing wordt gewijzigd sinds de laatste tijd gelezen rechtstreeks worden bijgewerkt door de client.

<a name="optimistic-concurrency-workflow"></a>Optimistische gelijktijdigheid-werkstroom
-------------------------------

U wordt aangeraden de optimistische gelijktijdigheid-strategie, met de volgende werkstroom gebruiken om te waarborgen dat geen onverwachte wijzigingen zijn aangebracht in uw resources.

1.  Een entiteit met behulp van de API's worden opgehaald. Het antwoord bevat een ETag-waarde die aangeeft van de momenteel opgeslagen versie van de entiteit (op het moment van het antwoord).
2.  Op het moment van bijwerken, kunt u deze dezelfde ETag-waarde bevatten in de verplichte **If-Match** aanvraagheader.
3.  De API vergelijkt de ETag-waarde ontvangen in de aanvraag met de huidige ETag-waarde van de entiteit in een atomic-transacties.
    *   Als de ETag-waarden verschillend zijn, de API retourneert een `412 Precondition Failed` HTTP-antwoord. Deze fout geeft aan dat een van beide een ander proces de entiteit is bijgewerkt sinds het laatste in de client worden opgehaald, of dat de ETag-waarde die is opgegeven in de aanvraag is onjuist.
    *  Als de ETag-waarden hetzelfde zijn, of de **If-Match** header bevat het sterretje jokerteken (`*`), de API de aangevraagde bewerking wordt uitgevoerd. De API-bewerking werkt ook de opgeslagen ETag-waarde van de entiteit.


> [!NOTE]
> Het jokerteken (*) op te geven de **If-Match** header resulteert in de API met behulp van de laatste één wins gelijktijdigheid-strategie. In dit geval de ETag-vergelijking wordt niet uitgevoerd en de resource wordt bijgewerkt zonder de controles. 
