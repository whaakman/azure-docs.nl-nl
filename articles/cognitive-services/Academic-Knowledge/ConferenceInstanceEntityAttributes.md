---
title: Conferentie exemplaar entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de Conferentie entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 183a307159adb5dfdb248eb0cf4862462a626db6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879187"
---
# <a name="conference-instance-entity"></a>Conferentie exemplaar entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie exemplaar entiteit. (Ty = '4') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
CIN     |Conferentie genormaliseerde exemplaarnaam ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |String     |Is gelijk aan
DCN     |Weergavenaam van de Conferentie-instantie ({ConferenceSeriesName}: {ConferenceInstanceYear})       |String     |geen
CIL     |Locatie van de Conferentie    |String     |Is gelijk aan,<br/>StartsWith
CISD    |De begindatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CIED    |Einddatum van de Conferentie    |Date       |Is gelijk aan,<br/>IsBetween
CIARD   |Abstracte registratie vervaldatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CISDD   |Verzending van vervaldatum van de Conferentie     |Date       |Is gelijk aan,<br/>IsBetween
CIFVD   |Definitieve versie vervaldatum van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CINDD   |Datum van de melding van de Conferentie   |Date       |Is gelijk aan,<br/>IsBetween
CD.T    |Titel van een conferentie exemplaar van gebeurtenis   |Date       |Is gelijk aan,<br/>IsBetween
CD.D    |Datum van een conferentie exemplaar van gebeurtenis    |Date       |Is gelijk aan,<br/>IsBetween
PCS.CN  |Naam van de Conferentie reeks van het exemplaar |String     |Is gelijk aan
PCS.CId |Conferentie reeks-ID van het exemplaar |Int64    |Is gelijk aan
CC      |De totale citaat exemplaren Conferentie           |Int32      |geen  
ECC     |De totale geschatte citaat exemplaren Conferentie |Int32      |geen


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Description               
--------|---------------------------    
FN      | Volledige naam Conferentie
