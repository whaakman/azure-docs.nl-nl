---
title: Papier entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit van het document in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 240541dd76221ee19c96b162da02ced90fb0ad90
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873084"
---
# <a name="paper-entity"></a>Document-entiteit

<sub> * Hieronder kenmerken zijn specifiek voor document-entiteit. (Ty = '0') </sub>


Name    |Description                                        |Type       | Bewerkingen
------- | ------------------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                                          |Int64      |Is gelijk aan
Ti      |Titel van document                                        |String     |Is gelijk aan,<br/>StartsWith
L       |Taalcode papier gescheiden door '\@@@ '            |String     |Is gelijk aan
J       |Papier jaar                                         |Int32      |Is gelijk aan,<br/>IsBetween
D       |Papier datum                                         |Date       |Is gelijk aan,<br/>IsBetween
CC      |Aantal citaat                                     |Int32      |geen  
ECC     |Geschatte citaat tellen                           |Int32      |geen
AA.AuN  |Naam van auteur                                        |String     |Is gelijk aan,<br/>StartsWith
AA.AuId |Auteur-ID                                          |Int64      |Is gelijk aan
AA. AfN  |Relatie is de naam van auteur                            |String     |Is gelijk aan,<br/>StartsWith
AA.AfId |De auteur van relatie-ID                              |Int64      |Is gelijk aan
AA. S    |Volgorde van de auteur van het document                         |Int32      |Is gelijk aan
F.FN    |Veld van de naam van het onderzoek                                |String     |Is gelijk aan,<br/>StartsWith
F.FId   |Veld van de ID van het onderzoek                                  |Int64      |Is gelijk aan
J.JN    |De naam van het logboek                                       |String     |Is gelijk aan,<br/>StartsWith
J.JId   |Wijzigingslogboek-ID                                         |Int64      |Is gelijk aan
C.CN    |De naam van de vergadering-serie                             |String     |Is gelijk aan,<br/>StartsWith
C.CId   |Conferentie reeks-ID                               |Int64      |Is gelijk aan
RId     |Waarnaar wordt verwezen, documenten-ID                              |Int64]    |Is gelijk aan
W       |Woorden uit papier titel en samenvatting                |String[]   |Is gelijk aan
E       |Uitgebreide metagegevens (Zie onderstaande tabel)                |String     |geen  
        


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Description               
--------|---------------------------    
DN      | Weergavenaam van het document 
S       | Bronnen - lijst van webbronnen van het document, gesorteerd op basis van statische positie
S.Ty    | Brontype (1:HTML, 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | Bron-URL
VFN     | Volledige naam van de venue - volledige naam van het logboek of de Conferentie
VSN     | Korte naam venue - korte naam zijn van het logboek of de Conferentie
V       | Volume - logboek volume
BV      | De naam van het logboek
BT      | 
PB      | Logboek Abreviations
I       | Probleem - logboek probleem
FP      | FirstPage - eerste pagina van document
LP      | LastPage - laatste pagina van document
DOI     | Digitale Object-id
CC      | Instantiedocumentcitaat contexten: lijst met waarnaar wordt verwezen, artikel-id's en de bijbehorende context in het document (bijvoorbeeld [{123: ['bruine vossen bekend voor springen waarnaar wordt verwezen in document 123', 'de vertraagde honden zijn een historische misnomer zoals wordt weergegeven in het document 123']})
IA      | Omgekeerde Abstract
IA.IndexLength| Aantal items in de index (abstract van word aantal)
IA.InvertedIndex| Overzicht van de abstracte woorden en hun bijbehorende positie in de oorspronkelijke abstract (bijvoorbeeld [{"het": [0, 15, 30]}, {'brown': [1]}, {"fox":[2]}])
