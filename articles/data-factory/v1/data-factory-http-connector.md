---
title: Gegevens verplaatsen van een HTTP-bron - Azure | Microsoft Docs
description: Informatie over het verplaatsen van gegevens uit een on-premises of HTTP-bron in de cloud met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 06eb11132d7e3968850aadb4bfdaa53261f14ada
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167469"
---
# <a name="move-data-from-an-http-source-by-using-azure-data-factory"></a>Gegevens verplaatsen van een HTTP-bron met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-http-connector.md)
> * [Versie 2 (huidige versie)](../connector-http.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Azure Data Factory-service gebruikt, raadpleegt u [HTTP-connector in V2](../connector-http.md).


In dit artikel bevat een overzicht van hoe u aan de Kopieeractiviteit in Azure Data Factory gebruiken om te verplaatsen van gegevens uit een on-premises of in de cloud van HTTP-eindpunt naar een ondersteunde sink-gegevensarchief. In dit artikel is gebaseerd op [gegevens verplaatsen met behulp van Kopieeractiviteit](data-factory-data-movement-activities.md), die een algemeen overzicht van de verplaatsing van gegevens met behulp van de Kopieeractiviteit geeft. Het artikel bevat ook de gegevensarchieven die ondersteuning biedt voor Kopieeractiviteit als bronnen en sinks.

Data Factory ondersteunt momenteel alleen om gegevens te verplaatsen van een HTTP-bron naar andere gegevensarchieven. Deze biedt geen gegevens te verplaatsen van andere gegevensarchieven naar een bestemming HTTP ondersteuning.

## <a name="supported-scenarios-and-authentication-types"></a>Ondersteunde scenario's en verificatietypen

U kunt deze HTTP-connector gebruiken om op te halen van gegevens uit *zowel een cloud en een eindpunt van de HTTP/S on-premises* met behulp van de HTTP **ophalen** of **POST** methoden. De volgende verificatietypen worden ondersteund: **anoniem**, **Basic**, **Digest**, **Windows**, en  **ClientCertificate**. Let op het verschil tussen deze connector en de [Web-connector voor tabel](data-factory-web-table-connector.md). De Web-connector voor tabel haalt inhoud uit een HTML-webpagina.

Wanneer u gegevens van een on-premises HTTP-eindpunt kopiëren, moet u Data Management Gateway installeren in de on-premises-omgeving of in een Azure-VM. Zie voor meer informatie over Data Management Gateway en voor stapsgewijze instructies voor het instellen van de gateway, [om gegevens te verplaatsen tussen on-premises locaties en de cloud](data-factory-move-data-between-onprem-and-cloud.md).

## <a name="get-started"></a>Aan de slag

U kunt een pijplijn met een kopieeractiviteit om gegevens te verplaatsen van een HTTP-bron met behulp van verschillende hulpprogramma's of API's maken:

- De eenvoudigste manier om een pijplijn te maken is het gebruik van de wizard kopiëren van gegevens. Zie voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren van gegevens, [zelfstudie: een pijplijn maken met behulp van de wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: de **Azure-portal**, **Visual Studio**, **Azure PowerShell**, een **Azure Resource Manager sjabloon**, wordt de **.NET API**, of de **REST-API**. Zie voor stapsgewijze instructies voor het maken van een pijplijn met copy activity de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Zie voor JSON-die gegevens kopiëren van een HTTP-bron naar Azure Blob-opslag voorbeelden, [JSON voorbeelden](#json-examples).

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel beschrijft de JSON-elementen die specifiek voor de HTTP-gekoppelde service zijn:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type | De **type** eigenschap moet worden ingesteld op **Http**. | Ja |
| url | De basis-URL naar de webserver. | Ja |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn **anoniem**, **Basic**, **Digest**, **Windows**, en **ClientCertificate**. <br><br> Raadpleeg de secties verderop in dit artikel voor meer eigenschappen en voorbeelden van de JSON voor deze verificatietypen. | Ja |
| enableServerCertificateValidation | Hiermee geeft u op of validatie van het servercertificaat SSL wordt ingeschakeld als de bron een HTTPS-webserver is. Wanneer de HTTPS-server een zelf-ondertekend certificaat gebruikt, stel dit in op **false**. | Nee<br /> (de standaardwaarde is **waar**) |
| gatewayName | De naam van het exemplaar van Data Management Gateway gebruiken om te verbinden met een on-premises HTTP-bron. | Ja, als u bij het kopiëren van gegevens uit een on-premises HTTP-bron |
| encryptedCredential | De versleutelde referenties voor toegang tot het HTTP-eindpunt. De waarde is automatisch gegenereerd bij het configureren van de verificatie-informatie in de wizard kopiëren of met behulp van de **ClickOnce** in het dialoogvenster. | Nee<br /> (alleen van toepassing wanneer u gegevens van een on-premises HTTP-server kopiëren) |

Zie voor meer informatie over het instellen van referenties voor een gegevensbron van on-premises HTTP-connector [gegevens verplaatsen tussen on-premises bronnen en de cloud met behulp van Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="using-basic-digest-or-windows-authentication"></a>Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

Stel **authenticationType** naar **Basic**, **Digest**, of **Windows**. Naast de algemene HTTP-Connectoreigenschappen die worden beschreven in de voorgaande secties, stel de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| gebruikersnaam | De gebruikersnaam gebruiken voor toegang tot het HTTP-eindpunt. | Ja |
| wachtwoord | Het wachtwoord voor de gebruiker (**gebruikersnaam**). | Ja |

**Voorbeeld: Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Met behulp van ClientCertificate verificatie

Als u wilt gebruikmaken van basisverificatie instellen **authenticationType** naar **ClientCertificate**. Naast de algemene HTTP-Connectoreigenschappen die worden beschreven in de voorgaande secties, stel de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| embeddedCertData | De met Base64 gecodeerde inhoud van de binaire gegevens van het PFX-bestand. | Geef óf een **embeddedCertData** of **certThumbprint** |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op de gatewaycomputer certificatenstore. Gelden alleen wanneer u gegevens van een on-premises HTTP-bron kopiëren. | Geef óf een **embeddedCertData** of **certThumbprint** |
| wachtwoord | Het wachtwoord dat is gekoppeld aan het certificaat. | Nee |

Als u **certThumbprint** voor verificatie en het certificaat is geïnstalleerd in het persoonlijke archief van de lokale computer, verleen leesmachtigingen voor de gateway-service:

1. Open de Microsoft Management Console (MMC). Voeg de **certificaten** -module die is gericht op **lokale Computer**.
2. Vouw **certificaten** > **persoonlijke**, en selecteer vervolgens **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief, en selecteer vervolgens **alle taken** >**persoonlijke sleutels beheren**.
3. Op de **Security** tabblad, voegt u het gebruikersaccount waarmee de Data Management Gateway Host-Service wordt uitgevoerd, met leestoegang tot het certificaat.  

**Voorbeeld: Met behulp van een clientcertificaat**

Deze gekoppelde service koppelt uw data factory op een on-premises HTTP-webserver. Het maakt gebruik van een clientcertificaat dat is geïnstalleerd op de machine waarvoor Data Management Gateway is geïnstalleerd.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

**Voorbeeld: Een clientcertificaat gebruiken in een bestand**

Deze gekoppelde service koppelt uw data factory op een on-premises HTTP-webserver. Het maakt gebruik van een client-certificaatbestand op een computer met Data Management Gateway is geïnstalleerd.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "Base64-encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Sommige delen van een gegevensset JSON-bestand, zoals de structuur, beschikbaarheid en het beleid, zijn identiek voor alle typen van gegevensset (Azure SQL Database, Azure Blob-opslag, Azure Table storage).

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets maken](data-factory-create-datasets.md).

De **typeProperties** sectie verschilt voor elk type gegevensset. De **typeProperties** sectie bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor een gegevensset met de **Http** type heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** van de gegevensset moet worden ingesteld op **Http**. | Ja |
| relativeurl bevatten | Een relatieve URL naar de resource die de gegevens bevat. Als het pad is niet opgegeven, wordt alleen de URL die opgegeven in de definitie van de gekoppelde service gebruikt. <br><br> Kan de URL van een dynamische, kunt u [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md). Voorbeeld: **relativeUrl**: **$$Text.Format ('/ Mijn/rapport? maand = {0: yyyy}-{0:MM} & fmt csv =', slicestart-waarde)**. | Nee |
| requestMethod | De HTTP-methode. Toegestane waarden zijn **ophalen** en **POST**. | Nee <br />(de standaardwaarde is **ophalen**) |
| additionalHeaders | Extra kopteksten die HTTP-aanvraag. | Nee |
| RequestBody | De hoofdtekst van de HTTP-aanvraag. | Nee |
| Indeling | Als u wilt *de gegevens opgehaald uit een HTTP-eindpunt als-is* zonder deze parseren, gaat de **indeling** instelling. <br><br> Als u de inhoud van de HTTP-reactie parseren tijdens het kopiëren wilt, de volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, en **ParquetFormat**. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format). |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

**Voorbeeld: Met behulp van de methode GET (standaard)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Voorbeeld: Met behulp van de POST-methode**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [het maken van pijplijnen](data-factory-create-pipelines.md). 

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor een kopieeractiviteit variëren eigenschappen, afhankelijk van de typen van bronnen en sinks.

Op dit moment, wanneer de bron in de Kopieeractiviteit is van de **HttpSource** typt, worden de volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| httpRequestTimeout | De time-out (de **TimeSpan** waarde) voor de HTTP-aanvraag reageert. De time-out is een antwoord, niet de time-out voor lezen van gegevens van de reactie. | Nee<br />(standaardwaarde: **00:01:40**) |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestanden en compressie

Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) voor meer informatie.

## <a name="json-examples"></a>JSON-voorbeelden

De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De voorbeelden laten zien hoe gegevens kopiëren van een HTTP-bron naar Azure Blob-opslag. Echter, de gegevens kunnen worden gekopieerd *rechtstreeks* uit een van de bronnen aan een van de sinks [die worden ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

**Voorbeeld: Gegevens kopiëren van een HTTP-bron naar Azure Blob-opslag**

De Data Factory-oplossing voor dit voorbeeld bevat de volgende Data Factory-entiteiten:

*   Een gekoppelde service van het type [HTTP](#linked-service-properties).
*   Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
*   Invoer [gegevensset](data-factory-create-datasets.md) van het type [Http](#dataset-properties).
*   Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
*   Een [pijplijn](data-factory-create-pipelines.md) waarvoor een kopieeractiviteit die gebruikmaakt van [HttpSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een HTTP-bron naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de volgende secties de voorbeelden.

### <a name="http-linked-service"></a>HTTP-gekoppelde service

Dit voorbeeld wordt de HTTP-gekoppelde service met anonieme verificatie. Zie [HTTP gekoppelde service](#linked-service-properties) voor verschillende soorten verificatie die u kunt gebruiken.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Gekoppelde Azure storage-service

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
    }
  }
}
```

### <a name="http-input-dataset"></a>HTTP-invoergegevensset

Instellen van **externe** naar **waar** informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en is niet door een activiteit in de data factory gemaakt.

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Azure Blob-uitvoergegevensset

Gegevens worden geschreven naar een nieuwe blob elk uur (**frequentie**: **uur**, **interval**: **1**).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-that-uses-a-copy-activity"></a>Pijplijn met behulp van een kopieeractiviteit

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets. De kopieeractiviteit is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **HttpSource** en de **sink** type is ingesteld op **BlobSink**.

Voor de lijst met eigenschappen die **HttpSource** ondersteunt, Zie [HttpSource](#copy-activity-properties).

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with a copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

> [!NOTE]
> Zie het toewijzen van kolommen uit een brongegevensset op kolommen uit een sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen

Zie voor meer informatie over belangrijke factoren die invloed hebben op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren, de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md).