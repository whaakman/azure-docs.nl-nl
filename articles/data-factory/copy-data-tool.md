---
title: "Gegevens kopiëren van Azure Data Factory hulpprogramma | Microsoft Docs"
description: "Bevat informatie over het hulpprogramma voor kopiëren in Azure Data Factory-gebruikersinterface"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 2fb25dcc0de4ebb1d025101670a9edfe3fe2bea9
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Hulpprogramma voor kopiëren-gegevens in Azure Data Factory
De gegevens kopiëren van Azure Data Factory-hulpprogramma kan vergemakkelijken en optimaliseert de het proces van het opnemen van gegevens in een data lake, dat meestal de eerste stap in een end-to-end gegevens integratiescenario.  Bespaart u tijd, vooral wanneer u Azure Data Factory om op te nemen van gegevens uit een gegevensbron voor de eerste keer gebruikt. Enkele van de voordelen van het gebruik van dit hulpprogramma zijn:

- Wanneer u de gegevens kopiëren van Azure Data Factory-hulpprogramma gebruikt, doen moet u niet Data Factory-definities voor de gekoppelde services, gegevenssets, pijplijnen, activiteiten en triggers begrijpen. 
- De stroom van gegevens kopiëren-hulpprogramma is een intuïtieve voor het laden van gegevens in een data lake. Het hulpprogramma maakt automatisch alle benodigde Data Factory-resources om gegevens te kopiëren uit de gegevensopslag van de geselecteerde bron in het geselecteerde doel/sink-gegevensarchief. 
- Het hulpprogramma voor kopiëren helpt u bij het valideren van de gegevens die op het moment van ontwerpen, waardoor u te voorkomen dat eventuele fouten aan het begin zelf is wordt ingenomen.
- Als u nodig hebt om complexe bedrijfslogica om gegevens te laden in een data lake te implementeren, kunt u de Data Factory-resources gemaakt door het hulpprogramma voor kopiëren met behulp van het ontwerp van per activiteit in de gebruikersinterface van Data Factory nog bewerken. 

De volgende tabel bevat richtlijnen voor het gebruik van het hulpprogramma voor kopiëren versus per activiteit ontwerpen in Data Factory-gebruikersinterface: 

| Hulpprogramma voor kopiëren-gegevens | Per activiteit (kopieeractiviteit) ontwerpen |
| -------------- | -------------------------------------- |
| U wilt gemakkelijk maken voor een gegevens laden van taak zonder te weten over Azure Data Factory-entiteiten (gekoppelde services, gegevenssets, pijplijnen, enz.) | Wilt u complexe en flexibele logica voor het laden van gegevens in lake implementeren. |
| Wilt u snel een groot aantal gegevensartefacten laden in een data lake. | U wilt kopiëren activiteit met de volgende activiteiten voor opschonen of in verwerking gegevens zijn gekoppeld. |

Voor het starten van het hulpprogramma voor kopiëren, klikt u op de **gegevens kopiëren** tegel op de startpagina van uw gegevensfactory.

![Pagina aan de slag - koppeling naar gegevens kopiëren-hulpprogramma](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Intuïtieve stroom voor het laden van gegevens in een data lake
Dit hulpprogramma kunt u eenvoudig om gegevens te verplaatsen van een groot aantal gegevensbronnen naar bestemmingen in minuten met een intuïtieve stroom:  

1. Configureer instellingen voor de **bron**.
2. Configureer instellingen voor de **bestemming**. 
3. Configureer **geavanceerde instellingen** voor de kopieerbewerking zoals kolomtoewijzing, prestatie-instellingen en tolerantie-instellingen voor fouttolerantie. 
4. Geef een **planning** voor de gegevens laden van taak. 
5. Bekijk **samenvatting** van Data Factory-entiteiten worden gemaakt. 
6. **Bewerken** de pijplijn voor het bijwerken van instellingen voor de kopieeractiviteit indien nodig. 

 Het hulpprogramma is ontworpen met big data in gedachten vanaf het begin, met ondersteuning voor diverse gegevens en objecttypen. U kunt deze honderden mappen, bestanden of tabellen te verplaatsen. Het hulpprogramma ondersteunt voorbeeld van automatische gegevens, schema vastleggen en automatische toewijzing en gegevens ook filteren.

![Hulpprogramma voor kopiëren-gegevens](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
Een gedeelte van de gegevens uit de geselecteerde bronsite gegevensopslag, zodat u kunt de gegevens die wordt gekopieerd, kunt u bekijken. Als de brongegevens zich in een tekstbestand, parseert het hulpprogramma voor kopiëren bovendien het tekstbestand voor automatische detectie van de rij en kolom scheidingstekens en het schema.

![Instellingen van bestand](./media/copy-data-tool/file-format-settings.png)

Na de detectie:

![Gedetecteerde bestandsinstellingen en preview](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Schema vastleggen en automatische toewijzing
Het schema van de gegevensbron is mogelijk niet hetzelfde als het schema van de gegevensbestemming in veel gevallen. In dit scenario moet u de kolommen van het schema van de gegevensbron aan kolommen van het doelschema toewijzen.

Het hulpprogramma voor kopiëren wordt bewaakt en uw gedrag leert wanneer u kolommen tussen de bron- en doelserver winkels wilt toewijzen. Nadat u een of meer kolommen uit brongegevensarchief selecteren en toe te aan het doelschema wijzen, start het hulpprogramma voor kopiëren voor het analyseren van het patroon voor kolom-paren die u aan beide zijden verzameld. Vervolgens het hetzelfde patroon toegepast op de rest van de kolommen. Daarom ziet u alle kolommen zijn toegewezen aan de bestemming op een manier die u wilt dat direct na enkele klikken.  Als u niet tevreden met de keuze van de kolomtoewijzing is opgegeven door gegevens kopiëren hulpprogramma bent, kunt u deze negeren en doorgaan met de kolommen handmatig toewijzen. Ondertussen het hulpprogramma voor kopiëren voortdurend leert en updates van het patroon en bereikt uiteindelijk de juiste patroon voor de kolomtoewijzing die u wilt bereiken. 

> [!NOTE]
> Bij het kopiëren van gegevens uit SQL Server of Azure SQL Database in Azure SQL Data Warehouse, als de tabel niet in het doelarchief bestaat, ondersteunt gegevens kopiëren hulpprogramma het maken van de tabel automatisch met behulp van het schema van de gegevensbron. 

## <a name="filter-data"></a>Gegevens filteren
U kunt filteren brongegevens om alleen de gegevens selecteren die moet worden gekopieerd naar het gegevensarchief sink. Filteren vermindert het volume van de gegevens worden gekopieerd naar het gegevensarchief sink en daarom verhoogt de doorvoer van de kopieerbewerking. Hulpprogramma voor kopiëren-gegevens biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van de SQL-querytaal of bestanden in een Azure blob-map. 

### <a name="filter-data-in-a-database"></a>Filteren van gegevens in een database
De volgende schermafbeelding ziet een SQL-query om de gegevens te filteren.

![Filteren van gegevens in een database](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Filter de gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad van de gegevens gekopieerd van een map. De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{uur}**, en **{minuut}**. Bijvoorbeeld: inputfolder / {year} / {month} / {day}. 

Stel dat ingevoerde mappen in de volgende indeling hebben: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016 -> 03 -> 01-02 >), en klik op **Kies**. U ziet 2016-03/01-02 in het tekstvak. 

Vervang vervolgens **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{uur}**, en druk op de **tabblad** sleutel. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Filterbestand of map](./media/copy-data-tool/filter-file-or-folder.png)

Het hulpprogramma voor kopiëren genereert parameters met expressies, functies, en systeemvariabelen die kunnen worden gebruikt om aan te duiden {year}, {month}, {day}, {uur} en {minuut} bij het maken van de pijplijn. Zie voor meer informatie de [het lezen of schrijven gepartitioneerd gegevens](how-to-read-write-partitioned-data.md) artikel.

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren nadat de of volgens een schema (elk uur, dagelijks, enzovoort). Deze opties kunnen worden gebruikt voor de connectors in verschillende omgevingen, met inbegrip van on-premises, cloud en lokale bureaublad. 

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar doel. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u kopiëren van gegevens op een terugkeer die u opgeeft. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Opties voor het plannen](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Volgende stappen
Probeer deze zelfstudies die gebruikmaken van het hulpprogramma voor kopiëren:

- [Snelstartgids: Maak een gegevensfactory met het hulpprogramma gegevens kopiëren](quickstart-create-data-factory-copy-data-tool.md)
- [Zelfstudie: gegevens kopiëren in Azure met behulp van het hulpprogramma voor kopiëren](tutorial-copy-data-tool.md) 
- [Zelfstudie: kopie on-premises gegevens in Azure maken met het hulpprogramma voor kopiëren](tutorial-hybrid-copy-data-tool.md)
