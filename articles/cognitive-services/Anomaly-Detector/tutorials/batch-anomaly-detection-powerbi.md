---
title: Afwijkingen met behulp van batch-detectie- en Power BI visualiseren
titleSuffix: Azure Cognitive Services
description: Gebruik de API voor Afwijkingsdetectie Detector en Power BI afwijkingen in uw time series-gegevens visualiseren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: tutorial
ms.date: 04/30/2019
ms.author: aahi
ms.openlocfilehash: 74b51d04f2706d890475c500e1e730cff75397c5
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721482"
---
# <a name="tutorial-visualize-anomalies-using-batch-detection-and-power-bi"></a>Zelfstudie: Afwijkingen met behulp van batch-detectie- en Power BI visualiseren

Met deze zelfstudie zoeken naar afwijkingen in een tijd-serie-gegevensset als een batch. Met behulp van Power BI desktop, wordt u nemen van een Excel-bestand, de gegevens voorbereiden voor de API van de detectie van afwijkingen en statistische afwijkingen in het visualiseren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Power BI Desktop gebruiken om te importeren en transformeren van een gegevensset van time series
> * Power BI Desktop integreren in de Anomaliedetectie Detector-API voor afwijkingsdetectie voor batch
> * Afwijkingen vinden in uw gegevens, inclusief waarden verwacht en gezien en anomaly detection grenzen visualiseren.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/get-started/), dat verkrijgbaar is gratis.
* Een excel-bestand (.xlsx) met time series-gegevens verwijst. De voorbeeldgegevens voor deze Quick Start kunt u vinden op [GitHub](https://go.microsoft.com/fwlink/?linkid=2090962)

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="load-and-format-the-time-series-data"></a>Laden en formatteren van de time series-gegevens

Als u wilt beginnen, opent u Power BI Desktop en laden van de time series-gegevens die u hebt gedownload van de vereisten. Dit excel-bestand bevat een reeks Coordinated Universal Time (UTC) tijdstempel / waarde-paren.  

> [!NOTE]
> Power BI kunt gegevens uit een groot aantal bronnen, zoals CSV-bestanden, SQL databases, Azure blob-opslag en meer gebruiken.  

Klik in het hoofdvenster van Power BI Desktop, op de **Start** lint. In de **externe gegevens** groep van het lint, open de **gegevens ophalen** vervolgkeuzelijst en klik op **Excel**.

![Een afbeelding van de knop 'Gegevens ophalen' in Power BI](../media/tutorials/power-bi-get-data-button.png)

Nadat u het dialoogvenster wordt weergegeven, gaat u naar de map waar u het voorbeeld .xlsx-bestand hebt gedownload en selecteer deze. Na de **Navigator** dialoogvenster wordt weergegeven, klikt u op **Sheet1**, en vervolgens **bewerken**.

![Een installatiekopie van het scherm data source 'Navigator' in Power BI](../media/tutorials/navigator-dialog-box.png)

Power BI converteert de tijdstempels in de eerste kolom een `Date/Time` gegevenstype. Deze tijdstempels moet worden geconverteerd naar tekst om te worden verzonden naar de detectie van afwijkingen API. Als de Power Query-editor niet automatisch wordt geopend, klikt u op **query's bewerken** op het tabblad Start. 

Klik op de **transformeren** lint in de Power Query-Editor. In de **elke kolom** groep, open de **gegevenstype:** vervolgkeuzelijst en selecteer **tekst**.

![Een installatiekopie van het scherm data source 'Navigator' in Power BI](../media/tutorials/data-type-drop-down.png)

Wanneer u een melding ontvangt over het wijzigen van het kolomtype, klikt u op **huidige vervangen**. Daarna klikt u op **sluiten en toepassen** of **toepassen** in de **Start** lint. 

## <a name="create-a-function-to-send-the-data-and-format-the-response"></a>Maak een functie voor het verzenden van de gegevens en formatteren van de reactie

Als u wilt opmaken en het gegevensbestand verzenden naar de Anomaliedetectie-API voor Detector, kunt u een query op de tabel hierboven gemaakte aanroepen. In de Power Query-Editor van het **Start** lint, open de **nieuwe bron** vervolgkeuzelijst en klik op **lege Query**.

Zorg ervoor dat uw nieuwe query is geselecteerd en klik vervolgens op **geavanceerde Editor**. 

![Een afbeelding van de knop 'Geavanceerde Editor' in Power BI](../media/tutorials/advanced-editor-screen.png)

Gebruik het volgende codefragment in de Power Query M om de kolommen uit de tabel te verzenden naar de API in de geavanceerde Editor. De query wordt later een tabel maken van het JSON-antwoord en retourneren. Vervang de `apiKey` variabele door de geldige Anomaliedetectie Detector API-sleutel en `endpoint` aan uw eindpunt. Nadat u de query in de geavanceerde Editor hebt ingevoerd, klikt u op **gedaan**.

```M
(table as table) => let

    apikey      = "[Placeholder: Your Anomaly Detector resource access key]",
    endpoint    = "[Placeholder: Your Anomaly Detector resource endpoint]/anomalydetector/v1.0/timeseries/entire/detect",
    inputTable = Table.TransformColumnTypes(table,{{"Timestamp", type text},{"Value", type number}}),
    jsontext    = Text.FromBinary(Json.FromValue(inputTable)),
    jsonbody    = "{ ""Granularity"": ""daily"", ""Sensitivity"": 95, ""Series"": "& jsontext &" }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Content-Type" = "application/json", #"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),

    respTable = Table.FromColumns({
                    
                     Table.Column(inputTable, "Timestamp")
                     ,Table.Column(inputTable, "Value")
                     , Record.Field(jsonresp, "IsAnomaly") as list
                     , Record.Field(jsonresp, "ExpectedValues") as list
                     , Record.Field(jsonresp, "UpperMargins")as list
                     , Record.Field(jsonresp, "LowerMargins") as list
                     , Record.Field(jsonresp, "IsPositiveAnomaly") as list
                     , Record.Field(jsonresp, "IsNegativeAnomaly") as list

                  }, {"Timestamp", "Value", "IsAnomaly", "ExpectedValues", "UpperMargin", "LowerMargin", "IsPositiveAnomaly", "IsNegativeAnomaly"}
               ),
    
    respTable1 = Table.AddColumn(respTable , "UpperMargins", (row) => row[ExpectedValues] + row[UpperMargin]),
    respTable2 = Table.AddColumn(respTable1 , "LowerMargins", (row) => row[ExpectedValues] -  row[LowerMargin]),
    respTable3 = Table.RemoveColumns(respTable2, "UpperMargin"),
    respTable4 = Table.RemoveColumns(respTable3, "LowerMargin"),

    results = Table.TransformColumnTypes(

                respTable4,
                {{"Timestamp", type datetime}, {"Value", type number}, {"IsAnomaly", type logical}, {"IsPositiveAnomaly", type logical}, {"IsNegativeAnomaly", type logical},
                 {"ExpectedValues", type number}, {"UpperMargins", type number}, {"LowerMargins", type number}}
              )

 in results
```

Aanroepen van de query op het gegevensblad door te selecteren `Sheet1` hieronder **parameterwaarde invoeren**, en klikt u op **Invoke**. 

![Een afbeelding van de knop 'Geavanceerde Editor'](../media/tutorials/invoke-function-screenshot.png)

## <a name="data-source-privacy-and-authentication"></a>Bron-privacy van gegevens en -verificatie

> [!NOTE]
> Houd rekening met het beleid van uw organisatie voor de privacy van gegevens- en toegangsbeheer. Zie [privacyniveaus van Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-privacy-levels) voor meer informatie.

Krijgt u mogelijk een waarschuwingsbericht wordt weergegeven wanneer u probeert uit te voeren van de query omdat deze gebruikmaakt van een externe gegevensbron. 

![Een afbeelding van een waarschuwing gemaakt door Power BI](../media/tutorials/blocked-function.png)

U kunt dit verhelpen, klikt u op **bestand**, en **opties en instellingen**. Klik vervolgens op **opties**. Hieronder **huidige bestand**, selecteer **Privacy**, en **negeren de privacyniveaus en mogelijk verbeterde prestaties**. 

Bovendien krijgt u mogelijk een bericht waarin u wordt gevraagd om op te geven hoe u wilt verbinding maken met de API.

![Een afbeelding van een aanvraag voor het opgeven van referenties voor toegang](../media/tutorials/edit-credentials-message.png)

U kunt dit verhelpen, klikt u op **referenties bewerken** in het bericht. Nadat u het dialoogvenster wordt weergegeven, selecteert u **anoniem** anoniem verbinding maken met de API. Klik vervolgens op **Verbinden**. 

Daarna klikt u op **sluiten en toepassen** in de **Start** lint om de wijzigingen te passen.

## <a name="visualize-the-anomaly-detector-api-response"></a>Visualiseer de Anomaliedetectie Detector API-reactie

In het hoofdvenster van Power BI, beginnen met behulp van de query's hierboven hebt gemaakt om de gegevens te visualiseren. Selecteer eerst **lijndiagram** in **visualisaties**. Vervolgens de tijdstempel van de aangeroepen functie toevoegen aan het lijndiagram **as**. Met de rechtermuisknop op deze en selecteer **Timestamp**. 

![Met de rechtermuisknop op de waarde Timestamp](../media/tutorials/timestamp-right-click.png)

Voeg de volgende velden uit de **aangeroepen functie** aan van de grafiek **waarden** veld. Gebruik de onderstaande schermafbeelding voor het opbouwen van de grafiek.

    * Waarde
    * UpperMargins
    * LowerMargins
    * ExpectedValues

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/chart-settings.png)

Na het toevoegen van de velden, klik op de grafiek en pas de grootte ervan alle van de gegevenspunten weergeven. De grafiek ziet eruit als de in de schermafbeelding hieronder:

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/chart-visualization.png)

### <a name="display-anomaly-data-points"></a>Anomaliedetectie-gegevenspunten worden weergegeven

Aan de rechterkant van het Power BI-venster onder de **velden** in het deelvenster met de rechtermuisknop op **waarde** onder de **aangeroepen functiequery**, en klikt u op **nieuwe snelle meting**.

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/new-quick-measure.png)

Selecteer op het scherm dat verschijnt, **gefilterde waarde** als de berekening. Stel **baseren waarde** naar `Sum of Value`. Sleep vervolgens `IsAnomaly` uit de **aangeroepen functie** velden **Filter**. Selecteer `True` uit de **Filter** vervolgkeuzelijst.

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/new-quick-measure-2.png)

Nadat u hebt geklikt **Ok**, hebt u een `Value for True` veld onder aan de lijst met velden. Met de rechtermuisknop op het en wijzig de naam **Anomaliedetectie**. Toe te voegen aan van de grafiek **waarden**. Selecteer vervolgens de **indeling** hulpprogramma en het type x-as instellen op **Categorisch**.

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/format-x-axis.png)

Kleuren toepassen op de grafiek door te klikken op de **indeling** hulpprogramma en **gegevenskleuren**. De grafiek moet er ongeveer als volgt uitzien:

![Een installatiekopie van het nieuwe snelle meting scherm](../media/tutorials/final-chart.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Streaming afwijkingsdetectie met Azure Databricks](anomaly-detection-streaming-databricks.md)
