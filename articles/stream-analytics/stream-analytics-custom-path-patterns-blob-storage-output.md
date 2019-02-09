---
title: Azure Stream Analytics aangepaste blob partitioneren van uitvoer
description: Dit artikel beschrijft de aangepaste patronen voor datum/tijd-pad en de aangepaste veld of de kenmerken van de functies voor blob storage-uitvoer van Azure Stream Analytics-taken.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/07/2019
ms.custom: seodec18
ms.openlocfilehash: 9cdf99884845a9cb83ac26723c3ea0e7a779ebff
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55982672"
---
# <a name="azure-stream-analytics-custom-blob-output-partitioning"></a>Azure Stream Analytics aangepaste blob partitioneren van uitvoer

Azure Stream Analytics biedt ondersteuning voor aangepaste blob uitvoer partitioneren met aangepaste velden of kenmerken en aangepaste datum-/ padpatronen. 

## <a name="custom-field-or-attributes"></a>Aangepast veld of kenmerken

Aangepast veld of invoerkenmerken aan de downstream-gegevensverwerking en rapportage van werkstromen doordat meer controle over de uitvoer.

### <a name="partition-key-options"></a>De opties voor partitie

De partitiesleutel of kolomnaam, die wordt gebruikt voor het partitioneren van invoergegevens mag alfanumerieke tekens met afbreekstreepjes, onderstrepingstekens en spaties bevatten. Het is niet mogelijk met gebruik van geneste velden als een partitiesleutel, tenzij gebruikt in combinatie met aliassen.

### <a name="example"></a>Voorbeeld

Stel dat een taak duurt invoergegevens van live gebruikerssessies die zijn verbonden met een externe videogame-service waar opgenomen gegevens bevat voor een kolom **client_id** voor het identificeren van de sessies. Voor het partitioneren van de gegevens door **client_id**, stelt u het veld Padpatroon Blob om op te nemen van een token partitie **{client_id}** in blob-uitvoereigenschappen bij het maken van een taak. Als gegevens met verschillende **client_id** waarden langs de Stream Analytics-taak, wordt de uitvoergegevens die worden opgeslagen in afzonderlijke mappen op basis van een enkel **client_id** waarde per map.

![Padpatroon met client-id](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-path-pattern-client-id.png)

Op dezelfde manier als de Taakinvoer is sensorgegevens uit miljoenen sensoren waar elke sensor heeft een **sensor_id**, zou het Padpatroon **{sensor_id}** voor het partitioneren van elke sensorgegevens naar verschillende mappen.  


Met de REST-API, de sectie uitvoer van een JSON bestand dat wordt gebruikt voor deze aanvraag ziet er als volgt uit:  

![Uitvoer van de REST-API](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-rest-output.png)

Zodra de taak uitgevoerd wordt, de *clients* container kan er als volgt uitzien:  

![Clients container](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-clients-container.png)

Elke map kan meerdere blobs waarbij elke blob een of meer records bevat bevatten. In het bovenstaande voorbeeld moet u er één blob is in een map met de naam '06000000' met de volgende inhoud:

![Inhoud van een BLOB](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-contents.png)

U ziet dat elke record in de blob heeft een **client_id** die overeenkomt met de map kolomnaam omdat de kolom die wordt gebruikt voor het partitioneren van de uitvoer in het uitvoerpad is **client_id**.

### <a name="limitations"></a>Beperkingen

1. Slechts één aangepaste partitiesleutel is toegestaan in de eigenschap Padpatroon blob uitvoer. Alle van de volgende Padpatronen zijn geldig:

   * cluster1/{date}/{aFieldInMyData}  
   * cluster1/{time}/{aFieldInMyData}  
   * cluster1/{aFieldInMyData}  
   * cluster1/{date}/{time}/{aFieldInMyData}  

2. Partitiesleutels zijn niet hoofdlettergevoelig, dus partitiesleutels, zoals 'John' en 'john' gelijk zijn. Expressies kunnen niet ook worden gebruikt als partitiesleutels. Bijvoorbeeld, **{columnA + columnB}** werkt niet.  

3. Wanneer een invoerstroom uit records met de kardinaliteit van de sleutel van een partitie onder 8000 bestaat, worden de records wordt toegevoegd aan bestaande blobs en maken alleen nieuwe blobs wanneer dat nodig. Als de kardinaliteit van de ligt boven de 8000 er is geen garantie dat bestaande blobs worden geschreven naar en nieuwe blobs wordt niet worden gemaakt voor een willekeurig aantal records met dezelfde partitiesleutel.  

## <a name="custom-datetime-path-patterns"></a>Aangepaste datum/tijd-padpatronen

Aangepaste datum/tijd-padpatronen kunnen u opgeven de indeling van een uitvoer die overeenstemt met Hive-Streaming-conventies, waardoor Azure Stream Analytics om gegevens te verzenden naar Azure HDInsight en Azure Databricks voor downstream verwerken. Aangepaste datum/tijd-padpatronen eenvoudig worden geïmplementeerd met behulp van de `datetime` sleutelwoord in het veld pad naar het voorvoegsel van de uitvoer, samen met de indelingsopgave blob. Bijvoorbeeld `{datetime:yyyy}`.

### <a name="supported-tokens"></a>Ondersteunde tokens

De volgende indeling aanduiding tokens kunnen zelfstandig of in combinatie worden gebruikt om aangepaste datum/tijd-notatie:

|Indelingsopgave   |Description   |De resultaten in het voorbeeld van de tijd 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|Een getal van vier cijfers van het jaar|2018|
|{datetime:MM}|Maand vanaf 01 en 12|01|
|{datetime:M}|Maand van 1 tot en met 12|1|
|{datetime:dd}|Dag vanaf 01 tot en met 31|02|
|{datetime:d}|Dag van 1 tot en met 12|2|
|{datetime:HH}|Met behulp van de 24-uurs tijdnotatie, tussen 00 en 23 uur|10|
|{datetime:mm}|Minuten van 00 24 uur per dag|06|
|{datetime:m}|Minuten van 0 tot en met 24|6|
|{datetime:ss}|Seconden tussen 00 en 60|08|

Als u niet gebruiken van aangepaste datum/tijd-patronen wilt, u kunt toevoegen van de {date} en/of {time}-token voor het voorvoegsel voor het pad voor het genereren van een vervolgkeuzelijst met ingebouwde notaties voor datum/tijd.

![Stream Analytics oude datum-/ tijdindelingen](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

### <a name="extensibility-and-restrictions"></a>Uitbreidbaarheid en beperkingen

U kunt zoveel tokens `{datetime:<specifier>}`, zoals u in het padpatroon wilt, totdat het voorvoegsel van pad tekenlimiet is bereikt. Indeling weergaven kunnen niet worden gecombineerd in een enkele token buiten de al vermeld door de datum en tijd vervolgkeuzelijsten combinaties. 

Voor de partitie van een pad van `logs/MM/dd`:

|Geldige expressie   |Ongeldige expressie   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

U kunt de dezelfde indelingsopgave meerdere keren gebruiken in het voorvoegsel voor het pad. Het token moet telkens worden herhaald.

### <a name="hive-streaming-conventions"></a>Conventies voor hive-Streaming

Aangepaste padpatronen voor blob-opslag kunnen worden gebruikt met de Hive-Streaming-overeenkomst, die wordt verwacht dat de mappen die moeten worden gelabeld met `column=` in naam van de map.

Bijvoorbeeld `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Aangepaste uitvoer wordt voorkomen dat de moeite van het wijzigen van tabellen en partities handmatig toe te voegen aan poortgegevens tussen Azure Stream Analytics en Hive. Veel mappen kunnen in plaats daarvan worden toegevoegd automatisch met behulp van:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Voorbeeld

Maken van een storage-account, een resourcegroep, een Stream Analytics-taak en een invoerbron volgens de [Azure Stream Analytics-Azure Portal](stream-analytics-quick-create-portal.md) snelstartgids. Gebruik de dezelfde voorbeeldgegevens gebruikt in de Quick Start guide, die ook beschikbaar in de [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Maak een uitvoerlocatie blob met de volgende configuratie:

![Stream Analytics maken uitvoerlocatie blob](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Het patroon volledig pad is als volgt:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


Wanneer u de taak start, wordt de mapstructuur van een op basis van het padpatroon gemaakt in uw blobcontainer. U kunt inzoomen op het niveau van de dag.

![Stream Analytics blob uitvoer met de aangepaste padpatroon](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Volgende stappen

* [Inzicht in de uitvoer van Azure Stream Analytics](stream-analytics-define-outputs.md)
