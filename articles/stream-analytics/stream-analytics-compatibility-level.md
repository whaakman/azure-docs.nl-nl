---
title: Informatie over het compatibiliteitsniveau voor Azure Stream Analytics-taken
description: Informatie over het instellen van een compatibiliteitsniveau voor een Azure Stream Analytics-taak en belangrijke wijzigingen in de meest recente compatibiliteitsniveau
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 5/2/2019
ms.openlocfilehash: c1b2875e6899d2301a4c4b564882214dc7bc4981
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205464"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Compatibiliteitsniveau voor Azure Stream Analytics-taken

Dit artikel beschrijft de optie voor toepassingscompatibiliteit niveau in Azure Stream Analytics. Stream Analytics is een beheerde service, met de reguliere functie-updates en verbeterde prestaties. De meeste van de service runtimes updates worden automatisch beschikbaar gemaakt voor eindgebruikers. 

Evenwel sommige nieuwe functies in de service kan leiden tot een belangrijke wijziging, zoals een wijziging in het gedrag van een bestaande taak of een wijziging in de manier waarop gegevens wordt gebruikt bij het uitvoeren van taken. U kunt uw bestaande Stream Analytics-taken die worden uitgevoerd zonder ingrijpende wijzigingen door te laten het compatibiliteitsniveau verlaagde instellen. Wanneer u gereed voor de meest recente gedrag voor runtime bent, u kunt zich aanmelden door in te dienen het compatibiliteitsniveau. 

## <a name="choose-a-compatibility-level"></a>Kies een compatibiliteitsniveau

Compatibiliteitsniveau bepaalt het runtimegedrag van een stream analytics-taak. 

Azure Stream Analytics biedt momenteel ondersteuning voor drie compatibiliteitsniveaus:

* 1.0 - gedrag van het vorige
* 1.1 - standaardgedrag
* 1.2 (preview) - nieuwste gedrag met de meest recente verbeteringen in de evaluatie

Het oorspronkelijke 1.0 compatibiliteitsniveau is ingevoerd bij algemene beschikbaarheid van Azure Stream Analytics jaren geleden.

Wanneer u een nieuwe Stream Analytics-taak maakt, is het een aanbevolen procedure om deze te maken met behulp van het compatibiliteitsniveau van de meest recente. Start uw ontwerp taak vertrouwt op de meest recente gedrag, om te voorkomen dat later toegevoegd wijzigen en de complexiteit.

## <a name="set-the-compatibility-level"></a>Stel het compatibiliteitsniveau

U kunt het compatibiliteitsniveau voor een Stream Analytics-taak kunt instellen in Azure portal of met behulp van de [oproep voor de REST-API maken](/rest/api/streamanalytics/stream-analytics-job).

Bijwerken van het compatibiliteitsniveau van de taak in Azure portal:

1. Gebruik de [Azure-portal](https://portal.azure.com) om te zoeken naar uw Stream Analytics-taak.
2. **Stop** de taak voor het bijwerken van het compatibiliteitsniveau. U kunt het compatibiliteitsniveau niet bijwerken als de taak actief is is.
3. Onder de **configureren** kop, selecteer **compatibiliteitsniveau**.
4. Kies de gewenste waarde voor de compatibiliteit-niveau.
5. Selecteer **opslaan** aan de onderkant van de pagina.

![Compatibiliteitsniveau van Stream Analytics in Azure portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

Wanneer u het compatibiliteitsniveau bijwerkt, wordt de taak met de syntaxis die overeenkomt met het geselecteerde compatibiliteitsniveau gevalideerd door de T-SQL-compiler.

## <a name="compatibility-level-12-preview"></a>Compatibiliteitsniveau 1.2 (Preview)

De volgende belangrijke wijzigingen zijn geïntroduceerd in compatibiliteitsniveau 1.2:

### <a name="geospatial-functions"></a>Georuimtelijke functies

**Vorige niveaus:** Azure Stream Analytics gebruikt Geografie berekeningen.

**1.2 niveau:** Azure Stream Analytics kunt u voor het berekenen van geometrische verwachte geo-coördinaten. Er is geen wijziging in de handtekening van de georuimtelijke functies. Er is echter hun semantiek wijkt licht af, zodat nauwkeurigere berekening dan voordat.

Azure Stream Analytics biedt ondersteuning voor gegevens van verwijzing in georuimtelijke indexering. Referentiegegevens met georuimtelijke elementen kunnen worden geïndexeerd voor een snellere join-berekeningen.

De bijgewerkte georuimtelijke functies doen om de volledige expressiviteit van invoer (WELL-KNOWN Well Known Text) georuimtelijke indeling. U kunt andere georuimtelijke-onderdelen die eerder werden niet ondersteund met GeoJson opgeven.

Zie voor meer informatie, [Updates voor georuimtelijke functies in Azure Stream Analytics-Cloud en IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Parallelle queryuitvoering voor invoerbronnen met meerdere partities

**Vorige niveaus:** Azure Stream Analytics-query's vereist het gebruik van PARTITION BY-component voor de verwerking van query's parallel over meerdere partities van de invoerbron.

**1.2 niveau:** Als querylogica kan worden geparallelliseerd over meerdere partities van de invoerbron, wordt Azure Stream Analytics maakt u afzonderlijke query-exemplaren en berekeningen in parallel wordt uitgevoerd.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>Systeemeigen bulksgewijs API-integratie met CosmosDB-uitvoer

**Vorige niveaus:** Is het gedrag van de upsert *invoegen of samenvoegen*.

**1.2 niveau:** Systeemeigen bulksgewijs API-integratie met CosmosDB-uitvoer maximaliseert de doorvoer en efficiënte wijze verwerkt Aanvraagbeperkingen. Zie voor meer informatie, [de Azure Stream Analytics-uitvoer naar Azure Cosmos DB-pagina](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-documentdb-output#improved-throughput-with-compatibility-level-12).

Het gedrag van upsert *invoegen of vervangen*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>DateTimeOffset bij het schrijven naar de SQL-uitvoer

**Vorige niveaus:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) typen zijn ingesteld op UTC.

**1.2 niveau:** DateTimeOffset wordt niet meer aangepast.

### <a name="strict-validation-of-prefix-of-functions"></a>Strikte validatie van voorvoegsel van functies

**Vorige niveaus:** Er is geen strikte validatie van de functie voorvoegsels.

**1.2 niveau:** Azure Stream Analytics is een strikte validatie van de functie voorvoegsels. Een voorvoegsel toe te voegen aan een ingebouwde functie veroorzaakt een fout. Bijvoorbeeld,`myprefix.ABS(…)` wordt niet ondersteund.

Resulteren in fout is ook een voorvoegsel voor ingebouwde statistische functies toe te voegen. Bijvoorbeeld, `myprefix.SUM(…)` wordt niet ondersteund.

Met behulp van het voorvoegsel 'systeem' voor de gebruiker gedefinieerde functies resultaten in de fout.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>Matrix- en als de eigenschappen van de sleutel in Cosmos DB-uitvoeradapter weigeren

**Vorige niveaus:** Matrix- en -typen worden ondersteund als een sleuteleigenschap.

**1.2 niveau:** Matrix- en -typen worden niet meer ondersteund als een sleuteleigenschap.

## <a name="compatibility-level-11"></a>Compatibiliteitsniveau 1.1

De volgende belangrijke wijzigingen zijn geïntroduceerd in compatibiliteitsniveau 1.1 van:

### <a name="service-bus-xml-format"></a>Service Bus-XML-indeling

**1.0 niveau:** Azure Stream Analytics gebruikt DataContractSerializer, zodat de inhoud van het bericht XML-tags opgenomen. Bijvoorbeeld:

`@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId":"1", "Temperature":64\}\u0001`

**1.1 niveau:** De inhoud van het bericht bevat de stream rechtstreeks met geen andere labels. Bijvoorbeeld: `{ "SensorId":"1", "Temperature":64}`

### <a name="persisting-case-sensitivity-for-field-names"></a>Permanente hoofdlettergevoeligheid voor veldnamen

**1.0 niveau:** Veldnamen zijn gewijzigd in kleine letters wanneer verwerkt door de Azure Stream Analytics-engine.

**1.1 niveau:** hoofdlettergevoeligheid voor veldnamen worden bewaard wanneer ze worden verwerkt door de Azure Stream Analytics-engine.

> [!NOTE]
> Permanente hoofdlettergevoeligheid is nog niet beschikbaar voor Stream Analytics-taken die worden gehost met behulp van Edge-omgeving. Als gevolg hiervan, worden alle veldnamen geconverteerd naar kleine letters, als de taak voor edge-apparaten wordt gehost.

### <a name="floatnandeserializationdisabled"></a>FloatNaNDeserializationDisabled

**1.0 niveau:** CREATE TABLE-opdracht heeft geen filter voor gebeurtenissen met NaN (Not a Number. Bijvoorbeeld oneindig, -oneindig) in een FLOAT-kolom typt, omdat deze buiten het bereik beschreven voor deze getallen zijn.

**1.1 niveau:** CREATE TABLE kunt u een sterke schema opgeven. De Stream Analytics-engine valideert dat de gegevens aan dit schema voldoen. De opdracht kunt u gebeurtenissen met NaN waarden filteren met dit model.

### <a name="disable-automatic-upcast-for-datetime-strings-in-json"></a>Uitschakelen van automatische upcast voor datum/tijd-tekenreeksen in JSON

**1.0 niveau:** De JSON-parser zou automatisch opgewaardeerd tekenreekswaarden met datum/tijdzone-informatie naar DateTime-type en deze vervolgens converteren naar UTC. Dit gedrag leidde tot verlies van informatie over de tijdzone.

**1.1 niveau:** Er is niet meer automatisch opgewaardeerd van tekenreekswaarden met datum/tijdzone-informatie naar DateTime-type. Als gevolg hiervan, wordt informatie over de tijdzone bewaard.

## <a name="next-steps"></a>Volgende stappen

* [Azure Stream Analytics-invoer oplossen](stream-analytics-troubleshoot-input.md)
* [Stream Analytics-Resource health](stream-analytics-resource-health.md)
