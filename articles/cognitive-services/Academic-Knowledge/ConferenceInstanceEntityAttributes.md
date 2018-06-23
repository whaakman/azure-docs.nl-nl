---
title: Telefonische vergaderingen exemplaar entiteitskenmerken in de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de kenmerken die u met de entiteit conferentie exemplaar in de Academic Knowledge API in cognitieve Services gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ef2bca4346a4666905f3dfb7bd448720f3b0ef8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344412"
---
# <a name="conference-instance-entity"></a>Telefonische vergaderingen exemplaar entiteit

<sub> * De volgende kenmerken zijn specifiek voor conferentie exemplaar entiteit. (Ty = '4') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
CIN     |Telefonische vergaderingen genormaliseerde exemplaarnaam ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Reeks     |Is gelijk aan
DCN     |Weergavenaam van de Conferentie-instantie ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Reeks     |geen
CIL     |Locatie van het exemplaar van de Conferentie    |Reeks     |Is gelijk aan,<br/>StartsWith
CISD    |De begindatum van de Conferentie-exemplaar  |Date       |Is gelijk aan,<br/>IsBetween
CIED    |Einddatum van het exemplaar van de Conferentie    |Date       |Is gelijk aan,<br/>IsBetween
CIARD   |Registratie van abstracte vervaldatum van het exemplaar van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CISDD   |Verzending van de vervaldatum van het exemplaar van de Conferentie     |Date       |Is gelijk aan,<br/>IsBetween
CIFVD   |Definitieve versie vervaldatum van het exemplaar van de Conferentie  |Date       |Is gelijk aan,<br/>IsBetween
CINDD   |Datum van het exemplaar van de Conferentie bericht   |Date       |Is gelijk aan,<br/>IsBetween
CD. T    |Titel van een conferentie exemplaar van gebeurtenis   |Date       |Is gelijk aan,<br/>IsBetween
CD. D    |Datum van een conferentie exemplaar van gebeurtenis    |Date       |Is gelijk aan,<br/>IsBetween
PC 'S. ALGEMENE NAAM  |Telefonische vergaderingen reeksnaam van het exemplaar |Reeks     |Is gelijk aan
PC 'S. CId |Telefonische vergaderingen reeks-ID van het exemplaar |Int64    |Is gelijk aan
CC      |Telefonische vergaderingen totale instantiedocumentcitaat exemplaren           |Int32      |geen  
ECC     |Telefonische vergaderingen totale geschatte instantiedocumentcitaat exemplaren |Int32      |geen


## <a name="extended-metadata-attributes"></a>Uitgebreide Metagegevenskenmerken ##

Naam    | Beschrijving               
--------|---------------------------    
FN      | Telefonische vergaderingen exemplaar volledige naam