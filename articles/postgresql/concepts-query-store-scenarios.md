---
title: Query Store-gebruiksscenario's in Azure Database for PostgreSQL
description: Dit artikel beschrijft enkele scenario's voor de Query Store in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 3cd3ebf86221db1531bd19e94bb072b4559d07d6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395590"
---
# <a name="usage-scenarios-for-query-store"></a>Gebruiksscenario's voor Query Store

**Is van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

> [!IMPORTANT]
> De functie voor Query Store is in openbare Preview in een beperkt aantal regio's.

U kunt Query Store in een groot aantal scenario's waarin bij te houden en beheren van voorspelbare werkbelasting prestaties is van essentieel belang. Houd rekening met de volgende voorbeelden: 
- Identificeren van en het afstemmen van de bovenste dure query 's 
- A / B-tests 
- Prestaties stabiel te houden tijdens upgrades 
- Identificeren van en het verbeteren van de ad-hoc workloads 

## <a name="identify-and-tune-expensive-queries"></a>Identificeren en dure query's af te stemmen 

### <a name="identify-longest-running-queries"></a>Langste uitvoeren van query's identificeren 
Gebruik de [Query Performance Insight](concepts-query-performance-insight.md) weergave in de Azure portal de langste uitvoeren van query's snel identificeren. Deze query's vaak meestal een aanzienlijke hoeveelheid resources gebruiken. Optimaliseren van uw langst lopende vragen, kan de prestaties verbeteren door het vrijmaken van resources voor gebruik door andere query's die worden uitgevoerd op uw systeem. 

### <a name="target-queries-with-performance-deltas"></a>Doel-query's waarvan prestaties delta 's 
Query Store worden de gegevens van segmenten in tijdvensters, zodat u kunt de prestaties van query's gedurende een periode bijhouden. Hiermee kunt u identificeren precies welke query's leveren tot een toename van de totale tijd besteed aan het. Als gevolg hiervan kunt u doen gerichte oplossen van problemen met uw workload.

### <a name="tuning-expensive-queries"></a>Afstemming van dure query 's 
De actie die u bij het bepalen van een query met niet-optimale prestaties zijn afhankelijk van de aard van het probleem: 
- Gebruik [aanbevelingen voor prestaties](concepts-performance-recommendations.md) om te bepalen of er een voorgestelde indexen. Zo ja, de index maken en vervolgens Query Store gebruiken om te evalueren van prestaties van query's na het maken van de index. 
- Zorg ervoor dat de statistieken bijgewerkt voor de onderliggende tabellen die worden gebruikt door de query zijn.
- Houd rekening met het herschrijven van dure query's. Bijvoorbeeld, profiteren van queryparameterisering en gebruik van dynamic SQL beperken. Optimale logica implementeren bij het lezen van gegevens, zoals het toepassen van de gegevens filteren op database, gaf niet aan de kant van de toepassing. 


## <a name="ab-testing"></a>A / B-tests 
Gebruik Query Store kunnen werkbelasting prestaties vóór en na de wijziging van een toepassing die u van plan bent om te introduceren. Voorbeelden van scenario's voor het gebruik van de Query Store te beoordelen wat de impact van de omgeving of de toepassing wijzigen op de prestaties van de workload: 
- Een nieuwe versie van een toepassing geïmplementeerd. 
- Aanvullende bronnen toevoegen aan de server. 
- Ontbrekende indexen op tabellen waarnaar wordt verwezen door dure query's te maken. 
 
In elk van deze scenario's kunt toepassing de volgende werkstroom: 
1. Uw workload uitvoeren met Query Store vóór de geplande wijziging voor het genereren van een basislijn van prestaties. 
2. Toepassing wijziging(en) gecontroleerde momenteel in de tijd van toepassing. 
3. De werkbelasting lang genoeg actief voor het genereren van afbeelding van de prestaties van het systeem na de wijziging blijven. 
4. Vergelijk de resultaten van vóór en na de wijziging. 
5. Bepaal of het wijzigen of terugdraaien. 


## <a name="identify-and-improve-ad-hoc-workloads"></a>Identificeren en ad hoc-workloads te verbeteren 
Sommige werkbelastingen geen dominante query's die u afstemmen kunt om algehele prestaties te verbeteren. Deze werkbelastingen worden doorgaans gekenmerkt met een relatief groot aantal unieke query's, elk van hen een gedeelte van het systeemresources verbruikt. Elke unieke query wordt uitgevoerd niet regelmatig worden, dus afzonderlijk dat hun gebruik van de runtime is niet kritiek. Aan de andere kant, gezien het feit dat de toepassing nieuwe query's voortdurend genereert, is een aanzienlijk deel van systeemresources die besteed aan query-compilatie niet optimaal is. Deze situatie gebeurt meestal, als uw toepassing query's genereert (in plaats van met opgeslagen procedures of geparameteriseerde query's) of als er wordt gebruikgemaakt van object-relationele gegevens frameworks die query's standaard genereren. 
 
Als u controle over de code van de toepassing, kunt u overwegen de Gegevenstoegangslaag voor het gebruik van opgeslagen procedures of geparameteriseerde query's te herschrijven. Maar kan deze situatie worden ook verbeterd zonder wijzigingen in de toepassing door af te dwingen queryparameterisering voor de gehele database (alle query's) of voor de sjablonen voor afzonderlijke query's met dezelfde queryhash. 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de [aanbevolen procedures voor het gebruik van de Query Store](concepts-query-store-best-practices.md)