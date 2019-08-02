---
title: Kenmerken van papier entiteit-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u kunt gebruiken met de Paper-entiteit in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 4b9431469a7925d26003ad9c34f6b401e5767f6d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704946"
---
# <a name="paper-entity"></a>Paper-entiteit

<sub>* Onderstaande kenmerken zijn specifiek voor de Paper-entiteit. (Ty = '0') </sub>


Name    |Description                                        |type       | Bewerkingen
------- | ------------------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                                          |Int64      |Is gelijk aan
Tij      |Titel van het document                                        |Reeks     |Gelijk is aan<br/>StartsWith
L       |Taal code van het papier gescheiden\@door ""\@\@          |Tekenreeks     |Is gelijk aan
J       |Papier jaar                                         |Int32      |Gelijk is aan<br/>IsBetween
D       |Papier datum                                         |Date       |Gelijk is aan<br/>IsBetween
CC      |Aantal citaten                                     |Int32      |geen  
ECC     |Aantal geschatte citaten                           |Int32      |geen
AA.AuN  |Naam van auteur                                        |Tekenreeks     |Gelijk is aan<br/>StartsWith
AA.AuId |Auteur-ID                                          |Int64      |Is gelijk aan
A. AfN  |Naam van de auteurs aansluiting                            |Tekenreeks     |Gelijk is aan<br/>StartsWith
A. AfId |ID van de auteurs relatie                              |Int64      |Is gelijk aan
A. Z    |De auteur van het papier best Ellen                         |Int32      |Is gelijk aan
F.FN    |Veld met studie naam                                |Tekenreeks     |Gelijk is aan<br/>StartsWith
F.FId   |Veld met studie-ID                                  |Int64      |Is gelijk aan
J.JN    |Journaal naam                                       |Tekenreeks     |Gelijk is aan<br/>StartsWith
J.JId   |Journaal-ID                                         |Int64      |Is gelijk aan
C.CN    |Naam van de vergaderings reeks                             |Tekenreeks     |Gelijk is aan<br/>StartsWith
C.CId   |Reeks-ID van vergadering                               |Int64      |Is gelijk aan
RId     |ID van de Paper waarnaar wordt verwezen                              |Int64 []    |Is gelijk aan
W       |Woorden van de document titel en de samen vatting                |String[]   |Is gelijk aan
E       |Uitgebreide meta gegevens (Zie de onderstaande tabel)                |Reeks     |geen  
        


## <a name="extended-metadata-attributes"></a>Uitgebreide meta gegevens kenmerken ##

Name    | Description               
--------|---------------------------    
DN-NAAM      | Weergave naam van het papier 
Z       | Bronnen: lijst met webbronnen van het papier, gesorteerd op statische positie
S. ty    | Bron type (1: HTML, 2: tekst, 3: PDF, 4: DOC, 5: PPT, 6: XLS, 7: PS)
S. U     | Bron-URL
VFN     | Volledige naam van de platform-volledige naam van het logboek of de vergadering
VSN     | Locatie korte naam-korte naam van het logboek of de conferentie
V       | Volume-journaal volume
BV      | Journaal naam
BT      | 
PB      | Afkortingen van journalen
I       | Probleem-logboek probleem
FP      | Eerste pagina van papier
SNELHEID      | LastPage-laatste pagina van papier
DOI     | Digitale object-id
CC      | Citaten: een lijst met Paper-id's waarnaar wordt verwezen, en de bijbehorende context in het papier (bijvoorbeeld [{123: ["bruin Foxes zijn bekend als waarnaar wordt verwezen in papier 123", "de luie honden zijn een historisch misnomer, zoals weer gegeven in Paper 123"]})
A      | Omgekeerde samen vatting
A. IndexLength| Aantal items in de index (aantal woorden van de abstractie)
IA.InvertedIndex| Lijst met abstracte woorden en de bijbehorende positie in de oorspronkelijke samen vatting (bijvoorbeeld [{": [0, 15, 30]}, {" Brown ": [1]}, {" Fox ": [2]}])
