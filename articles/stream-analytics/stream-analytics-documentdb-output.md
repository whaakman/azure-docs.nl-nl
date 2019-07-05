---
title: Azure Stream Analytics-uitvoer naar Cosmos DB
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics gebruiken om op te slaan van de uitvoer naar Azure Cosmos DB voor JSON-uitvoer, voor het archiveren van gegevens en query's met lage latentie voor niet-gestructureerde JSON-gegevens.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: de5febaeecd176a8718364720132d3fa4433c57f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443630"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-uitvoer naar Azure Cosmos DB  
Stream Analytics kunt richten [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) inschakelen voor JSON-uitvoer, gegevens archiveren en lage latentie-query's voor niet-gestructureerde JSON-gegevens. In dit document staan enkele aanbevolen procedures voor het implementeren van deze configuratie.

Voor gebruikers die niet bekend met Cosmos DB bent, kijk eens [leertraject voor Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag. 

> [!Note]
> Op dit moment ondersteunt Azure Stream Analytics alleen verbinding met Azure Cosmos DB met behulp van **SQL API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics met de Azure Cosmos DB-accounts die zijn gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Basisbeginselen van het Cosmos DB als een bestemming voor uitvoer
De Azure Cosmos DB-uitvoer in Stream Analytics kunt schrijven uw stream resultaten verwerken als JSON-uitvoer in uw Cosmos DB-container (s). Stream Analytics biedt geen containers maken in uw database in plaats daarvan vereist dat u deze vooraf te maken. Dit is zodat de factureringskosten van Cosmos DB-containers worden beheerd door u en zodat u kunt afstemmen de prestaties, consistentie en capaciteit van uw containers direct met behulp van de [Cosmos DB-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> U moet 0.0.0.0 toevoegen aan de lijst met toegestane IP-adressen van uw Azure Cosmos DB-firewall.

Enkele van de Cosmos DB-container-opties worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen
Om te voldoen aan uw toepassing, kunt Azure Cosmos DB u afstemmen van de database en de containers en wisselwerking tussen consistentie, beschikbaarheid, latentie en doorvoer. Afhankelijk van welke mate van lezen van consistentie op basis van de behoeften van uw scenario lezen en schrijven latentie, dat kunt u een consistentieniveau kiezen voor uw databaseaccount. Doorvoer kan worden verbeterd door omhoog Units(RUs) aanvragen voor de container te schalen. Standaard kunnen Azure Cosmos DB ook synchrone indexering voor elke CRUD-bewerking naar de container. Dit is een andere handige optie voor het beheren van de prestaties voor schrijven/lezen in Azure Cosmos DB. Raadpleeg voor meer informatie de [wijzigen van uw database en query-consistentieniveaus](../cosmos-db/consistency-levels.md) artikel.

## <a name="upserts-from-stream-analytics"></a>Upsert-bewerking van Stream Analytics
Stream Analytics-integratie met Azure Cosmos DB kunt u invoegen of bijwerken van de records in de container op basis van een bepaald Document-ID-kolom. Dit wordt ook wel een *Upsert*.

Stream Analytics maakt gebruik van een benadering optimistische upsert waar de updates worden alleen uitgevoerd als invoegen is mislukt met een conflict met Document-ID. Deze update wordt uitgevoerd met Compatibility Level 1.0, als een PATCH, zodat deze gedeeltelijke updates aan het document, dat wil zeggen schakelt, de toevoeging van nieuwe eigenschappen of het vervangen van die een eigenschap van een bestaande stapsgewijs wordt uitgevoerd. Echter, wijzigingen in de waarden van Matrixeigenschappen in uw JSON-document, resulteren in de volledige matrix ophalen overschreven, dat wil zeggen, de matrix is niet samengevoegd. Met 1.2, upsert gedrag gewijzigd voor het invoegen of vervangen van het document. Dit is beschreven in de onderstaande sectie met Compatibility Level 1.2.

Als het binnenkomende JSON-document een bestaande id-veld heeft, dat veld automatisch als de Document-ID-kolom in Cosmos DB gebruikt wordt en alle daaropvolgende schrijfbewerkingen worden verwerkt als zodanig leidt tot een van deze situaties:
- de unieke id's leiden om in te voegen
- dubbele id's en 'Document-ID' ingesteld op 'ID' leidt tot upsert
- dubbele id's en Document-ID niet set leidt tot een fout, na het eerste document

Als u wilt opslaan <i>alle</i> documenten met inbegrip van de items met een dubbele ID, wijzig de naam van het ID-veld in de query (met het sleutelwoord AS) en laat Cosmos DB maakt het ID-veld of de ID vervangen door een andere kolom-waarde (met behulp van de AS-trefwoord of door met behulp van de instelling 'Document-ID').

## <a name="data-partitioning-in-cosmos-db"></a>Partitioneren van gegevens in Cosmos DB
Azure Cosmos DB [onbeperkte](../cosmos-db/partition-data.md) containers zijn de aanbevolen aanpak voor het partitioneren van uw gegevens, als de Azure Cosmos DB automatisch wordt geschaald partities op basis van uw werkbelasting. Bij het schrijven naar het onbeperkt aantal containers, Stream Analytics maakt gebruik van zo veel parallelle schrijvers als de vorige query stap of invoer partitieschema.
> [!NOTE]
> Op dit moment ondersteunt Azure Stream Analytics alleen onbeperkte containers met partitiesleutels op het hoogste niveau. Bijvoorbeeld, `/region` wordt ondersteund. Geneste partitiesleutels (bijvoorbeeld `/region/name`) worden niet ondersteund. 

Afhankelijk van uw keuze van partitiesleutel u ontvangt, kan dit _waarschuwing_:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Het is belangrijk om te kiezen van een partitie-sleuteleigenschap die een aantal afzonderlijke waarden heeft, en kunt u uw workload gelijkmatig verdelen over deze waarden. Als een natuurlijke artefact van partitionering, worden aanvragen met betrekking tot de dezelfde partitiesleutel worden beperkt door de maximale doorvoer van één partitie. De maximale grootte voor documenten die horen bij dezelfde partitiesleutel is bovendien beperkt tot 10GB. Een ideale partitiesleutel is een die vaak worden weergegeven als een filter in uw query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing is schaalbaar.

Een partitiesleutel is ook de grens voor transacties in DocumentDB van opgeslagen procedures en triggers. U moet de partitiesleutel kiezen zodat documenten die zich bij elkaar transacties voordoen de dezelfde waarde voor de partitiesleutel delen. Het artikel [partitionering in Cosmos DB](../cosmos-db/partitioning-overview.md) geeft meer informatie over het kiezen van een partitiesleutel.

Stream Analytics kunnen omhoog of omlaag schalen wanneer ze volledig op geen enkele manier voor vaste Azure Cosmos DB-containers. Ze hebben een limiet van 10 GB en doorvoer van 10.000 RU/s.  Als u wilt de gegevens van een vaste container migreren naar een onbeperkte container (bijvoorbeeld, één met ten minste 1000 RU/s en een partitiesleutel), die u wilt gebruiken de [hulpprogramma voor gegevensmigratie](../cosmos-db/import-data.md) of de [wijzigingenfeed bibliotheek](../cosmos-db/change-feed.md).

De mogelijkheid om te schrijven naar meerdere vaste containers wordt afgeschaft en wordt niet aanbevolen voor het uitbreiden van uw Stream Analytics-taak.

## <a name="improved-throughput-with-compatibility-level-12"></a>Verbeterde doorvoer met Compatibility Level 1.2
Met compatibiliteitsniveau 1.2, Stream Analytics ondersteunt systeemeigen integratie voor het bulksgewijs schrijven in Cosmos DB. Hierdoor kunnen schrijven effectief met Cosmos DB met maximaliseert de doorvoer en efficiënt bandbreedtebeperking aanvragen verwerken. Het mechanisme voor verbeterde schrijven zich bevindt onder een nieuwe compatibiliteitsniveau vanwege een verschil in gedrag upsert.  Voordat u 1.2 is het gedrag van de upsert invoegen of samenvoegen van het document. Met 1.2, upsert-bewerking gedrag gewijzigd voor het invoegen of vervangen van het document. 

Voordat u 1.2, gebruikt een aangepaste opgeslagen procedure voor het bulksgewijs upsert documenten per partitiesleutel in Cosmos DB, waarbij een batch wordt aangeduid als een transactie. Zelfs wanneer één record komt binnen via een tijdelijke fout (beperking), de hele batch moet opnieuw worden uitgevoerd. Deze scenario's met relatief trager zelfs redelijke beperking. Vergelijking van de volgende laat zien hoe deze taken wilt werken met 1.2.

Het volgende voorbeeld ziet twee identieke Stream Analytics-taken lezen uit dezelfde Event Hub-invoer. Beide Stream Analytics-taken zijn [volledig gepartitioneerd](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) met een passthrough-query en het schrijven naar identieke cosmos DB-containers. Metrische gegevens aan de linkerkant van de taak die is geconfigureerd met compatibiliteitsniveau 1.0 zijn en die aan de rechterkant worden geconfigureerd met 1.2. Een Cosmos DB-container partitiesleutel is een unieke GUID die afkomstig zijn van de invoer gebeurtenis.

![vergelijking van metrische gegevens over de stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Snelheid van binnenkomende gebeurtenissen in Event Hub is 2 x groter is dan de Cosmos DB-containers (20K ru's) zijn geconfigureerd voor het opnemen, zodat de beperking wordt verwacht in Cosmos DB. De taak met 1.2, is echter consistent met een hogere doorvoer (uitvoer gebeurtenissen/minuut) en met een lagere gemiddelde gebruikspercentage voor schrijven. In uw omgeving afhankelijk dit verschil van enkele meer factoren zoals keuze van de indeling voor gebeurtenissen, gebeurtenis/invoerbericht grootte, partitiesleutels, query, enzovoort.

![vergelijking van cosmos db metrische gegevens](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

Stream Analytics is met 1.2, intelligentere bij het gebruik van 100% van de beschikbare doorvoer in Cosmos DB met heel weinig resubmissions van beperking/gelden enkele beperkingen. Dit biedt een betere ervaring voor andere werkbelastingen, zoals query's uitgevoerd op de container op hetzelfde moment. Als u uitproberen hoe ASA opgeschaald met Cosmos DB wilt als een sink voor 1k en 10k berichten per seconde, hier is een [voorbeelden van azure-project](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) waarmee u dat doen.
Houd er rekening mee dat de doorvoer van Cosmos DB-uitvoer gelijk aan 1.0 en 1.1 is. Aangezien 1.2 momenteel niet de standaard is, kunt u [Stel compatibiliteitsniveau](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) voor een Stream Analytics-taak met behulp van de portal of met behulp van de [oproep voor de REST-API maken](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Er *sterk aanbevolen* Compatibility Level 1.2 in ASA gebruiken met Cosmos DB. 



## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-instellingen voor JSON-uitvoer

Het maken van Cosmos DB als uitvoer in Stream Analytics, genereert een prompt voor meer informatie, zoals hieronder wordt weergegeven. In deze sectie bevat een uitleg van de definitie van de eigenschappen.

![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Veld           | Description|
|-------------   | -------------|
|Uitvoeralias    | Een alias om te verwijzen deze uitvoer in de ASA-query.|
|Abonnement    | Kies het Azure-abonnement.|
|Account-id      | De naam of URI van de Azure Cosmos DB-account van het eindpunt.|
|Accountcode     | De gedeelde toegangssleutel voor het Azure Cosmos DB-account.|
|Database        | De naam van de Azure Cosmos DB-database.|
|Containernaam | De containernaam moet worden gebruikt. `MyContainer` een voorbeeld van geldige invoer - een container met de naam is `MyContainer` moet bestaan.  |
|Document-id     | Optioneel. De naam van de kolom in uitvoergebeurtenissen dat wordt gebruikt als de unieke sleutel bij welke invoegen of bijwerken bewerkingen moeten worden gebaseerd. Als u niets opgeeft, wordt alle gebeurtenissen ingevoegd, zonder optie update.|
