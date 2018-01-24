---
title: Gegevens verplaatsen van een FTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens van een FTP-server met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: daf865ef33e2b099e01f4647b17f36ca8df92c94
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Gegevens verplaatsen van een FTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-ftp-connector.md)
> * [Versie 2 - Preview](../connector-ftp.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [FTP-connector in V2](../connector-ftp.md).

Dit artikel wordt uitgelegd hoe u de kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een FTP-server. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een FTP-server kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data Factory ondersteunt momenteel alleen zwevend gegevens van een FTP-server naar andere gegevensarchieven, maar niet verplaatsen van gegevens van andere gegevens worden opgeslagen met een FTP-server. Deze ondersteuning biedt voor zowel on-premises en in de cloud van de FTP-servers.

> [!NOTE]
> De kopieeractiviteit verwijdert het bronbestand niet nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopie wilt, maakt u een aangepaste activiteit voor het verwijderen van het bestand en gebruikt u de activiteit in de pijplijn. 

## <a name="enable-connectivity"></a>Connectiviteit inschakelen
Als u overstapt gegevens uit een **lokale** FTP-server aan een cloud gegevens opslaan (bijvoorbeeld naar Azure Blob-opslag), installeren en gebruiken van Data Management Gateway. Data Management Gateway is een clientagent die is geïnstalleerd op uw on-premises machine en cloudservices voor verbinding met een on-premises resource maakt. Zie voor meer informatie [Data Management Gateway](data-factory-data-management-gateway.md). Voor stapsgewijze instructies voor het instellen de gateway boven en gebruik, Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md). U kunt de gateway verbinding maken met een FTP-server, zelfs als de server zich op een Azure-infrastructuur als een dienst (IaaS) virtuele machine (VM).

Het is mogelijk voor het installeren van de gateway op de lokale machine of de IaaS VM als de FTP-server. We raden echter aan dat u de gateway installeren op een afzonderlijke computer of op IaaS VM om bronconflicten te voorkomen, en voor betere prestaties. Wanneer u de gateway op een afzonderlijke computer installeert, is de machine moet toegang hebben tot de FTP-server.

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens van een FTP-bron verplaatst met behulp van verschillende hulpprogramma's of API's.

De eenvoudigste manier om een pijplijn maken is met de **Data Factory-Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren van een FTP-gegevensarchief de [JSON-voorbeeld: gegevens kopiëren van de FTP-server naar Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) sectie van dit artikel.

> [!NOTE]
> Zie voor meer informatie over ondersteunde indelingen voor de bestands- en compressie te gebruiken, [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor FTP.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel beschrijft de JSON-elementen die specifiek zijn voor een FTP-gekoppelde service.

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| type |Stel dit in op FtpServer. |Ja |&nbsp; |
| host |Geef de naam of IP-adres van de FTP-server. |Ja |&nbsp; |
| authenticationType |Geef het verificatietype. |Ja |Basic, anonieme |
| gebruikersnaam |De gebruiker die toegang tot de FTP-server heeft opgeven. |Nee |&nbsp; |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee |&nbsp; |
| encryptedCredential |Geef de versleutelde referenties voor toegang tot de FTP-server. |Nee |&nbsp; |
| gatewayName |Geef de naam van de gateway in Data Management Gateway verbinding maken met een on-premises FTP-server. |Nee |&nbsp; |
| poort |Geef de poort waarop de FTP-server luistert. |Nee |21 |
| enableSsl |Geef op of FTP gebruiken via een SSL/TLS-kanaal. |Nee |waar |
| enableServerCertificateValidation |Geef op of validatie van het servercertificaat SSL inschakelen wanneer u FTP via SSL/TLS-kanaal. |Nee |waar |

### <a name="use-anonymous-authentication"></a>Gebruik anonieme verificatie

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Gebruik van gebruikersnaam en wachtwoord voor basisverificatie in tekst zonder opmaak

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Gebruik poort, enableSsl, enableServerCertificateValidation

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>EncryptedCredential gebruiken voor verificatie en gateway

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets [gegevenssets maken](data-factory-create-datasets.md). Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset.

De **typeProperties** sectie verschilt voor elk type dataset. Het levert informatie die specifiek is voor het type dataset. De **typeProperties** sectie voor een gegevensset van het type **FileShare** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** mappaden op basis van het segment start en eindtijden datum. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand is in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Geef een filter om te worden gebruikt voor het selecteren van een subset van de bestanden in de **folderPath**, in plaats van alle bestanden.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeeld 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** is van toepassing op een bestandsshare-invoergegevensset. Deze eigenschap wordt niet ondersteund met Hadoop Distributed File System (HDFS). |Nee |
| partitionedBy |Hiermee geeft u een dynamische **folderPath** en **fileName** voor tijd reeksgegevens. Bijvoorbeeld, kunt u een **folderPath** met parameters die voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie de [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt kopiëren van bestanden, omdat ze tussen winkels op basis van bestanden (binaire kopiëren), slaat u de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**, en ondersteunde niveaus zijn **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Geef op of de binaire overdrachtsmodus gebruiken. De waarden zijn true zijn voor binaire modus (dit is de standaardwaarde) en false voor ASCII. Deze eigenschap kan alleen worden gebruikt als het type van de bijbehorende gekoppelde service van het type: FtpServer. |Nee |

> [!NOTE]
> **Bestandsnaam** en **fileFilter** niet gelijktijdig worden gebruikt.

### <a name="use-the-partionedby-property"></a>Gebruik de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische **folderPath** en **fileName** voor time series-gegevens met de **partitionedBy** eigenschap.

Zie voor meer informatie over tijd reeks gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Voorbeeld 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} is vervangen door de waarde van de Data Factory systeemvariabele SliceStart, in de opgegeven indeling (YYYYMMDDHH). De SliceStart verwijst voor het starten van de tijd van het segment. Het mappad verschilt voor elk segment. (Bijvoorbeeld wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.)

#### <a name="sample-2"></a>Voorbeeld 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
In dit voorbeeld wordt het jaar, maand, dag en tijd van de SliceStart worden uitgepakt in verschillende variabelen die worden gebruikt door de **folderPath** en **fileName** eigenschappen.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten [pijplijnen maken](data-factory-create-pipelines.md). Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit aan de andere kant variëren met elk activiteitstype. Voor de kopieeractiviteit wordt de type-eigenschappen variëren afhankelijk van de typen van bronnen en Put.

In de kopieeractiviteit, wanneer de bron van het type **FileSystemSource**, de volgende eigenschap is beschikbaar in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen, of alleen uit de opgegeven map. |True, False (standaard) |Nee |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van de FTP-server naar Azure Blob
Dit voorbeeld laat zien hoe gegevens kopiëren van een FTP-server naar Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de PUT vermeld in de [ondersteunde gegevensarchieven en indelingen](data-factory-data-movement-activities.md#supported-data-stores-and-formats), met behulp van de kopieeractiviteit in Data Factory.  

De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* Een gekoppelde service van het type [FtpServer](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [bestandsshare](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

Het voorbeeld kopieert gegevens van een FTP-server naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

### <a name="ftp-linked-service"></a>Gekoppelde FTP-service

Basisverificatie, wordt dit voorbeeld met de gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken:

* Anonieme verificatie
* Basisverificatie met versleutelde referenties
* FTP via SSL/TLS (FTPS)

Zie de [FTP gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
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
### <a name="ftp-input-dataset"></a>FTP-invoergegevensset

Deze gegevensset verwijst naar de FTP-map `mysharedfolder` en de bestandsnaam `test.csv`. De pijplijn kopieert het bestand naar de bestemming.

Instelling **externe** naar **true** informeert de Data Factory-service in de gegevensset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob-uitvoergegevensset

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur delen van de begintijd.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Een kopieeractiviteit in een pijplijn met systeem bron- en blob bestandssink

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **FileSystemSource**, en de **sink** type is ingesteld op **BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* Zie voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (kopieeractiviteit) in de Data Factory en verschillende manieren om te optimaliseren, de [activiteit prestaties en prestatieafstemming handleiding kopiëren](data-factory-copy-activity-performance.md).

* Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
