---
title: Het detecteren van gegevensbronnen in Azure Data Catalog | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u voor het detecteren van geregistreerde gegevensassets met Azure Data Catalog, met inbegrip van zoeken en filteren en met behulp van de treffers markeren mogelijkheden van de Azure Data Catalog-portal.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Het detecteren van gegevensbronnen in Azure Data Catalog
## <a name="introduction"></a>Inleiding
Azure Data Catalog is een volledig beheerde cloudservice die als een systeem van registratie en detectie van zakelijke gegevensbronnen fungeert. Met andere woorden, Data Catalog helpt mensen detecteren, begrijpen en gebruiken van gegevensbronnen en het helpt organisaties Haal meer uit hun bestaande gegevens. Nadat een gegevensbron is geregistreerd met Data Catalog, worden de metagegevens van de wordt geïndexeerd door de service, zodat u gemakkelijk zoeken kunt voor het detecteren van de gegevens die u nodig.

## <a name="searching-and-filtering"></a>Zoeken en filteren
Detectie in Data Catalog gebruikt twee primaire mechanismen: zoeken en filteren.

Zoeken is zowel intuïtief als krachtig. Standaard worden de zoektermen vergeleken met een eigenschap in de catalogus, met inbegrip van door de gebruiker gemaakte aantekeningen.

Filteren is bedoeld als aanvulling op het zoeken. U kunt specifieke kenmerken zoals deskundigen, gegevensbrontype, objecttype en labels selecteren. U kunt zien alleen overeenkomende gegevensassets en zoekresultaten in die overeenkomt met activa beperken.

Met behulp van een combinatie van zoeken en filteren kunt u snel de gegevensbronnen die zijn geregistreerd met Data Catalog voor het detecteren van gegevensbronnen u moet navigeren.

## <a name="search-syntax"></a>Zoeksyntaxis
Hoewel het zoeken met vrije tekst standaard eenvoudige en intuïtieve is, kunt u ook zoeksyntaxis van Data Catalog gebruiken voor meer controle over de zoekresultaten. Zoeken in de catalogus ondersteunt de volgende technieken:

| Techniek | Gebruiken | Voorbeeld |
| --- | --- | --- |
| Basic zoeken |Basic zoeken die gebruikmaakt van een of meer zoektermen. De resultaten zijn elementen die overeenkomen met een eigenschap met een of meer van de opgegeven voorwaarden. |`sales data` |
| Bereik van eigenschap definiëren |Alleen gegevensbronnen waarbij de zoekterm met de opgegeven eigenschap overeenkomt retourneren. |`name:finance` |
| Booleaanse operators |Verbreed of Verfijn een zoekopdracht met Booleaanse bewerkingen. |`finance NOT corporate` |
| Groeperen met haakjes |Gebruik haakjes en groepeer delen van de query voor logische isolatie, met name in combinatie met Booleaanse operators. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Vergelijkingsoperators |Gebruik andere vergelijkingen dan gelijkheid voor eigenschappen die de gegevenstypen Numeriek en datum hebben. |`modifiedTime > "11/05/2014"` |

Zie voor meer informatie over Data Catalog search de [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) artikel.

## <a name="hit-highlighting"></a>Markeren
Wanneer u zoekresultaten, elk weergegeven eigenschappen die overeenkomen met de opgegeven zoektermen (zoals de naam van de asset gegevens, beschrijving en labels bekijkt) worden gemarkeerd om deze is gemakkelijker te identificeren waarom een bepaalde gegevensasset geretourneerd door een bepaalde zoekactie.

> [!NOTE]
> Als u wilt uitschakelen treffers markeren, gebruiken de **Markeer** switch in de portal voor Data Catalog.
>
>

Wanneer u de zoekresultaten weergeeft, niet altijd worden duidelijk waarom een gegevensasset opgenomen, is zelfs met treffers markering is ingeschakeld. Omdat alle eigenschappen standaard worden doorzocht, een gegevensasset mogelijk geretourneerde vanwege een overeenkomst voor een eigenschap op kolomniveau. En omdat meerdere gebruikers kunnen aantekeningen toevoegen aan geregistreerde gegevensassets met hun eigen labels en beschrijvingen, niet alle metagegevens kan worden weergegeven in de lijst met zoekresultaten.

In de standaard tegelweergave, elke tegel in de zoekresultaten weergegeven bevat een **weergave zoekterm overeenkomt met** pictogram, zodat u kunt snel het aantal overeenkomsten en de locatie, en om door te gaan als u wilt weergeven.

 ![Markeren en zoeken komt overeen met in de portal voor Azure Data Catalog](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Samenvatting
Omdat u een gegevensbron met Data Catalog structurele en beschrijvende metagegevens uit de gegevensbron wordt gekopieerd naar de catalogusservice, wordt de gegevensbron eenvoudiger te detecteren en begrijpen. Nadat u een gegevensbron hebt geregistreerd, kunt u ontdekken met behulp van het filteren en zoeken van in de portal Data Catalog.

## <a name="next-steps"></a>Volgende stappen
* Zie voor stapsgewijze informatie over het detecteren van gegevensbronnen [aan de slag met Azure Data Catalog](data-catalog-get-started.md).
