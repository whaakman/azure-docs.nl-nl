---
title: Aangepaste datum/tijd-padpatronen voor Azure Stream Analytics blob storage-uitvoer (Preview)
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: da29c6bd8ddc1e2f62a78fb683df5e1784141722
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452561"
---
# <a name="custom-datetime-path-patterns-for-azure-stream-analytics-blob-storage-output-preview"></a>Aangepaste datum/tijd-padpatronen voor Azure Stream Analytics blob storage-uitvoer (Preview)

Azure Stream Analytics biedt ondersteuning voor aangepaste datum en tijd-indeling weergaven in het pad voor blob storage-uitvoer. Aangepaste datum/tijd-padpatronen kunnen u opgeven de indeling van een uitvoer die overeenstemt met Hive-Streaming-conventies, waardoor Azure Stream Analytics om gegevens te verzenden naar Azure HDInsight en Azure Databricks voor downstream verwerken. Aangepaste datum/tijd-padpatronen eenvoudig worden geïmplementeerd met behulp van de `datetime` sleutelwoord in het veld pad naar het voorvoegsel van de uitvoer, samen met de indelingsopgave blob. Bijvoorbeeld `{datetime:yyyy}`.

Gebruik deze koppeling voor [Azure Portal](https://portal.azure.com/?Microsoft_Azure_StreamAnalytics_bloboutputcustomdatetimeformats=true) om uit te schakelen van de functievlag waarmee de aangepaste datum/tijd-padpatronen voor blob storage-Uitvoervoorbeeld. Deze functie zal binnenkort worden ingeschakeld in de belangrijkste portal.

## <a name="supported-tokens"></a>Ondersteunde tokens

De volgende indeling aanduiding tokens kunnen zelfstandig of in combinatie worden gebruikt om aangepaste datum/tijd-notatie:

|Indelingsopgave   |Beschrijving   |De resultaten in het voorbeeld van de tijd 2018-01-02T10:06:08|
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

## <a name="extensibility-and-restrictions"></a>Uitbreidbaarheid en beperkingen

U kunt zoveel tokens `{datetime:<specifier>}`, zoals u in het padpatroon wilt, totdat het voorvoegsel van pad tekenlimiet is bereikt. Indeling weergaven kunnen niet worden gecombineerd in een enkele token buiten de al vermeld door de datum en tijd vervolgkeuzelijsten combinaties. 

Voor de partitie van een pad van `logs/MM/dd`:

|Geldige expressie   |Ongeldige expressie   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

U kunt de dezelfde indelingsopgave meerdere keren gebruiken in het voorvoegsel voor het pad. Het token moet telkens worden herhaald.

## <a name="hive-streaming-conventions"></a>Conventies voor hive-Streaming

Aangepaste padpatronen voor blob-opslag kunnen worden gebruikt met de Hive-Streaming-overeenkomst, die wordt verwacht dat de mappen die moeten worden gelabeld met `column=` in naam van de map.

Bijvoorbeeld `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Aangepaste uitvoer wordt voorkomen dat de moeite van het wijzigen van tabellen en partities handmatig toe te voegen aan poortgegevens tussen Azure Stream Analytics en Hive. Veel mappen kunnen in plaats daarvan worden toegevoegd automatisch met behulp van:

```
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Voorbeeld

Maken van een storage-account, een resourcegroep, een Stream Analytics-taak en een invoerbron volgens de [Azure Stream Analytics-Azure Portal](stream-analytics-quick-create-portal.md) snelstartgids. Gebruik de dezelfde voorbeeldgegevens gebruikt in de Quick Start guide, die ook beschikbaar in de [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Maak een uitvoerlocatie blob met de volgende configuratie:

![Stream Analytics maken uitvoerlocatie blob](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Het patroon volledig pad is als volgt:

```
year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}
```

Wanneer u de taak start, wordt de mapstructuur van een op basis van het padpatroon gemaakt in uw blobcontainer. U kunt inzoomen op het niveau van de dag.

![Stream Analytics blob uitvoer met de aangepaste padpatroon](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Volgende stappen

* [Inzicht in de uitvoer van Azure Stream Analytics](stream-analytics-define-outputs.md)
