---
title: Azure Stream Analytics-uitvoer naar Cosmos DB
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics gebruiken om op te slaan van de uitvoer naar Azure Cosmos DB voor JSON-uitvoer, voor het archiveren van gegevens en query's met lage latentie voor niet-gestructureerde JSON-gegevens.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: 9bdb012db2e7502d765fd342a636591bbbcb2c6c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311735"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-uitvoer naar Azure Cosmos DB  
Stream Analytics kunt richten [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) inschakelen voor JSON-uitvoer, gegevens archiveren en lage latentie-query's voor niet-gestructureerde JSON-gegevens. In dit document staan enkele aanbevolen procedures voor het implementeren van deze configuratie.

Voor gebruikers die niet bekend met Cosmos DB bent, kijk eens [leertraject voor Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag. 

> [!Note]
> Op dit moment ondersteunt Azure Stream Analytics alleen verbinding met Azure Cosmos DB met behulp van **SQL API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics met de Azure Cosmos DB-accounts die zijn gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Basisbeginselen van het Cosmos DB als een bestemming voor uitvoer
De Azure Cosmos DB-uitvoer in Stream Analytics kunt u uw stream resultaten verwerken als JSON-uitvoer in uw Cosmos DB-verzameling(en) schrijven. Stream Analytics biedt geen verzamelingen maken in uw database in plaats daarvan vereist dat u deze vooraf te maken. Dit is zodat de factureringskosten van Cosmos DB-verzamelingen door u worden beheerd en zodat u kunt afstemmen de prestaties, consistentie en capaciteit van de verzamelingen die rechtstreeks met behulp van de [Cosmos DB-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> U moet 0.0.0.0 toevoegen aan de lijst met toegestane IP-adressen van u Azure Cosmos DB-firewall.

Enkele van de opties voor het verzamelen van Cosmos DB worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen
Om te voldoen aan uw toepassing, kunt Azure Cosmos DB u fijn afstemmen van de database en de verzamelingen en wisselwerking tussen consistentie, beschikbaarheid en latentie. Afhankelijk van welke mate van lezen van consistentie op basis van de behoeften van uw scenario lezen en schrijven latentie, dat kunt u een consistentieniveau kiezen voor uw databaseaccount. Standaard kunnen Azure Cosmos DB ook synchrone indexering voor elke bewerking CRUD aan uw verzameling. Dit is een andere handige optie voor het beheren van de prestaties voor schrijven/lezen in Azure Cosmos DB. Raadpleeg voor meer informatie de [wijzigen van uw database en query-consistentieniveaus](../cosmos-db/consistency-levels.md) artikel.

## <a name="upserts-from-stream-analytics"></a>Upsert-bewerking van Stream Analytics
Stream Analytics-integratie met Azure Cosmos DB kunt u invoegen of bijwerken van de records in de verzameling op basis van een bepaald Document-ID-kolom. Dit wordt ook wel een *Upsert*.

Stream Analytics maakt gebruik van een benadering optimistische upsert waar de updates worden alleen uitgevoerd als invoegen is mislukt met een conflict met Document-ID. Deze update wordt uitgevoerd als een PATCH, zodat deze gedeeltelijke updates aan het document, dat wil zeggen schakelt, de toevoeging van nieuwe eigenschappen of het vervangen van die een eigenschap van een bestaande stapsgewijs wordt uitgevoerd. Echter, wijzigingen in de waarden van Matrixeigenschappen in uw JSON-document, resulteren in de volledige matrix ophalen overschreven, dat wil zeggen, de matrix is niet samengevoegd.

Als het binnenkomende JSON-document een bestaande id-veld heeft, dat veld automatisch als de Document-ID-kolom in Cosmos DB gebruikt wordt en alle daaropvolgende schrijfbewerkingen worden verwerkt als zodanig leidt tot een van deze situaties:
- de unieke id's leiden om in te voegen
- dubbele id's en 'Document-ID' ingesteld op 'ID' leidt tot upsert
- dubbele id's en Document-ID niet set leidt tot een fout, na het eerste document

Als u wilt opslaan <i>alle</i> documenten met inbegrip van de items met een dubbele ID, wijzig de naam van het ID-veld in de query (met het sleutelwoord AS) en laat Cosmos DB maakt het ID-veld of de ID vervangen door een andere kolom-waarde (met behulp van de AS-trefwoord of door met behulp van de instelling 'Document-ID').

## <a name="data-partitioning-in-cosmos-db"></a>Partitioneren van gegevens in Cosmos DB
Azure Cosmos DB [onbeperkte](../cosmos-db/partition-data.md) zijn de aanbevolen aanpak voor uw gegevens automatisch partitioneren als Azure Cosmos DB kan worden geschaald op basis van uw werkbelasting partities. Bij het schrijven naar het onbeperkt aantal containers, Stream Analytics maakt gebruik van zo veel parallelle schrijvers als de vorige querystap of invoer partitieschema.
> [!Note]
> Op dit moment ondersteunt Azure Stream Analytics alleen onbeperkte verzamelingen met partitiesleutels op het hoogste niveau. Bijvoorbeeld, `/region` wordt ondersteund. Geneste partitiesleutels (bijvoorbeeld `/region/name`) worden niet ondersteund. 

Stream Analytics kunnen omhoog of omlaag schalen wanneer ze volledig op geen enkele manier voor vaste Azure Cosmos DB-verzamelingen. Ze hebben een limiet van 10 GB en doorvoer van 10.000 RU/s.  Als u wilt de gegevens van een vaste container migreren naar een onbeperkte container (bijvoorbeeld, één met ten minste 1000 RU/s en een partitiesleutel), die u wilt gebruiken de [hulpprogramma voor gegevensmigratie](../cosmos-db/import-data.md) of de [wijzigingenfeed bibliotheek](../cosmos-db/change-feed.md).

Schrijven naar meerdere vaste containers wordt afgeschaft en is niet de aanbevolen aanpak voor het schalen van uw Stream Analytics-taak. Het artikel [partitioneren en schalen in Cosmos DB](../cosmos-db/sql-api-partition-data.md) biedt meer details.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-instellingen voor JSON-uitvoer
Het maken van Cosmos DB als uitvoer in Stream Analytics, genereert een prompt voor meer informatie, zoals hieronder wordt weergegeven. In deze sectie bevat een uitleg van de definitie van de eigenschappen.


![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Veld           | Beschrijving 
-------------   | -------------
Uitvoeralias    | Een alias om te verwijzen deze uitvoer in de ASA-query   
Accountnaam    | De naam of het eindpunt-URI van de Azure Cosmos DB-account 
Accountsleutel     | De gedeelde toegangssleutel voor het Azure Cosmos DB-account
Database        | De naam van de Azure Cosmos DB-database
Verzamelingnaam | De naam van de verzameling voor de verzameling moet worden gebruikt. `MyCollection` een voorbeeld van geldige invoer - één verzameling met de naam is `MyCollection` moet bestaan.  
Document-id     | Optioneel. De naam van de kolom in uitvoergebeurtenissen dat wordt gebruikt als de unieke sleutel bij welke invoegen of bijwerken bewerkingen moeten worden gebaseerd. Als u niets opgeeft, wordt alle gebeurtenissen ingevoegd, zonder optie update.
