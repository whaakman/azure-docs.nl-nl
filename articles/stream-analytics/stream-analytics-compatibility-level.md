---
title: Informatie over het compatibiliteitsniveau voor Azure Stream Analytics-taken
description: Informatie over het instellen van een compatibiliteitsniveau voor een Azure Stream Analytics-taak en belangrijke wijzigingen in de meest recente compatibiliteitsniveau
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 6fb93152263d253de983b17d25f02f4c68a172fd
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361402"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Compatibiliteitsniveau voor Azure Stream Analytics-taken
 
Compatibiliteitsniveau verwijst naar de release-specifieke gedrag van een Azure Stream Analytics-service. Azure Stream Analytics is een beheerde service, met de reguliere functie-updates en verbeterde prestaties. Meestal worden updates automatisch beschikbaar gemaakt voor eindgebruikers. Enkele nieuwe functies kunnen echter introduceert belangrijke wijzigingen dergelijke als-wijziging in het gedrag van een bestaande taak, wijziging van de processen gebruiken van gegevens van deze taken enzovoort. Een compatibiliteitsniveau wordt gebruikt voor belangrijke wijzigingen die zijn geïntroduceerd in Stream Analytics. Belangrijke wijzigingen zijn altijd met een nieuwe compatibiliteitsniveau geïntroduceerd. 

Compatibiliteitsniveau zorgt ervoor dat bestaande taken worden uitgevoerd zonder een storing. Wanneer u een nieuwe Stream Analytics-taak maakt, is het een aanbevolen procedure om deze te maken met behulp van het compatibiliteitsniveau van de meest recente. 
 
## <a name="set-a-compatibility-level"></a>Stel een compatibiliteitsniveau 

Compatibiliteitsniveau bepaalt het runtimegedrag van een stream analytics-taak. U kunt het compatibiliteitsniveau voor een Stream Analytics-taak kunt instellen met behulp van de portal of met behulp van de [oproep voor de REST-API maken](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics ondersteunt momenteel twee compatibiliteit niveaus-"1.0" en '1.1'. Het compatibiliteitsniveau is standaard ingesteld op "1.0", dat werd geïntroduceerd bij algemene beschikbaarheid van Azure Stream Analytics. Voor het bijwerken van de standaardwaarde, gaat u naar uw bestaande Stream Analytics-taak > Selecteer de **compatibiliteitsniveau** optie **configureren** sectie en wijzig de waarde. 

Zorg ervoor dat u de taak stoppen voordat u het compatibiliteitsniveau bijwerkt. U kunt het compatibiliteitsniveau niet bijwerken als de taak actief is is. 

![Compatibiliteitsniveau van Stream Analytics in Azure portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Wanneer u het compatibiliteitsniveau bijwerkt, wordt de taak met de syntaxis die overeenkomt met het geselecteerde compatibiliteitsniveau gevalideerd door de T-SQL-compiler. 

## <a name="major-changes-in-the-latest-compatibility-level-12"></a>Belangrijke wijzigingen in de meest recente compatibiliteitsniveau (1.2)

De volgende belangrijke wijzigingen zijn geïntroduceerd in compatibiliteitsniveau 1.2:

### <a name="geospatial-functions"></a>Georuimtelijke functies 

**Vorige versies:** Azure Stream Analytics gebruikt Geografie berekeningen.

**huidige versie:** Azure Stream Analytics kunt u voor het berekenen van geometrische verwachte geo-coördinaten. Er is geen wijziging in de handtekening van de georuimtelijke functies. Er is echter hun semantiek wijkt licht af, zodat nauwkeurigere berekening dan voordat.

Azure Stream Analytics biedt ondersteuning voor gegevens van verwijzing in georuimtelijke indexering. Referentiegegevens met georuimtelijke elementen kunnen worden geïndexeerd voor een snellere join-berekeningen.

De bijgewerkte georuimtelijke functies doen om de volledige expressiviteit van invoer (WELL-KNOWN Well Known Text) georuimtelijke indeling. U kunt andere georuimtelijke-onderdelen die eerder werden niet ondersteund met GeoJson opgeven.

Zie voor meer informatie, [Updates voor georuimtelijke functies in Azure Stream Analytics-Cloud en IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Parallelle queryuitvoering voor invoerbronnen met meerdere partities 

**Vorige versies:** Azure Stream Analytics-query's vereist het gebruik van PARTITION BY-component voor de verwerking van query's parallel over meerdere partities van de invoerbron.

**huidige versie:** Als querylogica kan worden geparallelliseerd over meerdere partities van de invoerbron, wordt Azure Stream Analytics maakt u afzonderlijke query-exemplaren en berekeningen in parallel wordt uitgevoerd.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>Systeemeigen bulksgewijs API-integratie met CosmosDB-uitvoer

**Vorige versies:** Is het gedrag van de upsert *invoegen of samenvoegen*.

**huidige versie:** Systeemeigen bulksgewijs API-integratie met CosmosDB-uitvoer maximaliseert de doorvoer en efficiënte wijze verwerkt Aanvraagbeperkingen.

Het gedrag van upsert *invoegen of vervangen*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>DateTimeOffset bij het schrijven naar de SQL-uitvoer

**Vorige versies:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) typen zijn ingesteld op UTC.

**huidige versie:** DateTimeOffset wordt niet meer aangepast.

### <a name="strict-validation-of-prefix-of-functions"></a>Strikte validatie van voorvoegsel van functies

**Vorige versies:** Er is geen strikte validatie van de functie voorvoegsels.

**huidige versie:** Azure Stream Analytics is een strikte validatie van de functie voorvoegsels. Een voorvoegsel toe te voegen aan een ingebouwde functie veroorzaakt een fout. Bijvoorbeeld,`myprefix.ABS(…)` wordt niet ondersteund.

Resulteren in fout is ook een voorvoegsel voor ingebouwde statistische functies toe te voegen. Bijvoorbeeld, `myprefix.SUM(…)` wordt niet ondersteund.

Met behulp van het voorvoegsel 'systeem' voor de gebruiker gedefinieerde functies resultaten in de fout.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>Matrix- en als de eigenschappen van de sleutel in Cosmos DB-uitvoeradapter weigeren

**Vorige versies:** Matrix- en -typen worden ondersteund als een sleuteleigenschap.

**huidige versie:** Matrix- en -typen worden niet meer ondersteund als een sleuteleigenschap.


## <a name="next-steps"></a>Volgende stappen
* [Azure Stream Analytics-invoer oplossen](stream-analytics-troubleshoot-input.md)
* [Stream Analytics-Resource health](stream-analytics-resource-health.md)
