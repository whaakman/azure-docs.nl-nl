---
title: Gegevens verplaatsen van een HTTP-bron - Azure | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een on-premises of een HTTP-bron van cloud met Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: a6dad8242c709240b57b8a47acc44c5ddfdaa755
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Gegevens verplaatsen van een HTTP-bron met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-http-connector.md)
> * [Versie 2 - Preview](../connector-http.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [HTTP-connector in V2](../connector-http.md).


In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises/cloudtoepassing HTTP-eindpunt met een ondersteunde sink-gegevensarchief. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met daarin een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit en de lijst met gegevensarchieven ondersteund als bronnen/Put.

Data factory ondersteunt momenteel alleen zwevend gegevens van een HTTP-bron naar andere gegevensarchieven, maar niet verplaatsen van gegevens van andere gegevens worden opgeslagen op een HTTP-doel.

## <a name="supported-scenarios-and-authentication-types"></a>Ondersteunde scenario's en verificatietypen
U kunt deze connector HTTP-gegevens ophalen van **zowel cloud als on-premises HTTP/s-eindpunt** met behulp van HTTP **ophalen** of **POST** methode. De volgende verificatietypen worden ondersteund: **anoniem**, **Basic**, **Digest**, **Windows**, en **ClientCertificate**. Noteer het verschil tussen deze connector en de [Web tabel connector](data-factory-web-table-connector.md) is: de laatste tabelinhoud ophalen uit webpagina HTML wordt gebruikt.

Bij het kopiëren van gegevens uit een lokale HTTP-eindpunt, moet u een Data Management Gateway installeren in de lokale omgeving/Azure VM. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens van een HTTP-bron verplaatst met behulp van verschillende hulpprogramma's voor API's.

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. Zie voor voorbeelden van JSON gegevens van HTTP-bron kopiëren naar Azure Blob Storage, [JSON voorbeelden](#json-examples) sectie van dit artikel.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat de beschrijving voor JSON-elementen die specifiek zijn voor HTTP gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type | De eigenschap type moet worden ingesteld op: `Http`. | Ja |
| url | Basis-URL voor de webserver | Ja |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn: **anoniem**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Raadpleeg de secties onder deze tabel over meer eigenschappen en voorbeelden voor deze verificatietypen JSON respectievelijk. | Ja |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen als bron is een HTTPS-webserver | Nee, de standaardwaarde is true |
| gatewayName | Naam van de Data Management Gateway verbinding maken met een lokale HTTP-bron. | Ja als u gegevens kopiëren van een lokale HTTP-bron. |
| encryptedCredential | Versleutelde referentie voor toegang tot het HTTP-eindpunt. Automatisch wordt gegenereerd wanneer u de verificatie-informatie in de wizard kopiëren of de ClickOnce-pop-dialoogvenster configureert. | Nee. Alleen van toepassing wanneer het kopiëren van gegevens uit een lokale HTTP-server. |

Zie [gegevens verplaatsen tussen lokale bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie over het instellen van referenties voor on-premises HTTP connector-gegevensbron.

### <a name="using-basic-digest-or-windows-authentication"></a>Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

Stel `authenticationType` als `Basic`, `Digest`, of `Windows`, en geef de volgende eigenschappen naast de HTTP-connector algemene resources die hierboven zijn geïntroduceerd:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| gebruikersnaam | Gebruikersnaam voor toegang tot het HTTP-eindpunt. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). | Ja |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Voorbeeld: met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

```JSON
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

### <a name="using-clientcertificate-authentication"></a>ClientCertificate-verificatie

Met basisverificatie, stelt `authenticationType` als `ClientCertificate`, en geef de volgende eigenschappen naast de HTTP-connector algemene resources die hierboven zijn geïntroduceerd:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| embeddedCertData | De Base64-gecodeerde inhoud van de binaire gegevens van het bestand (Personal Information Exchange (PFX). | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op de gatewaycomputer certificaatarchief. Alleen van toepassing wanneer het kopiëren van gegevens van een lokale HTTP-bron. | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| wachtwoord | Het wachtwoord is gekoppeld aan het certificaat. | Nee |

Als u `certThumbprint` voor verificatie en het certificaat in het persoonlijke archief van de lokale computer is geïnstalleerd, moet u de machtiging lezen voor de gateway-service:

1. Start Microsoft Management Console (MMC). Voeg de **certificaten** -module die gericht is op de **lokale Computer**.
2. Vouw **certificaten**, **persoonlijke**, en klik op **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief en selecteer **alle taken**->**persoonlijke sleutels beheren...**
3. Op de **beveiliging** tabblad, het toevoegen van het gebruikersaccount waaronder Data Management Gateway Host Service wordt uitgevoerd met leestoegang tot het certificaat.  

#### <a name="example-using-client-certificate"></a>Voorbeeld: met behulp van clientcertificaat
Deze gekoppelde service wordt uw gegevensfactory op een lokale HTTP-webserver. Dit maakt gebruik van een clientcertificaat dat is geïnstalleerd op de machine met Data Management Gateway is geïnstalleerd.

```JSON
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

#### <a name="example-using-client-certificate-in-a-file"></a>Voorbeeld: clientcertificaat gebruiken in een bestand
Deze gekoppelde service wordt uw gegevensfactory op een lokale HTTP-webserver. Een certificaatbestand van de client op de machine wordt met Data Management Gateway is geïnstalleerd.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **Http** heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | Het type van de gegevensset zijn opgegeven. moet worden ingesteld op `Http`. | Ja |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat. Als het pad niet wordt opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. <br><br> Kan de dynamische-URL, kunt u [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md), bijvoorbeeld 'relativeUrl': ' $$Text.Format ('/ Mijn/rapport? maand = {0:yyyy}-{0:MM} & fmt csv =', SliceStart) '. | Nee |
| requestMethod | HTTP-methode. Toegestane waarden zijn **ophalen** of **POST**. | Nee. Standaard is `GET`. |
| additionalHeaders | Aanvullende HTTP-aanvraagheaders. | Nee |
| requestBody | Instantie voor HTTP-aanvraag. | Nee |
| Indeling | Als u gewoon wilt **gegevens ophalen van HTTP-eindpunt als-is** overslaan zonder deze parseren, deze instellingen. <br><br> Als u de inhoud van het HTTP-antwoord geparseerd tijdens het kopiëren wilt, de volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

### <a name="example-using-the-get-default-method"></a>Voorbeeld: met behulp van de methode GET (standaard)

```JSON
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

### <a name="example-using-the-post-method"></a>Voorbeeld: via de POST-methode

```JSON
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
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit aan de andere kant variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer de bron is in de kopieerbewerking is momenteel van het type **HttpSource**, de volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| httpRequestTimeout | De time-out (TimeSpan) voor de HTTP-aanvragen reageert. De time-out is reageert niet op de time-out antwoordgegevens lezen. | Nee. Standaardwaarde: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestands- en compressie
Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-examples"></a>JSON-voorbeelden
Het volgende voorbeeld leveren voorbeeld JSON definities die u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van HTTP-bron kopiëren naar Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a>Voorbeeld: Gegevens kopiëren van HTTP-bron naar Azure Blob-opslag
De Data Factory-oplossing voor dit voorbeeld bevat de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [HTTP](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [Http](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [HttpSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een HTTP-bron met een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

### <a name="http-linked-service"></a>Gekoppelde HTTP-service
Dit voorbeeld wordt de HTTP-gekoppelde service met anonieme verificatie. Zie [HTTP gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```JSON
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

### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="http-input-dataset"></a>HTTP-invoergegevensset
Instelling **externe** naar **true** informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1).

```JSON
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

### <a name="pipeline-with-copy-activity"></a>Pijplijn met de kopieeractiviteit

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **HttpSource** en **sink** type is ingesteld op **BlobSink**.

Zie [HttpSource](#copy-activity-properties) voor de lijst met eigenschappen die door de HttpSource ondersteund.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
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
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
