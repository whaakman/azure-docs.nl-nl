---
title: Gegevens verplaatsen van on-premises HDFS | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit on-premises HDFS met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: ac9ba682079f735aa2fdd416070c5d206d526ad4
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629698"
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Gegevens verplaatsen van on-premises HDFS met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-hdfs-connector.md)
> * [Versie 2 (huidige versie)](../connector-hdfs.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [HDFS-connector in V2](../connector-hdfs.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises HDFS. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit HDFS kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een on-premises HDFS naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een on-premises HDFS.

> [!NOTE]
> Kopieeractiviteit worden het bronbestand niet worden verwijderd nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopieerbewerking wilt, maakt u een aangepaste activiteit wilt verwijderen van het bestand en het gebruik van de activiteit in de pijplijn. 

## <a name="enabling-connectivity"></a>Verbindingen inschakelen
Data Factory-service ondersteunt een verbinding met on-premises HDFS met behulp van de Data Management Gateway. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway. De gateway gebruiken om te verbinden met HDFS, zelfs als deze wordt gehost in een Azure IaaS-VM.

> [!NOTE]
> Zorg ervoor dat de Data Management Gateway kan toegang tot **alle** de [naam van knooppunt server]: [knooppunt poort name] en [servers voor het knooppunt van gegevens]: [knooppunt gegevenspoort] van het Hadoop-cluster. Standaard [naam van knooppunt poort] is 50070 en standaard [knooppunt gegevenspoort] 50075 is.

U kunt de gateway installeren op de dezelfde on-premises computer of de Azure-VM als de HDFS, we raden aan dat u de gateway op een afzonderlijke computer/Azure IaaS VM installeren. Met gateway op een afzonderlijke computer vermindert conflicten tussen resources en verbetert de prestaties. Wanneer u de gateway op een afzonderlijke computer installeert, is de machine moet toegang hebben tot de machine met het HDFS.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van een HDFS-bron verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon **, **.NET API**, en **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een gegevensarchief HDFS, [JSON-voorbeeld: gegevens kopiëren van on-premises HDFS naar Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor HDFS:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een gekoppelde service verbindt een gegevensopslag op een data factory. U maakt een gekoppelde service van het type **Hdfs** naar een on-premises HDFS koppelen aan uw gegevensfactory. De volgende tabel bevat de beschrijving voor JSON-elementen die specifiek zijn voor HDFS gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Hdfs** |Ja |
| URL |URL naar het HDFS |Ja |
| authenticationType |Anoniem, of Windows. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector, raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| Gebruikersnaam |Gebruikersnaam voor Windows-verificatie. Geef voor Kerberos-verificatie, `<username>@<domain>.com`. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met het HDFS. |Ja |
| encryptedCredential |[Nieuwe AzureRMDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/azurerm.datafactories/new-azurermdatafactoryencryptvalue) uitvoer van de referentie op die toegang. |Nee |

### <a name="using-anonymous-authentication"></a>Met behulp van anonieme verificatie

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Met behulp van Windows-verificatie

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **bestandsshare** (waaronder HDFS gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de map. Voorbeeld: `myfolder`<br/><br/>Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: voor folder\subfolder, geeft u de map\\\\submap en geef voor de d:\samplefolder, d:\\\\Voorbeeldmap.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: <br/><br/>De gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor time series-gegevens. Voorbeeld: folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ** ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

### <a name="using-partionedby-property"></a>Met behulp van de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath en de bestandsnaam voor time series-gegevens met de **partitionedBy** eigenschap [Data Factory-functies en de systeemvariabelen](data-factory-functions-variables.md).

Zie voor meer informatie over time series-gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} wordt vervangen door de opgegeven waarde van de Data Factory systeemvariabele slicestart-waarde in de notatie (YYYYMMDDHH). De slicestart-waarde verwijst naar de begintijd van het segment. FolderPath verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

```JSON
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

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Voor de Kopieeractiviteit, wanneer de gegevensbron van het type **FileSystemSource** de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestanden en compressie
Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Voorbeeld van JSON: gegevens kopiëren van on-premises HDFS naar Azure Blob
Dit voorbeeld laat zien hoe u gegevens kopiëren van een on-premises HDFS naar Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

Het voorbeeld bevat JSON-definities voor de volgende Data Factory-entiteiten. U kunt deze definities om te maken van een pijplijn om gegevens te kopiëren uit HDFS naar Azure Blob Storage met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. Een gekoppelde service van het type [OnPremisesHdfs](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [bestandsshare](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een on-premises HDFS naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van de data management gateway. De instructies in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**HDFS gekoppelde service:** in dit voorbeeld wordt de Windows-verificatie. Zie [HDFS gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Gekoppelde Azure Storage-service:**

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

**HDFS-invoergegevensset:** deze gegevensset verwijst naar de map HDFS DataTransfer/UnitTest /. De pijplijn worden alle bestanden in deze map gekopieerd naar de bestemming.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Een kopieeractiviteit in een pijplijn met File System-bron- en Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van deze gegevenssets voor invoer en uitvoer en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Kerberos-verificatie voor HDFS-connector gebruiken
Er zijn twee opties voor het instellen van de on-premises-omgeving om Kerberos-verificatie gebruiken in HDFS-connector. U kunt dat de beter aansluit op uw situatie.
* Optie 1: [Join gatewaycomputer in Kerberos-realm](#kerberos-join-realm)
* Optie 2: [wederzijdse vertrouwensrelatie tussen Windows-domein- en Kerberos-realm inschakelen](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Optie 1: Join gatewaycomputer in Kerberos-realm

#### <a name="requirement"></a>Vereiste:

* Het gateway-apparaat moet deelnemen aan het Kerberos-realm en kan niet deelnemen aan een Windows-domein.

#### <a name="how-to-configure"></a>Het configureren van:

**Op de gatewaycomputer:**

1.  Voer de **Ksetup** hulpprogramma om de Kerberos KDC-server en de realm te configureren.

    De machine moet worden geconfigureerd als een lid van een werkgroep, omdat een Kerberos-realm af van een Windows-domein wijkt. Dit kan worden bereikt door het Kerberos-realm en als volgt een KDC-server toe te voegen. Vervang *REALM.COM* met uw eigen respectieve realm indien nodig.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Opnieuw opstarten** de machine na het uitvoeren van deze opdrachten 2.

2.  Controleer de configuratie met **Ksetup** opdracht. De uitvoer moet, zoals:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Configureer de HDFS-connector op basis **Windows-verificatie** samen met uw Kerberos principal-naam en het wachtwoord voor het verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie voor informatie over de configuratie.

### <a name="kerberos-mutual-trust"></a>Optie 2: Wederzijdse vertrouwensrelatie tussen Windows-domein- en Kerberos-realm inschakelen

#### <a name="requirement"></a>Vereiste:
*   Het gateway-apparaat moet een Windows-domein toevoegen.
*   U moet zijn gemachtigd om de instellingen van de domeincontroller te werken.

#### <a name="how-to-configure"></a>Het configureren van:

> [!NOTE]
> Vervangen door REALM.COM en AD.COM in de volgende zelfstudie als u uw eigen respectieve realm en de domeincontroller naar behoefte.

**Op KDC-server:**

1.  Bewerken in de configuratie van de KDC **krb5.conf** bestand om te laten KDC vertrouwt Windows-domein verwijst naar de volgende configuratiesjabloon. Standaard de configuratie bevindt zich in **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Opnieuw opstarten** de KDC-service na de configuratie.

2.  Voorbereiden van een principal met de naam ** krbtgt/REALM.COM@AD.COM ** in de KDC-server met de volgende opdracht uit:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  In **hadoop.security.auth_to_local** HDFS-serviceconfiguratie-bestand, toe te voegen `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Op de domeincontroller:**

1.  Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Vertrouwensrelatie van Windows-domein voor Kerberos-Realm. [wachtwoord] is het wachtwoord voor de principal ** krbtgt/REALM.COM@AD.COM **.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Schakel versleuteling algoritme dat wordt gebruikt in Kerberos.

    1. Ga aan de Server Manager > Groepsbeleidsbeheer > domein > groepsbeleidsobjecten > standaard of beleid van het actieve domein en bewerken.

    2. In de **Editor voor Groepsbeleidsbeheer** pop-upvenster, Ga naar Computerconfiguratie > beleidsregels > Windows-instellingen > Beveiligingsinstellingen > lokaal beleid > beveiligingsopties, en configureer **netwerk beveiliging: voor Kerberos toegestane versleutelingstypen configureren**.

    3. Selecteer het versleutelingsalgoritme die u wilt gebruiken wanneer verbinding met KDC. Over het algemeen kunt u gewoon alle opties selecteren.

        ![Configuratie-versleutelingstypen voor Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Gebruik **Ksetup** opdracht voor het opgeven van het versleutelingsalgoritme op de specifieke REALM moet worden gebruikt.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Maak de toewijzing tussen het domeinaccount en Kerberos-principal om Kerberos-principal gebruiken in Windows-domein.

    1. Start de beheerprogramma's > **Active Directory: gebruikers en Computers**.

    2. Geavanceerde functies configureren door te klikken op **weergave** > **geavanceerde functies**.

    3. Zoek de account die u wilt maken, toewijzingen en om weer te geven met de rechtermuisknop op **naamstoewijzingen** > klikt u op **Kerberos-namen** tabblad.

    4. Voeg een principal van de realm.

        ![Toewijzen van beveiligings-id](media/data-factory-hdfs-connector/map-security-identity.png)

**Op de gatewaycomputer:**

* Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Configureer de HDFS-connector op basis **Windows-verificatie** samen met uw domeinaccount of Kerberos-Principal verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie voor informatie over de configuratie.

> [!NOTE]
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
