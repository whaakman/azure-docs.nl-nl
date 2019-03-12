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
ms.openlocfilehash: 34f2ab8f7ccafb8b30e298cd71e09171ad8c87cb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553690"
---
De volgende limieten gelden voor Azure Log Analytics-resources per abonnement.

| Resource | Standaardlimiet | Opmerkingen
| --- | --- | --- |
| Aantal gratis werkruimten per abonnement | 10 | Deze limiet kan niet worden verhoogd. |
| Aantal betaalde werkruimten per abonnement | N/A | U bent beperkt door het aantal resources binnen een resourcegroep en het aantal resourcegroepen per abonnement. | 

>[!NOTE]
>Vanaf 2 April 2018 gebruiken nieuwe werkruimten in een nieuw abonnement automatisch de *Per GB* prijsplan. Voor bestaande abonnementen die zijn gemaakt vóór 2 April, of een abonnement dat is gekoppeld aan een bestaande Enterprise Agreement-inschrijving, kunt u kiezen uit de drie Prijscategorieën voor nieuwe werkruimten blijven. 
>

De volgende limieten gelden voor elke Log Analytics-werkruimte.

|  | Gratis | Standard | Premium | Zelfstandig | OMS | Per GB |
| --- | --- | --- | --- | --- | --- |--- |
| Gegevensvolume verzameld per dag |500 MB<sup>1</sup> |Geen |Geen | Geen | Geen | Geen
| Bewaarperiode van gegevens |7 dagen |1 maand |12 maanden | 1 maand<sup>2</sup> | 1 maand<sup>2</sup>| 1 maand<sup>2</sup>|

<sup>1</sup>wanneer klanten bereiken hun 500 MB dagelijkse limiet voor gegevensoverdracht, analyse van gegevens gestopt en wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC.

<sup>2</sup>de bewaarperiode van gegevens voor de zelfstandige, OMS en Per GB-prijsplannen kan worden verhoogd tot 730 dagen.

| Categorie | Limieten | Opmerkingen
| --- | --- | --- |
| Gegevensverzamelaar-API | Maximale grootte voor een enkel bericht is 30 MB.<br>Maximumgrootte voor veldwaarden is 32 KB. | Grotere volumes splitsen in meerdere berichten.<br>Velden die langer zijn dan 32 KB worden afgebroken. |
| API voor zoeken | 5000 records geretourneerd voor niet-samengevoegde gegevens.<br>500.000 records voor samengevoegde gegevens. | Samengevoegde gegevens is een zoekopdracht met de `summarize` opdracht.
 
