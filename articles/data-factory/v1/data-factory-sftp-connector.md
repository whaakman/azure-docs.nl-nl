---
title: Verplaatsen van gegevens van de SFTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een on-premises of in een cloud SFTP-server met behulp van Azure Data Factory.
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
ms.openlocfilehash: 3485120347923dd291663962d528a1e5996b477f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Gegevens verplaatsen van een SFTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-sftp-connector.md)
> * [Versie 2 - Preview](../connector-sftp.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [SFTPconnector in V2](../connector-sftp.md).

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises/cloudtoepassing SFTP-server naar een ondersteunde sink-gegevensopslag. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met daarin een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit en de lijst met gegevensarchieven ondersteund als bronnen/Put.

Data factory ondersteunt momenteel alleen zwevend gegevens van een server SFTP tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een SFTP-server. Deze ondersteuning biedt voor zowel on-premises en in de cloud SFTP-servers.

> [!NOTE]
> Kopieeractiviteit verwijdert het bronbestand niet nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopie wilt, maakt u een aangepaste activiteit voor het bestand verwijderen en het gebruik van de activiteit in de pijplijn. 

## <a name="supported-scenarios-and-authentication-types"></a>Ondersteunde scenario's en verificatietypen
U kunt deze SFTP-connector gebruiken voor het kopiëren van gegevens van **zowel cloud SFTP-servers en on-premises SFTP-servers**. **Basic** en **SshPublicKey** authenticatietypen worden ondersteund bij het verbinden met de SFTP-server.

Als u gegevens uit een on-premises SFTP-server, moet u een Data Management Gateway installeren in de lokale omgeving/Azure VM. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over de gateway. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor stapsgewijze instructies voor het instellen van de gateway en het gebruik van het artikel.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een bron SFTP verplaatst met behulp van verschillende hulpprogramma's voor API's.

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. Zie voor voorbeelden van JSON gegevens gekopieerd van de SFTP-server naar Azure Blob Storage, [JSON-voorbeeld: gegevens kopiëren van de SFTP-server naar Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) sectie van dit artikel.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de FTP-gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| type | De eigenschap type moet worden ingesteld op `Sftp`. |Ja |
| host | Naam of IP-adres van de SFTP-server. |Ja |
| poort |De poort waarop de SFTP-server luistert. De standaardwaarde is: 21 |Nee |
| authenticationType |Geef het verificatietype. Toegestane waarden: **Basic**, **SshPublicKey**. <br><br> Raadpleeg [Using basisverificatie](#using-basic-authentication) en [met behulp van SSH openbare-sleutelauthenticatie](#using-ssh-public-key-authentication) respectievelijk secties over meer eigenschappen en voorbeelden van JSON. |Ja |
| skipHostKeyValidation | Geef op of validatie host-sleutel overslaan. | Nee. De standaardwaarde: false |
| hostKeyFingerprint | Geef de vingerafdruk van de hostsleutel. | Ja als de `skipHostKeyValidation` is ingesteld op false.  |
| gatewayName |Naam van de Data Management Gateway verbinding maken met een on-premises SFTP-server. | Ja als u kopiëren van gegevens uit een on-premises SFTP-server. |
| encryptedCredential | Versleutelde referentie voor de toegang tot de SFTP-server. Automatisch wordt gegenereerd wanneer u basisverificatie (gebruikersnaam en wachtwoord) of SshPublicKey verificatie (gebruikersnaam + pad naar de persoonlijke sleutel of inhoud) in de wizard kopiëren of de ClickOnce-pop-dialoogvenster opgeven. | Nee. Alleen van toepassing wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie

Met basisverificatie, stelt `authenticationType` als `Basic`, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam | De gebruiker die toegang tot de SFTP-server heeft. |Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). | Ja |

#### <a name="example-basic-authentication"></a>Voorbeeld: Basisverificatie
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Voorbeeld: Basisverificatie met versleutelde referentie

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>Met behulp van SSH-verificatie voor openbare sleutel

Voor het gebruik van SSH-verificatie voor openbare sleutel instellen `authenticationType` als `SshPublicKey`, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam |Gebruiker die toegang tot de SFTP-server heeft |Ja |
| privateKeyPath | Geef het absolute pad naar het persoonlijke sleutelbestand dat gateway toegankelijk. | Geef ofwel de `privateKeyPath` of `privateKeyContent`. <br><br> Alleen van toepassing wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |
| privateKeyContent | Een geserialiseerde tekenreeks van de inhoud van de persoonlijke sleutel. De Wizard kopiëren kan lezen van bestand met de persoonlijke sleutel en pak de inhoud van de persoonlijke sleutel automatisch. Als u van andere hulpprogramma/SDK gebruikmaakt, in plaats daarvan de eigenschap privateKeyPath gebruiken. | Geef ofwel de `privateKeyPath` of `privateKeyContent`. |
| passPhrase | Geef de pass woordgroep en het wachtwoord voor het ontsleutelen van de persoonlijke sleutel als het sleutelbestand is beveiligd met een wachtwoordzin. | Ja als u een bestand met de persoonlijke sleutel wordt beveiligd door een wachtwoordzin. |

> [!NOTE]
> SFTP-connector ondersteunen alleen OpenSSH-sleutel. Zorg ervoor dat uw sleutelbestand in de juiste indeling. U kunt Putty hulpprogramma ppk converteren naar OpenSSH-indeling.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Voorbeeld: SshPublicKey verificatie met behulp van persoonlijke sleutels filePath

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Voorbeeld: SshPublicKey verificatie met behulp van persoonlijke sleutels inhoud

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset.

De **typeProperties** sectie verschilt voor elk type dataset. Het levert informatie die specifiek is voor het type dataset. De typeProperties sectie voor een gegevensset van het type **FileShare** gegevensset heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeelden 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter geldt voor een invoergegevensset bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Geef binaire overdrachtsmodus of gebruiken. De waarde True voor binaire modus en ONWAAR ASCII. Standaardwaarde: True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde-servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.

### <a name="using-partionedby-property"></a>Gebruik de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath, filename voor time series-gegevens met partitionedBy opgeven. U kunt dit doen met de Data Factory-macro's en de systeemvariabele SliceStart, SliceEnd die wijzen op de logische periode voor een bepaalde gegevenssegment.

Zie voor meer informatie over tijd reeks gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} is vervangen door de opgegeven waarde van de Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst voor het starten van de tijd van het segment. FolderPath verschilt voor elk segment. Voorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

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
In dit voorbeeld worden jaar, maand, dag en tijd van de SliceStart uitgepakt in verschillende variabelen die worden gebruikt door de eigenschappen voor folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit met elk activiteitstype variëren. Voor de kopieeractiviteit, wordt de type-eigenschappen variëren afhankelijk van de typen van bronnen en Put.

[!INCLUDE [data-factory-file-system-source](../../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestands- en compressie
Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van SFTP-server naar Azure blob
Het volgende voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren van bron SFTP naar Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> Dit voorbeeld bevat JSON-fragmenten. Dit omvat geen stapsgewijze instructies voor het maken van de gegevensfactory. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

* Een gekoppelde service van het type [sftp](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [FileShare](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert gegevens van een SFTP-server naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde SFTP-service**

In dit voorbeeld gebruikt de basisverificatie met gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken:

* Basisverificatie met versleutelde referenties
* SSH-verificatie voor openbare sleutel

Zie [FTP gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Een gekoppelde Azure Storage-service**

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
**Invoergegevensset SFTP**

Deze gegevensset verwijst naar de map SFTP `mysharedfolder` en de bestandsnaam `test.csv`. De pijplijn kopieert het bestand naar de bestemming.

Instelling 'extern': 'true' informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pijplijn met de kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* [Zelfstudie voor kopiëren-activiteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een Kopieeractiviteit.
