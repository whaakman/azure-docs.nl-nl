---
title: bestand opnemen
description: bestand opnemen
services: scheduler
ms.service: scheduler
author: derek1ee
ms.topic: include
ms.date: 08/16/2016
ms.author: deli
ms.custom: include file
ms.openlocfilehash: b3788ede23a423bebf96661ea88b227bfb5fdf4c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553372"
---
De volgende tabel beschrijft elke van de belangrijkste quota's, limieten, standaardwaarden en vertragingen in Azure Scheduler.

| Resource | Beschrijving van de limiet |
| -------- | ----------------- |
| **Grootte van de taak** | De maximale grootte is 16.000. Als een PUT of PATCH-bewerking in een groter is dan deze limiet taakgrootte resulteert, wordt een statuscode 400 Ongeldige aanvraag geretourneerd. | 
| **Taakcollecties** | Het maximum aantal taakcollecties per Azure-abonnement is 200.000. | 
| **Taken per verzameling** | Standaard is het maximale aantal taken vijf taken in een gratis taakverzameling en 50 taken in een standard taakcollectie. <p>U kunt het maximum aantal taken op een taakverzameling wijzigen. Alle taken in een jobverzameling is beperkt tot de waarde voor de taakverzameling is ingesteld. Als u probeert te maken van meer taken dan het maximum aantal taken quotum, wordt de aanvraag mislukt met statuscode 409 Conflict. | 
| **Tijd tot tijd** | Het maximum 'time begintijd' is 18 maanden. |
| **Bereik van terugkeerpatroon** | Het maximale terugkeerpatroon dat bereik is 18 maanden. | 
| **Frequentie** | Het quotum maximale frequentie is standaard één uur in een gratis taakverzameling en één minuut in een standard taakcollectie. <p>U kunt de maximale frequentie maken op een taakverzameling lager is dan het maximum. Alle taken in de taakverzameling is beperkt tot de waarde voor de taakverzameling is ingesteld. Als u probeert te maken van een taak met een hogere frequentie dan de maximale frequentie voor de taakverzameling, wordt de aanvraag mislukt met statuscode 409 Conflict. | 
| **Grootte van standaardberichthoofdtekst** | De hoofdtekst van de maximale grootte voor een aanvraag is 8192 tekens. |
| **Grootte van de aanvraag-URL** | De maximale grootte voor een aanvraag-URL is 2048 tekens. |
| **Header-aantal** | Het aantal maximale header is 50 headers. | 
| **Cumulatieve header-grootte** | De maximale cumulatieve header-grootte is 4096 tekens. |
| **Timeout** | Time-out van de aanvraag is statisch, dat wil zeggen, niet worden geconfigureerd. en is 60 seconden voor HTTP-acties. Voor meer actieve bewerkingen, gaat u als volgt de asynchrone HTTP-protocollen. Bijvoorbeeld, een 202 onmiddellijk resultaten retourneerde, maar blijven werken op de achtergrond. | 
| **Jobgeschiedenis** | De maximale antwoordtekst die zijn opgeslagen in de taakgeschiedenis is 2048 bytes. |
| **Geschiedenis vasthouden** | Taakgeschiedenis wordt opgeslagen voor maximaal twee maanden of tot de laatste 1000 uitvoeringen. | 
| **Voltooide en mislukte taak bewaren** | Voltooide en mislukte jobs worden bewaard gedurende 60 dagen. |
||| 

