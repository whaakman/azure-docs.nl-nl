---
title: bestand opnemen
description: bestand opnemen
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 05/16/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 66cd09df128d454973d008adf4ffc5dd1017a18f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755588"
---
De volgende limieten gelden voor Log Analytics-resources per abonnement:

| Resource | Standaardlimiet | Opmerkingen
| --- | --- | --- |
| Aantal gratis werkruimten per abonnement | 10 | Deze limiet kan niet worden verhoogd. |
| Aantal betaalde werkruimten per abonnement | N/A | Er gelden limieten voor het aantal resources binnen een resourcegroep en voor het aantal resourcegroepen per abonnement | 

>[!NOTE]
>Vanaf 2 April 2018, worden automatisch nieuwe werkruimten in een nieuw abonnement gebruikt de *Per GB* prijsplan.  Voor bestaande abonnementen die zijn gemaakt vóór 2 April, of een abonnement dat is gekoppeld aan een bestaande EA-inschrijving, kunt u nog steeds te kiezen tussen de drie Prijscategorieën voor nieuwe werkruimten. 
>

De volgende limieten zijn van toepassing op elke Log Analytics-werkruimte:

|  | Gratis | Standard | Premium | Zelfstandig | OMS | Per GB |
| --- | --- | --- | --- | --- | --- |--- |
| Gegevensvolume verzameld per dag |500 MB<sup>1</sup> |Geen |Geen | Geen | Geen | Geen
| Bewaarperiode van gegevens |7 dagen |1 maand |12 maanden | 1 maand<sup>2</sup> | 1 maand <sup>2</sup>| 1 maand <sup>2</sup>|

<sup>1</sup> Als een klant de dagelijkse limiet van 500 MB voor gegevensoverdracht heeft bereikt, stopt het analyseren van gegevens. Dit wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC.

<sup>2</sup> de bewaarperiode van gegevens voor de zelfstandige, OMS en Per GB-prijsplannen kan worden verhoogd tot 730 dagen.

| Categorie | Limieten | Opmerkingen
| --- | --- | --- |
| Gegevensverzamelaar-API | Maximumgrootte voor een enkel bericht is 30 MB<br>Maximumgrootte voor veldwaarden is 32 KB | Grotere volumes splitsen in meerdere berichten<br>Velden die langer zijn dan 32 KB worden afgebroken. |
| API voor zoeken | 5000 records geretourneerd voor niet-samengevoegde gegevens<br>500.000 records voor samengevoegde gegevens | Samengevoegde gegevens is een zoekopdracht die de opdracht `summarize` omvat
 
