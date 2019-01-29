---
title: Conferentie exemplaar entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de Conferentie entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: db025f377a3fab2f788252db0c8e3555837a6de8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196103"
---
# <a name="conference-instance-entity"></a>Conferentie exemplaar entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie exemplaar entiteit. (Ty = '4') </sub>

Name    |Description                            |Type       | Bewerkingen
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
CD.T    |Titel van een conferentie exemplaar van gebeurtenis   |Date       |Is gelijk aan,<br/>IsBetween
CD.D    |Datum van een conferentie exemplaar van gebeurtenis    |Date       |Is gelijk aan,<br/>IsBetween
PCS.CN  |Naam van de Conferentie reeks van het exemplaar |Reeks     |Is gelijk aan
PCS.CId |Conferentie reeks-ID van het exemplaar |Int64    |Is gelijk aan
CC      |De totale citaat exemplaren Conferentie           |Int32      |geen  
ECC     |De totale geschatte citaat exemplaren Conferentie |Int32      |geen


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Beschrijving               
--------|---------------------------    
FN      | Volledige naam Conferentie
