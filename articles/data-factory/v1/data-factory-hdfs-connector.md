---
title: Gegevens verplaatsen van de lokale HDFS | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit de lokale HDFS met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 87acbe81d20e0f2b209565eace16de1b979b1d96
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Gegevens verplaatsen van de lokale HDFS met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-hdfs-connector.md)
> * [Versie 2 - Preview](../connector-hdfs.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [HDFS-connector in V2](../connector-hdfs.md).

Dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een lokale HDFS. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit HDFS kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen zwevend gegevens uit een lokale HDFS tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een lokale HDFS.

> [!NOTE]
> Kopieeractiviteit verwijdert het bronbestand niet nadat deze is gekopieerd naar de bestemming. Als u verwijderen van het bronbestand na een geslaagde kopie wilt, maakt u een aangepaste activiteit voor het bestand verwijderen en het gebruik van de activiteit in de pijplijn. 

## <a name="enabling-connectivity"></a>Connectiviteit inschakelen
Verbinding maken met lokale HDFS met behulp van Data Management Gateway biedt ondersteuning voor Data Factory-service. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway. Gebruik van de gateway verbinding maken met HDFS, zelfs als deze is opgenomen in een Azure IaaS VM.

> [!NOTE]
> Zorg ervoor dat de Data Management Gateway kunnen toegang tot **alle** [knooppunt naamserver]: [name knooppunt poort] en [knooppunt gegevensservers]: [knooppunt gegevenspoort] van het Hadoop-cluster. Standaard [naam van knooppunt poort] is 50070 en standaard [knooppunt gegevenspoort] 50075 is.

Terwijl u de gateway op de lokale machine of de Azure VM als de HDFS installeren kunt, wordt u aangeraden dat u de gateway op een afzonderlijke computer/Azure IaaS VM installeren. Met gateway op een afzonderlijke computer bronconflicten vermindert en verbetert de prestaties. Wanneer u de gateway op een afzonderlijke computer installeert, is de machine moet toegang kunnen krijgen tot de machine met de HDFS.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens van een HDFS-bron verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een gegevensopslag HDFS [JSON-voorbeeld: gegevens kopiëren van lokale HDFS naar Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar HDFS:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een gekoppelde service gegevensopslag is gekoppeld aan een gegevensfactory. Maken van een gekoppelde service van het type **Hdfs** een lokale HDFS koppelen aan uw gegevensfactory. De volgende tabel bevat de beschrijving voor JSON-elementen die specifiek zijn voor HDFS gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Hdfs** |Ja |
| URL |URL naar de HDFS |Ja |
| authenticationType |Anoniem, of Windows. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| userName |Gebruikersnaam voor Windows-verificatie. Geef voor Kerberos-verificatie, `<username>@<domain>.com`. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het HDFS. |Ja |
| encryptedCredential |[Nieuwe AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) uitvoer van de referentie toegang. |Nee |

### <a name="using-anonymous-authentication"></a>Anonieme verificatie

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
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **FileShare** (inclusief HDFS gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de map. Voorbeeld:`myfolder`<br/><br/>Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: map opgeven voor folder\subfolder,\\\\submap en geef voor d:\samplefolder, d:\\\\Voorbeeldmap.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Voorbeeld: folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.

### <a name="using-partionedby-property"></a>Gebruik de eigenschap partionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath en de bestandsnaam voor de time series-gegevens met de **partitionedBy** -eigenschap [Data Factory-functies en de systeemvariabelen](data-factory-functions-variables.md).

Zie voor meer informatie over het time series gegevenssets, planning en segmenten, [gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md), en [pijplijnen maken](data-factory-create-pipelines.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
In dit voorbeeld {segment} is vervangen door de opgegeven waarde van de Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst voor het starten van de tijd van het segment. FolderPath verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

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
In dit voorbeeld worden jaar, maand, dag en tijd van de SliceStart uitgepakt in verschillende variabelen die worden gebruikt door de eigenschappen voor folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Voor de Kopieeractiviteit, wanneer de bron van het type **FileSystemSource** de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |

## <a name="supported-file-and-compression-formats"></a>Ondersteunde indelingen voor bestands- en compressie
Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel voor meer informatie.

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van lokale HDFS naar Azure Blob
Dit voorbeeld laat zien hoe gegevens kopiëren van een lokale HDFS naar Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

Het voorbeeld bevat JSON-definities voor de volgende Data Factory-entiteiten. U kunt deze definities gebruiken voor het maken van een pijplijn om gegevens uit HDFS kopiëren naar Azure Blob Storage met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. Een gekoppelde service van het type [OnPremisesHdfs](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [FileShare](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert gegevens van een lokale HDFS naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van de data management gateway. De instructies in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

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

**HDFS invoergegevensset:** deze gegevensset verwijst naar de map HDFS DataTransfer/UnitTest /. De pijplijn kopieert alle bestanden in deze map naar de bestemming.

Instelling 'extern': 'true' informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

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

**Een kopieeractiviteit in een pijplijn met File System-bron en sink van Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van deze invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Kerberos-verificatie gebruiken voor HDFS-connector
Er zijn twee opties voor het instellen van de on-premises omgeving om het gebruik van Kerberos-verificatie in HDFS-connector. U kunt de beter past bij uw aanvraag.
* Optie 1: [Join gatewaycomputer in Kerberos-realm](#kerberos-join-realm)
* Optie 2: [wederzijdse vertrouwensrelatie tussen Windows-domein en Kerberos-realm inschakelen](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Optie 1: Join gatewaycomputer in Kerberos-realm

#### <a name="requirement"></a>Vereiste:

* Het gateway-apparaat moet deelnemen aan het Kerberos-realm en kan niet deelnemen aan een Windows-domein.

#### <a name="how-to-configure"></a>Het configureren van:

**Op de gatewaycomputer:**

1.  Voer de **Ksetup** hulpprogramma voor configuratie van het Kerberos KDC-server en de standaardrealm.

    De machine moet worden geconfigureerd als lid van een werkgroep, omdat een Kerberos-realm af van een Windows-domein wijkt. Dit kan worden bereikt door het Kerberos-realm instellen en als volgt een KDC-server toe te voegen. Vervang *REALM.COM* met uw eigen respectieve realm indien nodig.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Opnieuw opstarten** de machine na het uitvoeren van deze opdrachten 2.

2.  Controleer of de configuratie met **Ksetup** opdracht. De uitvoer moet als volgt:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Configureert de HDFS-connector met **Windows-verificatie** samen met uw Kerberos principal-naam en wachtwoord verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie over configuratie-informatie.

### <a name="kerberos-mutual-trust"></a>Optie 2: Wederzijdse vertrouwensrelatie tussen Windows-domein en Kerberos-realm inschakelen

#### <a name="requirement"></a>Vereiste:
*   Het gateway-apparaat moet lid worden van een Windows-domein.
*   U moet de machtiging voor het bijwerken van de domeincontroller-instellingen.

#### <a name="how-to-configure"></a>Het configureren van:

> [!NOTE]
> REALM.COM en AD.COM in de volgende zelfstudie vervangen door uw eigen respectieve realm en de domeincontroller naar behoefte.

**Op de KDC-server:**

1.  De configuratie van de KDC in bewerken **krb5.conf** bestand om te laten KDC vertrouwt Windows-domein die verwijst naar de volgende configuratiesjabloon. Standaard de configuratie bevindt zich op **/etc/krb5.conf**.

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

  **Opnieuw opstarten** na de configuratie van de KDC-service.

2.  Voorbereiden van een principal met de naam  **krbtgt/REALM.COM@AD.COM**  in de KDC-server met de volgende opdracht:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  In **hadoop.security.auth_to_local** HDFS-serviceconfiguratie bestand, het toevoegen van `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Op de domeincontroller:**

1.  Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Een vertrouwensrelatie van Windows-domein voor Kerberos-Realm. [wachtwoord] is het wachtwoord voor de principal  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Selecteer de versleutelingsalgoritme die worden gebruikt in Kerberos.

    1. Ga aan serverbeheer > Groepsbeleidsbeheer > Domain > groepsbeleidsobjecten > standaard of actieve domeinbeleid en bewerken.

    2. In de **Editor voor Groepsbeleidsbeheer** pop-upvenster, Ga naar Computerconfiguratie > beleidsregels > Windows-instellingen > Beveiligingsinstellingen > lokaal beleid > beveiligingsopties, en configureer **netwerkbeveiliging: Configureer versleutelingstypen voor Kerberos toegestaan**.

    3. Selecteer het versleutelingsalgoritme die u wilt gebruiken wanneer verbinding met de KDC. Doorgaans kunt u gewoon alle opties selecteren.

        ![Configuratie-versleutelingstypen voor Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Gebruik **Ksetup** opdracht voor het opgeven van het versleutelingsalgoritme in de specifieke REALM moet worden gebruikt.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  De toewijzing tussen de domeinaccount en Kerberos-principal maken om te gebruiken van Kerberos-principal in de Windows-domein.

    1. Start de beheerprogramma's > **Active Directory: gebruikers en Computers**.

    2. Geavanceerde functies configureren door te klikken op **weergave** > **geavanceerde functies**.

    3. Zoek de account waarmee u wilt toewijzingen maken en klik met de rechtermuisknop om weer te geven **naamstoewijzingen** > klikt u op **Kerberos-namen** tabblad.

    4. Een principal van de realm toevoegen.

        ![Beveiligings-id toewijzen](media/data-factory-hdfs-connector/map-security-identity.png)

**Op de gatewaycomputer:**

* Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Configureert de HDFS-connector met **Windows-verificatie** samen met uw domeinaccount of Kerberos-Principal verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie over configuratie-informatie.

> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
