---
title: Gegevens kopiëren hulpprogramma voor Azure Data Factory | Microsoft Docs
description: Bevat informatie over het hulpprogramma Copy Data in Azure Data Factory-UI
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: yexu
ms.openlocfilehash: 107687c785433f81870449d1445136b5148a4d2c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58101751"
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Hulpprogramma voor kopiëren-gegevens in Azure Data Factory
Het hulpprogramma Copy Data van Azure Data Factory vereenvoudigt en optimaliseert het proces voor het opnemen van gegevens in een data lake, die meestal een eerste stap in een scenario voor end-to-end data-integratie.  Bespaart u tijd, met name wanneer u Azure Data Factory voor opname van gegevens uit een gegevensbron voor de eerste keer gebruikt. Enkele van de voordelen van het gebruik van dit hulpprogramma zijn:

- Wanneer u het hulpprogramma Copy Data van Azure Data Factory gebruikt, doen moet u geen Data Factory-definities voor de gekoppelde services, gegevenssets, activiteiten, pijplijnen en triggers begrijpen. 
- De stroom van het hulpprogramma Copy Data is om gegevens te laden in een data lake intuïtieve. Het hulpprogramma maakt automatisch de noodzakelijke Data Factory resources voor gegevens van de geselecteerde bron-gegevensopslag kopiëren naar het geselecteerde doel/sink-gegevensarchief. 
- Het hulpprogramma Copy Data helpt u de gegevens die wordt opgenomen op het moment van schrijven, die helpt u bij het voorkomen van potentiële fouten aan het begin zelf valideren.
- Als u nodig hebt om complexe bedrijfslogica voor het laden van gegevens in een data lake te implementeren, kunt u de Data Factory-resources die door het hulpprogramma Copy Data wordt gemaakt met behulp van het ontwerpen van de per-activiteit in Data Factory-gebruikersinterface nog steeds bewerken. 

De volgende tabel bevat richtlijnen voor wanneer u het hulpprogramma Copy Data vs. per activiteit ontwerpen in Data Factory-gebruikersinterface: 

| Hulpprogramma voor het kopiëren van gegevens | Per activiteit (kopieeractiviteit) ontwerpen |
| -------------- | -------------------------------------- |
| U wilt dat eenvoudig op een gegevens te laden van de taak uit te leren over Azure Data Factory-entiteiten (gekoppelde services, gegevenssets, pijplijnen, enz.) | Wilt u complexe en flexibele logica voor het laden van gegevens in lake implementeren. |
| Wilt u snel een groot aantal gegevensartefacten laden in een data lake. | Wilt u keten van kopieeractiviteit met latere activiteiten voor gegevens opschonen of verwerking. |

Het hulpprogramma Copy Data starten, klikt u op de **Copy Data** tegel op de startpagina van uw data factory.

![De pagina aan slag - koppeling naar het hulpprogramma Copy Data](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Intuïtieve stroom voor het laden van gegevens in een data lake
Dit hulpprogramma kunt u eenvoudig om gegevens te verplaatsen van een groot aantal bronnen naar bestemmingen binnen enkele minuten met een intuïtieve stroom:  

1. Instellingen configureren voor de **bron**.
2. Instellingen configureren voor de **bestemming**. 
3. Configureer **geavanceerde instellingen** voor de kopieerbewerking zoals kolomtoewijzing, prestatie-instellingen en fouttolerantie tolerantie-instellingen. 
4. Geef een **planning** voor laden van de taak van de gegevens. 
5. Beoordeling **samenvatting** van Data Factory-entiteiten worden gemaakt. 
6. **Bewerken** de pijplijn voor het bijwerken van instellingen voor de kopieeractiviteit, indien nodig. 

   Het hulpprogramma is ontworpen met big data in gedachten vanaf het begin, met ondersteuning voor diverse gegevens en objecttypen. U kunt deze gebruiken om te verplaatsen van honderden mappen, bestanden of tabellen. Het hulpprogramma ondersteunt automatische Gegevensvoorbeeld, schema wordt vastgelegd en automatische toewijzing en gegevens ook filteren.

![Hulpprogramma voor het kopiëren van gegevens](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
U een voorbeeld van een deel van de gegevens van de geselecteerde brongegevensarchief, zodat u kunt het valideren van de gegevens die worden gekopieerd. Als de brongegevens bevinden zich in een tekstbestand, parseert het hulpprogramma Copy Data bovendien het tekstbestand dat voor het automatisch detecteren van de rij- als scheidingstekens en het schema.

![Bestandsinstellingen](./media/copy-data-tool/file-format-settings.png)

Na de detectie:

![Gedetecteerde instellingen en Preview-versie](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Schema vastleggen en automatische toewijzing
Het schema van de gegevensbron is mogelijk niet hetzelfde als het schema van de bestemming in veel gevallen. In dit scenario moet u kolommen uit van het schema van de gegevensbron op kolommen uit het doelschema toewijzen.

Het hulpprogramma Copy Data bewaakt en leert het gedrag wanneer u kolommen tussen bron- en doelserver zijn toegewezen. Nadat u een of meer kolommen uit de bron-gegevensopslag selecteren en toe te aan het doelschema wijzen, start het hulpprogramma Copy Data te analyseren van het patroon voor paren met een kolom die u uit beide zijden opgehaald. Vervolgens geldt het hetzelfde patroon voor de rest van de kolommen. Daarom ziet u alle kolommen zijn toegewezen aan de bestemming op een manier die u wilt dat direct na enkele klikken.  Als u niet tevreden met de keuze van de kolomtoewijzing geleverd door het hulpprogramma Copy Data bent, kunt u dit negeren en doorgaan met het handmatig toewijzen van de kolommen. Ondertussen het hulpprogramma Copy Data voortdurend leert en updates van het patroon en uiteindelijk de juiste patroon voor de kolomtoewijzing van de die u wilt bereiken bereikt. 

> [!NOTE]
> Bij het kopiëren van gegevens uit SQL Server of Azure SQL Database in Azure SQL Data Warehouse, als de tabel niet in het doelarchief bestaat, ondersteunt het hulpprogramma Copy Data het maken van de tabel automatisch met behulp van het schema van de gegevensbron. 

## <a name="filter-data"></a>Gegevens filteren
U kunt filteren brongegevens om alleen de gegevens die moet worden gekopieerd naar het sink-gegevensopslag te selecteren. Filteren vermindert het volume van de gegevens moeten worden gekopieerd naar het sink-gegevensopslag en daarom verhoogt de doorvoer van de kopieerbewerking. Het kopieerprogramma van gegevens biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van het SQL-query-taal, of de bestanden in een Azure blob-map. 

### <a name="filter-data-in-a-database"></a>Filteren van gegevens in een database
De volgende schermafbeelding ziet u een SQL-query om de gegevens te filteren.

![Filteren van gegevens in een database](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Filteren van gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad naar de gegevens uit een map te kopiëren. De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{hour}**, en **{minute}**. Bijvoorbeeld: inputfolder / {year} / {month} / {day}. 

Stel dat u hebt opgegeven dat mappen in de volgende indeling: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016-03 > -> 01-02 >), en klikt u op **kiezen**. Hier ziet u 2016/03/01/02 in het tekstvak in. 

Vervang vervolgens **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{hour}**, en druk op de **tabblad** sleutel. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Filterbestand of map](./media/copy-data-tool/filter-file-or-folder.png)

Het hulpprogramma Copy Data genereert parameters met expressies, functies en systeemvariabelen die kunnen worden gebruikt om aan te duiden {year}, {month}, {day}, {hour} en {minute} bij het maken van de pijplijn. Zie voor meer informatie de [lezen of schrijven gegevens gepartitioneerd](how-to-read-write-partitioned-data.md) artikel.

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren eenmalig of volgens een schema (per uur, dagelijks, enzovoort). Deze opties kunnen worden gebruikt voor de connectors in verschillende omgevingen, met inbegrip van on-premises, cloud en lokale bureaublad. 

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar een bestemming. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u het kopiëren van gegevens op een terugkeer die u opgeeft. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Opties voor het plannen](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Volgende stappen
Probeer deze zelfstudie die het hulpprogramma Copy Data gebruiken:

- [Snelstartgids: een data factory maken met het hulpprogramma Copy Data](quickstart-create-data-factory-copy-data-tool.md)
- [Zelfstudie: gegevens kopiëren in Azure met behulp van het hulpprogramma Copy Data](tutorial-copy-data-tool.md) 
- [Zelfstudie: on-premises gegevens kopiëren naar Azure met behulp van het hulpprogramma Copy Data](tutorial-hybrid-copy-data-tool.md)
