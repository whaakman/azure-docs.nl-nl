---
title: Gegevens kopiëren van Web-tabel met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over Web tabel Connector van Azure Data Factory waarmee u gegevens kopiëren uit een webtabel naar gegevensarchieven die worden ondersteund door Data Factory als sink.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: e578b3a6b3905569567b568b0130c1ed1b90d915
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019386"
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Gegevens van Web-tabel kopiëren met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-web-table-connector.md)
> * [Huidige versie](connector-web-table.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een database van de tabel Web. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

Het verschil tussen deze tabel Web-connector, de [REST-connector](connector-rest.md) en de [HTTP-connector](connector-http.md) zijn:

- **Web-connector voor tabel** extraheert inhoud van de webpagina van een HTML-tabel.
- **REST-connector** specifiek ondersteuning kopiëren van gegevens uit RESTful-API's.
- **HTTP-connector** is generiek gegevens ophalen van een HTTP-eindpunt, bijvoorbeeld om bestand te downloaden. 

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Web tabeldatabase kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies deze tabel Web connector ondersteunt **tabelinhoud extraheren uit een HTML-pagina**.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze Web-connector voor tabel, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor tabel-Web-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Web tabel gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Web** |Ja |
| url | URL naar de Web-bron |Ja |
| authenticationType | Toegestane waarde is: **Anonieme**. |Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Web table-gegevensset.

Om gegevens te kopiëren uit de tabel Web, stel de eigenschap type van de gegevensset in **WebTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **WebTable** | Ja |
| pad |Een relatieve URL naar de resource met de tabel. |Nee. Als geen pad is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

**Voorbeeld:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": {
            "referenceName": "<Web linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Web tabelbron.

### <a name="web-table-as-source"></a>Webtabel als bron

Om gegevens te kopiëren uit de tabel Web, stelt u het brontype in de kopieeractiviteit naar **WebSource**, geen extra eigenschappen worden ondersteund.

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

## <a name="get-index-of-a-table-in-an-html-page"></a>Index van een tabel in een HTML-pagina ophalen

Om op te halen van de index van een tabel die u configureren wilt [gegevensseteigenschappen](#dataset-properties), u kunt bijvoorbeeld Excel 2016 gebruiken als het hulpprogramma als volgt:

1. Start **Excel 2016** en schakel over naar de **gegevens** tabblad.
2. Klik op **nieuwe Query** op de werkbalk, wijs **van andere bronnen** en klikt u op **van Web**.

    ![Power Query-menu](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. In de **van Web** dialoogvenster vak, voer **URL** die u wilt gebruiken in de gekoppelde service JSON (bijvoorbeeld: https://en.wikipedia.org/wiki/) samen met het pad dat u voor de gegevensset opgeven wilt (bijvoorbeeld: AFI % 27s_100_Years... 100_Movies), en klikt u op **OK**.

    ![Uit het dialoogvenster Web](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    De URL die wordt gebruikt in dit voorbeeld: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Als u ziet **toegang tot webinhoud** dialoogvenster vak, selecteert u het recht **URL**, **verificatie**, en klikt u op **Connect**.

   ![Dialoogvenster voor toegang tot Web-inhoud](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. Klik op een **tabel** item in de structuurweergave wordt weergegeven om te zien van de inhoud uit de tabel en klik vervolgens op **bewerken** knop aan de onderkant.  

   ![Dialoogvenster Navigator](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. In de **Query-Editor** venster, klikt u op **geavanceerde Editor** op de werkbalk.

    ![Knop voor geavanceerde Editor](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Klik in het dialoogvenster Geavanceerde Editor is het getal naast 'Bron' de index.

    ![Geavanceerde Editor - Index](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Als u Excel 2013 gebruikt, gebruikt u [Microsoft Power Query voor Excel](https://www.microsoft.com/download/details.aspx?id=39379) om op te halen van de index. Zie [verbinding maken met een webpagina](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikel voor meer informatie. De stappen zijn vergelijkbaar als u [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/).


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
