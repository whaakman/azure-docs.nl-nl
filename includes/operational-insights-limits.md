---
title: bestand opnemen
description: bestand opnemen
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 03/29/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 29256b3bcfedb7fe5045ff4c6c3842eb25e00a28
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
De volgende limieten gelden voor Log Analytics-resources per abonnement:

| Resource | Standaardlimiet | Opmerkingen
| --- | --- | --- |
| Aantal gratis werkruimten per abonnement | 10 | Deze limiet kan niet worden verhoogd. |
| Aantal betaalde werkruimten per abonnement | N/A | Er gelden limieten voor het aantal resources binnen een resourcegroep en voor het aantal resourcegroepen per abonnement | 

>[!NOTE]
>Vanaf 2 April 2018, worden automatisch nieuwe werkruimten in een nieuw abonnement gebruikt de *Per GB* plan prijzen.  Voor bestaande abonnementen die zijn gemaakt vóór 2 April of een abonnement dat is gekoppeld aan een bestaande EA-inschrijving, kunt u kiezen tussen de drie Prijscategorieën voor nieuwe werkruimten blijven. 
>

De volgende limieten zijn van toepassing op elke Log Analytics-werkruimte:

|  | Gratis | Standard | Premium | Zelfstandig | OMS | Per GB |
| --- | --- | --- | --- | --- | --- |--- |
| Gegevensvolume verzameld per dag |500 MB<sup>1</sup> |Geen |None | Geen | Geen | Geen
| Bewaarperiode van gegevens |7 dagen |1 maand |12 maanden | 1 maand<sup>2</sup> | 1 maand <sup>2</sup>| 1 maand <sup>2</sup>|

<sup>1</sup> Als een klant de dagelijkse limiet van 500 MB voor gegevensoverdracht heeft bereikt, stopt het analyseren van gegevens. Dit wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC.

<sup>2</sup> de bewaarperiode voor gegevens voor de zelfstandige, OMS en Per GB prijsstelling kan worden verhoogd tot 730 dagen.

| Category | Limieten | Opmerkingen
| --- | --- | --- |
| Gegevensverzamelaar-API | Maximumgrootte voor een enkel bericht is 30 MB<br>Maximumgrootte voor veldwaarden is 32 KB | Grotere volumes splitsen in meerdere berichten<br>Velden die langer zijn dan 32 KB worden afgebroken. |
| API voor zoeken | 5000 records geretourneerd voor niet-samengevoegde gegevens<br>500.000 records voor samengevoegde gegevens | Samengevoegde gegevens is een zoekopdracht die de opdracht `measure` omvat
 
