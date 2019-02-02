---
title: Gegevens verplaatsen van een FTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens van een ftp_server met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: adca66b46fad1220b49af327797cc4f91d216091
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564660"
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Gegevens verplaatsen van een FTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-ftp-connector.md)
> * [Versie 2 (huidige versie)](../connector-ftp.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [FTP-connector in V2](../connector-ftp.md).

In dit artikel wordt uitgelegd hoe u van de kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een FTP-server. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens kopiëren van een FTP-server naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data Factory ondersteunt momenteel alleen om gegevens te verplaatsen van een FTP-server naar andere gegevensarchieven, maar niet om gegevens te verplaatsen uit de andere gegevens worden opgeslagen met een FTP-server. Deze biedt ondersteuning voor zowel on-premises en FTP-servers in de cloud.

> [!NOTE]
> De kopieeractiviteit worden het bronbestand niet worden verwijderd nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopieerbewerking wilt, maken van een aangepaste activiteit als u wilt verwijderen van het bestand en het gebruik van de activiteit in de pijplijn.

## <a name="enable-connectivity"></a>Connectiviteit
Als u gegevens van verplaatst een **on-premises** FTP-server aan een cloud gegevens opslaan (bijvoorbeeld naar Azure Blob storage), installeren en gebruiken van Data Management Gateway. De Data Management Gateway is een clientagent die is geïnstalleerd op uw on-premises computer en maakt cloudservices verbinding maken met een on-premises resource. Zie voor meer informatie, [Data Management Gateway](data-factory-data-management-gateway.md). Voor stapsgewijze instructies voor het instellen-up van de gateway en u ziet [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md). U kunt de gateway verbinding maken met een FTP-server gebruiken, zelfs als de server zich op een Azure-infrastructuur als een service (IaaS) virtuele machine (VM).

Het is mogelijk de gateway installeren op dezelfde on-premises computer of IaaS-VM als de FTP-server. We raden echter aan dat u de gateway installeert op een afzonderlijke computer of op IaaS-VM om bronconflicten te voorkomen, en voor betere prestaties. Wanneer u de gateway op een afzonderlijke computer installeert, is de machine moet toegang hebben tot de FTP-server.

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van een FTP-bron verplaatst met behulp van verschillende hulpprogramma's of API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard van Data Factory**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar een FTP-gegevensarchief, de [JSON-voorbeeld: Gegevens kopiëren van de FTP-server naar Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) sectie van dit artikel.

> [!NOTE]
> Zie voor meer informatie over ondersteunde indelingen van bestanden en compressie gebruiken [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met FTP.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel beschrijft de JSON-elementen die specifiek zijn voor een FTP-gekoppelde service.

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| type |Stel dit in op FtpServer. |Ja |&nbsp; |
| host |Geef de naam of IP-adres van de FTP-server. |Ja |&nbsp; |
| authenticationType |Geef het verificatietype. |Ja |Basic-, anonieme |
| gebruikersnaam |De gebruiker die toegang tot de FTP-server heeft opgeven. |Nee |&nbsp; |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee |&nbsp; |
| encryptedCredential |Geef de versleutelde referenties voor toegang tot de FTP-server. |Nee |&nbsp; |
| gatewayName |Geef de naam van de gateway in de Data Management Gateway verbinding maken met een on-premises FTP-server. |Nee |&nbsp; |
| poort |Geef de poort waarop de FTP-server luistert. |Nee |21 |
| enableSsl |Geef op of u FTP via een SSL/TLS-kanaal. |Nee |true |
| enableServerCertificateValidation |Geef op of validatie van het servercertificaat SSL inschakelen wanneer u FTP via SSL/TLS-kanaal. |Nee |true |

>[!NOTE]
>De FTP-connector biedt ondersteuning voor toegang tot FTP-server zonder versleuteling of een expliciete SSL/TLS-versleuteling; het biedt geen ondersteuning voor impliciete SSL/TLS-versleuteling.

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

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Gebruik van gebruikersnaam en wachtwoord in tekst zonder opmaak voor basisverificatie

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

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>EncryptedCredential gebruiken voor verificatie en -gateway

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
Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, [gegevenssets maken](data-factory-create-datasets.md). Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van de gegevensset.

De **typeProperties** sectie verschilt voor elk type gegevensset. Het bevat informatie die specifiek is voor het gegevenssettype. De **typeProperties** sectie voor een gegevensset van het type **bestandsshare** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** mappaden op basis van het segment de status begin en einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer **fileName** niet is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand bevindt zich in de volgende indeling: <br/><br/>De gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in de **folderPath**, in plaats van alle bestanden.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: `"fileFilter": "*.log"`<br/>Voorbeeld 2: `"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** is van toepassing voor een invoergegevensset van de bestandsshare. Deze eigenschap wordt niet ondersteund met Hadoop Distributed File System (HDFS). |Nee |
| partitionedBy |Hiermee geeft u een dynamisch **folderPath** en **fileName** voor time series-gegevens. Bijvoorbeeld, kunt u een **folderPath** die voor elk uur gegevens met parameters. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie de [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling ](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt kopiëren van bestanden, omdat ze tussen winkels op basis van bestanden (binaire kopie zijn), slaat u het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**, en ondersteunde niveaus zijn **optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Geef op of de binaire overdrachtsmodus gebruiken. De waarden zijn true voor binaire modus (dit is de standaardwaarde), en ONWAAR voor ASCII. Deze eigenschap kan alleen worden gebruikt bij het type van de bijbehorende gekoppelde service is van het type: FtpServer. |Nee |

> [!NOTE]
> **fileName** en **fileFilter** kunnen niet gelijktijdig worden gebruikt.

### <a name="use-the-partionedby-property"></a>Gebruik de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamisch **folderPath** en **fileName** voor time series-gegevens met de **partitionedBy** eigenschap.

Zie voor meer informatie over time series-gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [het maken van pijplijnen](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Voorbeeld 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} wordt vervangen door de waarde van Data Factory systeemvariabele slicestart-waarde, in de opgegeven indeling (YYYYMMDDHH). De slicestart-waarde verwijst naar de begintijd van het segment. Pad naar de map verschilt voor elk segment. (Bijvoorbeeld wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.)

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
In dit voorbeeld wordt het jaar, maand, dag en tijd van de slicestart-waarde zijn uitgepakt naar afzonderlijke variabelen die worden gebruikt door de **folderPath** en **fileName** eigenschappen.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, [het maken van pijplijnen](data-factory-create-pipelines.md). Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit, daarentegen, variëren met elk activiteitstype. Voor de kopieeractiviteit wordt de type-eigenschappen variëren afhankelijk van de typen van bronnen en sinks.

In de kopieeractiviteit, wanneer de bron van het type **FileSystemSource**, de volgende eigenschap is beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen, of alleen voor de opgegeven map. |True, False (standaard) |Nee |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van de FTP-server naar Azure Blob
Dit voorbeeld laat zien hoe u gegevens kopiëren van een FTP-server naar Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de sinks vermeld in de de [ondersteunde gegevensarchieven en indelingen](data-factory-data-movement-activities.md#supported-data-stores-and-formats), met behulp van de kopieeractiviteit in Data Factory.

De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* Een gekoppelde service van het type [FtpServer](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [bestandsshare](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

Het voorbeeld worden gegevens gekopieerd van een FTP-server naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

### <a name="ftp-linked-service"></a>FTP-gekoppelde service

In dit voorbeeld maakt gebruik van basisverificatie, met de gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken:

* Anonieme verificatie
* Basisverificatie wordt gebruikt met de versleutelde referenties
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

Deze gegevensset verwijst naar de FTP-map `mysharedfolder` en de bestandsnaam `test.csv`. De pijplijn wordt het bestand gekopieerd naar de bestemming.

Instellen van **externe** naar **waar** informeert de Data Factory-service de dataset bevindt zich buiten de data factory en wordt niet gemaakt door een activiteit in de data factory.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur onderdelen van de begintijd.

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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Een kopieeractiviteit in een pijplijn met de bestand system bron- en blob-sink

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **FileSystemSource**, en de **sink** type is ingesteld op **BlobSink**.

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
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* Zie voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (kopieeractiviteit) in Data Factory en de verschillende manieren om te optimaliseren, de [en afstemmingshandleiding van activiteit kopiëren](data-factory-copy-activity-performance.md).

* Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
