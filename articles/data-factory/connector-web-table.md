---
title: "Gegevens kopiëren van Web-tabel met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over Web tabel Connector van Azure Data Factory waarmee u gegevens voor het kopiëren van een tabel web tot gegevensarchieven die worden ondersteund door Data Factory als PUT."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: c5d2fdb3ed3c00114437b0be9759bf8bea2521b7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Gegevens kopiëren van Web-tabel met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-web-table-connector.md)
> * [Versie 2 - Preview](connector-web-table.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een database van de tabel Web. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Web tabel-connector in V1](v1/data-factory-web-table-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Web tabel database kopiëren naar een ondersteunde sink data store. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder deze tabel Web connector ondersteunt de **tabelinhoud extraheren uit een HTML-pagina**. Voor informatie over het ophalen van gegevens van een HTTP/s-eindpunt gebruik [HTTP connector](connector-http.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze tabel Web connector, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar de connector voor Web-tabel.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Web tabel gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Web** |Ja |
| url | URL van de webbron |Ja |
| authenticationType | De waarde is toegestaan: **anoniem**. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Een Runtime Self-hosted-integratie is vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld:**

```json
{
    "name": "WebLinkedService",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "url" : "https://en.wikipedia.org/wiki/",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Web tabel gegevensset.

Om gegevens te kopiëren uit de tabel Web, stel de eigenschap type van de gegevensset **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **WebTable** | Ja |
| pad |Een relatieve URL naar de resource die de tabel bevat. |Nee. Als het pad niet wordt opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get-index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

**Voorbeeld:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de tabel webbron.

### <a name="web-table-as-source"></a>Web-tabel als bron

Om gegevens te kopiëren van Web-tabel, stelt u het brontype in de kopieerbewerking naar **WebSource**, zonder aanvullende eigenschappen worden ondersteund.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromWebTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Web table input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "WebSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Ophalen van de index van een tabel in een HTML-pagina

1. Start **Excel 2016** en schakel over naar de **gegevens** tabblad.
2. Klik op **nieuwe Query** Wijs op de werkbalk **van andere bronnen** en klik op **uit Web**.

    ![Power Query-menu](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. In de **uit Web** dialoogvenster Voer **URL** die u wilt gebruiken in de gekoppelde service JSON (bijvoorbeeld: https://en.wikipedia.org/wiki/) samen met het pad dat u voor de gegevensset opgeven wilt (bijvoorbeeld: AFI % 27s_100_Years... 100_Movies), en klik op **OK**.

    ![Dialoogvenster Web](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    URL die wordt gebruikt in dit voorbeeld: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Als u ziet **toegang tot webinhoud** dialoogvenster Selecteer het recht **URL**, **verificatie**, en klik op **Connect**.

   ![Dialoogvenster voor toegang tot Web-inhoud](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. Klik op een **tabel** item in de structuurweergave van de inhoud uit de tabel en klik vervolgens op **bewerken** knop onderaan.  

   ![Dialoogvenster Navigator](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. In de **Query-Editor** venster, klikt u op **geavanceerde Editor** op de werkbalk.

    ![De knop Geavanceerd Editor](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Het getal naast 'Bron' is in het dialoogvenster Geavanceerde Editor voor de index.

    ![Geavanceerde Editor - Index](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Als u Excel 2013 gebruikt, gebruikt u [Microsoft Power Query voor Excel](https://www.microsoft.com/download/details.aspx?id=39379) ophalen van de index. Zie [verbinding maken met een webpagina](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikel voor meer informatie. De stappen zijn vergelijkbaar zijn als u [Microsoft Power BI voor bureaublad](https://powerbi.microsoft.com/desktop/).


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).