---
title: Gegevens verplaatsen van de SFTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens van een on-premises of een SFTP-server van cloud met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f57175598d3d63afcc7fd050fe5aaedd62e778fe
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746401"
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Gegevens verplaatsen van een SFTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-sftp-connector.md)
> * [Versie 2 (huidige versie)](../connector-sftp.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [SFTPconnector in V2](../connector-sftp.md).

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises/cloudtoepassing SFTP-server naar een ondersteunde sink-gegevensarchief. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met daarin een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit en de lijst met gegevensarchieven die worden ondersteund als bronnen/Put.

Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een SFTP-server naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een SFTP-server. Deze biedt ondersteuning voor zowel on-premises servers en cloudservers SFTP.

> [!NOTE]
> Kopieeractiviteit worden het bronbestand niet worden verwijderd nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopieerbewerking wilt, maakt u een aangepaste activiteit wilt verwijderen van het bestand en het gebruik van de activiteit in de pijplijn.

## <a name="supported-scenarios-and-authentication-types"></a>Ondersteunde scenario's en verificatietypen
U kunt deze SFTP-connector gebruiken om te kopiëren van gegevens uit **zowel cloud SFTP en on-premises SFTP servers**. **Basic** en **SshPublicKey** verificatietypen worden ondersteund bij het verbinden met de SFTP-server.

Het kopiëren van gegevens uit een on-premises SFTP-server, moet u een Data Management Gateway installeren in de on-premises omgeving/Azure-VM. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over de gateway. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies voor het instellen van de gateway en het gebruik van deze.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van een SFTP-bron verplaatst met behulp van verschillende hulpprogramma's / API's.

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. Zie voor JSON-voorbeelden om gegevens te kopiëren van de SFTP-server naar Azure Blob Storage, [JSON-voorbeeld: Gegevens kopiëren van de SFTP-server naar Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) sectie van dit artikel.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de FTP-gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- | --- |
| type | De eigenschap type moet worden ingesteld op `Sftp`. |Ja |
| host | Naam of IP-adres van de SFTP-server. |Ja |
| poort |De poort waarop de SFTP-server luistert. De standaardwaarde is: 21 |Nee |
| authenticationType |Geef het verificatietype. Toegestane waarden: **Basic**, **SshPublicKey**. <br><br> Raadpleeg [met basisverificatie](#using-basic-authentication) en [met behulp van SSH openbare sleutelverificatie](#using-ssh-public-key-authentication) respectievelijk de secties over meer eigenschappen en voorbeelden voor JSON. |Ja |
| skipHostKeyValidation | Geef op of moet worden overgeslagen sleutelvalidatie voor de host. | Nee. De standaardwaarde: false |
| hostKeyFingerprint | Geef de vingerafdruk van de hostsleutel. | Ja als de `skipHostKeyValidation` is ingesteld op false.  |
| gatewayName |De naam van de Data Management Gateway verbinding maken met een on-premises SFTP-server. | Ja als het kopiëren van gegevens uit een on-premises SFTP-server. |
| encryptedCredential | Versleutelde referenties voor toegang tot de SFTP-server. Automatisch gegenereerde wanneer u basisverificatie (gebruikersnaam en wachtwoord) of SshPublicKey verificatie (gebruikersnaam + pad naar de persoonlijke sleutel of inhoud) in de wizard kopiëren of de ClickOnce-pop-dialoogvenster opgeeft. | Nee. Gelden alleen wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie

Als u wilt gebruikmaken van basisverificatie instellen `authenticationType` als `Basic`, en geeft u de volgende eigenschappen naast de algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Description | Vereist |
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Voorbeeld: Basisverificatie wordt gebruikt met de versleutelde referentie

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

Voor het gebruik van SSH-verificatie voor openbare sleutel instellen `authenticationType` als `SshPublicKey`, en geeft u de volgende eigenschappen naast de algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Description | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam |Gebruikers die toegang tot de SFTP-server heeft |Ja |
| privateKeyPath | Geef het absolute pad naar het persoonlijke sleutelbestand dat de gateway toegankelijk. | Opgeven of de `privateKeyPath` of `privateKeyContent`. <br><br> Gelden alleen wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |
| privateKeyContent | Een geserialiseerde tekenreeks van de inhoud van de persoonlijke sleutel. De Wizard kopiëren kunt lezen van het bestand met persoonlijke sleutel en de persoonlijke sleutel inhoud automatisch uit te pakken. Als u van andere hulpprogramma/SDK gebruikmaakt, in plaats daarvan de eigenschap privateKeyPath gebruiken. | Opgeven of de `privateKeyPath` of `privateKeyContent`. |
| passPhrase | Geef de pass-woordgroep en wachtwoord in voor het ontsleutelen van de persoonlijke sleutel als bestand met de sleutel is beveiligd met een wachtwoordzin. | Ja als het bestand met persoonlijke sleutel wordt beveiligd door een wachtwoordzin. |

> [!NOTE]
> SFTP-connector ondersteunt RSA/DSA OpenSSH-sleutel. Zorg ervoor dat uw inhoud sleutelbestand begint met '---BEGIN [RSA/DSA] PRIVATE KEY---'. Als het bestand met persoonlijke sleutel een bestand ppk-indeling is, gebruikt u de Putty hulpprogramma converteren naar ppk OpenSSH-indeling.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Voorbeeld: SshPublicKey verificatie met behulp van de persoonlijke sleutel filePath

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Voorbeeld: SshPublicKey verificatie met behulp van de persoonlijke sleutel inhoud

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
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van de gegevensset.

De **typeProperties** sectie verschilt voor elk type gegevensset. Het bevat informatie die specifiek is voor het gegevenssettype. De typeProperties voor een gegevensset van het type sectie **bestandsshare** gegevensset heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: <br/><br/>De gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in het mappad in plaats van alle bestanden.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: `"fileFilter": "*.log"`<br/>Voorbeeld 2: `"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter is van toepassing voor een invoergegevensset van de bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor time series-gegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Opgeven of binaire overdrachtsmodus gebruiken. De waarde True voor binaire modus en de waarde false ASCII. Standaardwaarde: De waarde True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

### <a name="using-partionedby-property"></a>Met behulp van de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath, filename voor time series-gegevens met partitionedBy opgeven. U kunt dit doen met de Data Factory-macro's en de systeemvariabele slicestart-waarde, SliceEnd die wijzen op de logische periode voor een bepaalde gegevenssegment.

Zie voor meer informatie over time series-gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} wordt vervangen door de opgegeven waarde van de Data Factory systeemvariabele slicestart-waarde in de notatie (YYYYMMDDHH). De slicestart-waarde verwijst naar de begintijd van het segment. FolderPath verschilt voor elk segment. Voorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

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
Jaar, maand, dag en tijd van de slicestart-waarde in dit voorbeeld zijn uitgepakt naar afzonderlijke variabelen die worden gebruikt door de eigenschappen voor folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Voor de kopieeractiviteit, wordt de type-eigenschappen variëren afhankelijk van de typen van bronnen en sinks.

[!INCLUDE [data-factory-file-system-source](../../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestanden en compressie
Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van de SFTP-server naar Azure blob
Het volgende voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren van de SFTP-gegevensbron naar Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> In dit voorbeeld bevat JSON-fragmenten. Deze omvatten geen stapsgewijze instructies voor het maken van de data factory. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

* Een gekoppelde service van het type [sftp](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [bestandsshare](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een SFTP-server naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**SFTP gekoppelde service**

Dit voorbeeld wordt de basisverificatie met gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken:

* Basisverificatie wordt gebruikt met de versleutelde referenties
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
**SFTP-invoergegevensset**

Deze gegevensset verwijst naar de SFTP-map `mysharedfolder` en de bestandsnaam `test.csv`. De pijplijn wordt het bestand gekopieerd naar de bestemming.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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

**Pijplijn met kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**.

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

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* [Zelfstudie kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een Kopieeractiviteit.
