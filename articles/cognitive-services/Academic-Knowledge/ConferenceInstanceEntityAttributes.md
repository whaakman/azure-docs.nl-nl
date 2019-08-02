---
title: Entiteits kenmerken van conferentie-exemplaar-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u kunt gebruiken met de entiteit conferentie-exemplaar in het Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 37a353fbb86ca199b2316dcfba5904f4b46b0276
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705063"
---
# <a name="conference-instance-entity"></a>Entiteit van het conferentie-exemplaar

<sub>* De volgende kenmerken zijn specifiek voor de entiteit van het conferentie-exemplaar. (Ty = '4') </sub>

Name    |Description                            |type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                              |Int64      |Is gelijk aan
CIN     |Genormaliseerde naam van het Conference-exemplaar ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Tekenreeks     |Is gelijk aan
DCN     |Weergave naam van Vergader instantie ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Tekenreeks     |geen
CIL     |Locatie van het Conference-exemplaar    |Tekenreeks     |Gelijk is aan<br/>StartsWith
CISD    |Begin datum van het conferentie-exemplaar  |Date       |Gelijk is aan<br/>IsBetween
CIED    |Eind datum van het Vergader exemplaar    |Date       |Gelijk is aan<br/>IsBetween
CIARD   |De verval datum van de abstracte registratie van het conferentie-exemplaar  |Date       |Gelijk is aan<br/>IsBetween
CISDD   |Verval datum van de inzending van het conferentie-exemplaar     |Date       |Gelijk is aan<br/>IsBetween
CIFVD   |Eind datum van de laatste versie van het conferentie-exemplaar  |Date       |Gelijk is aan<br/>IsBetween
CINDD   |Meldings datum van het conferentie-exemplaar   |Date       |Gelijk is aan<br/>IsBetween
CD.T    |Titel van een conferentie-exemplaar gebeurtenis   |Date       |Gelijk is aan<br/>IsBetween
CD.D    |Datum van een conferentie-exemplaar gebeurtenis    |Date       |Gelijk is aan<br/>IsBetween
TABLETS. GENOEMD  |Naam van de vergaderings reeks van het exemplaar |Reeks     |Is gelijk aan
Tablets. CId |Reeks-ID van de vergadering van het exemplaar |Int64    |Is gelijk aan
CC      |Totaal aantal vermeldingen van conferentie-exemplaar           |Int32      |geen  
ECC     |Totaal aantal verwachte bron vermeldingen van conferentie-exemplaar |Int32      |geen


## <a name="extended-metadata-attributes"></a>Uitgebreide meta gegevens kenmerken ##

Name    | Description               
--------|---------------------------    
FN      | Volledige naam van het Conference-exemplaar
