---
title: Conferentie exemplaar entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de Conferentie entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 6111ad00044943f12b2e098c4fd07ffb40185799
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902409"
---
# <a name="conference-instance-entity"></a>Conferentie exemplaar entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie exemplaar entiteit. (Ty = "4") </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
CIN     |Conferentie genormaliseerde exemplaarnaam ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Reeks     |Is gelijk aan
DCN     |Weergavenaam van de Conferentie-instantie ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Reeks     |geen
CIL     |Locatie van de Conferentie    |Reeks     |Is gelijk aan,<br/>StartsWith
CISD    |De begindatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CIED    |Einddatum van de Conferentie    |Date       |Is gelijk aan,<br/>IsBetween
CIARD   |Abstracte registratie vervaldatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CISDD   |Verzending van vervaldatum van de Conferentie     |Date       |Is gelijk aan,<br/>IsBetween
CIFVD   |Definitieve versie vervaldatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CINDD   |Datum van de melding van de Conferentie   |Date       |Is gelijk aan,<br/>IsBetween
CD. T    |Titel van een conferentie exemplaar van gebeurtenis   |Date       |Is gelijk aan,<br/>IsBetween
CD. D    |Datum van een conferentie exemplaar van gebeurtenis    |Date       |Is gelijk aan,<br/>IsBetween
PC 'S. ALGEMENE NAAM  |Naam van de Conferentie reeks van het exemplaar |Reeks     |Is gelijk aan
PC 'S. CId |Conferentie reeks-ID van het exemplaar |Int64    |Is gelijk aan
CC      |De totale citaat exemplaren Conferentie           |Int32      |geen  
ECC     |De totale geschatte citaat exemplaren Conferentie |Int32      |geen


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Naam    | Beschrijving               
--------|---------------------------    
FN      | Volledige naam Conferentie