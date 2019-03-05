---
title: Gegevens kopiëren van HDFS met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een cloud of on-premises HDFS-bron naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: jingwang
ms.openlocfilehash: d0f22360a36105e5bfb877d7478b58c37fcc4353
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57213769"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Gegevens uit HDFS met Azure Data Factory kopiëren
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-hdfs-connector.md)
> * [Huidige versie](connector-hdfs.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van HDFS. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit HDFS kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit ondersteund als bronnen/sinks, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze HDFS-connector:

- Het kopiëren van bestanden met behulp van **Windows** (Kerberos) of **anoniem** verificatie.
- Het kopiëren van bestanden met behulp van **webhdfs** protocol of **ingebouwde DistCp** ondersteunen.
- Kopiëren van bestanden als-is of het parseren van/genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van een HDFS die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](concepts-integration-runtime.md) artikel voor meer informatie.

> [!NOTE]
> Zorg ervoor dat de Integration Runtime toegang tot **alle** de [naam van knooppunt server]: [knooppunt poort name] en [servers voor het knooppunt van gegevens]: [knooppunt gegevenspoort] van het Hadoop-cluster. Standaard [naam van knooppunt poort] is 50070 en standaard [knooppunt gegevenspoort] 50075 is.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op HDFS.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HDFS gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Hdfs**. | Ja |
| url |URL naar het HDFS |Ja |
| authenticationType | Toegestane waarden zijn: **Anonieme**, of **Windows**. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector, raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| Gebruikersnaam |Gebruikersnaam voor Windows-verificatie. Geef voor Kerberos-verificatie, `<username>@<domain>.com`. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja (voor Windows-verificatie) |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld: met behulp van anonieme verificatie**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: met behulp van Windows-verificatie**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HDFS-gegevensset.

Als u wilt kopiëren van gegevens uit HDFS, stel de eigenschap type van de gegevensset in **bestandsshare**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **FileShare** |Ja |
| folderPath | Pad naar de map. Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: rootfolder/submap/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Ja |
| fileName |  **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| Indeling | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u bestanden met een specifieke indeling parseren wilt, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken.

**Voorbeeld:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName | recursieve | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HDFS-bron.

### <a name="hdfs-as-source"></a>HDFS als bron

Als u wilt kopiëren van gegevens uit HDFS, stelt u het brontype in de kopieeractiviteit naar **HdfsSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HdfsSource** |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |
| distcpSettings | Eigenschappengroep wanneer HDFS DistCp gebruiken. | Nee |
| resourceManagerEndpoint | Het eindpunt van de Yarn ResourceManager | Ja als DistCp gebruiken |
| tempScriptPath | Pad naar een map gebruikt voor het opslaan van tijdelijke DistCp opdrachtscript. Het scriptbestand wordt gegenereerd door Data Factory en wordt verwijderd nadat de taak voor het kopiëren is voltooid. | Ja als DistCp gebruiken |
| distcpOptions | Extra opties opgegeven voor de opdracht DistCp. | Nee |

**Voorbeeld: HDFS-bron in de kopieeractiviteit middels de laden ongedaan maken**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

Meer informatie over het gebruik van DistCp gegevens uit HDFS efficiënt in de volgende sectie te kopiëren.

## <a name="use-distcp-to-copy-data-from-hdfs"></a>DistCp gebruiken om te kopiëren van gegevens uit HDFS

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) is een systeemeigen Hadoop-opdrachtregelprogramma voor het gedistribueerde kopiëren in een Hadoop-cluster. Wanneer een opdracht Distcp uitvoert, wordt eerst alle bestanden worden gekopieerd, maakt u meerdere taken van de kaart in het Hadoop-cluster wordt een lijst, en elke kaart taak binaire kopie van bron naar het sink-doet.

DistCp gebruiken om te kopiëren van bestanden als ondersteuning voor activiteit kopiëren-is in Azure Blob (met inbegrip van [gefaseerd kopiëren](copy-activity-performance.md) of Azure Data Lake Store, in welk geval het kan volledig benut de kracht van uw cluster in plaats van die worden uitgevoerd op de zelfgehoste Cloudintegratieruntime . Met name als uw cluster is een zeer krachtig en biedt betere doorvoer van de kopie. Op basis van uw configuratie in Azure Data Factory, kopieeractiviteit automatisch maken van een opdracht distcp indienen bij uw Hadoop-cluster en de kopiestatus controleren.

### <a name="prerequsites"></a>Prerequsites

DistCp gebruiken om te kopiëren bestanden als-is van HDFS tot Azure-Blob (met inbegrip van gefaseerd kopiëren) of Azure Data Lake Store, zorg ervoor dat uw Hadoop-cluster voldoet aan de volgende vereisten:

1. MapReduce en Yarn-services zijn ingeschakeld.
2. Yarn-versie is 2.5 of hoger.
3. HDFS-server is geïntegreerd met uw gegevensopslag doel - Azure Blob of Azure Data Lake Store:

    - Azure Blob-bestandssysteem systeem zelf wordt ondersteund sinds Hadoop 2.7. U hoeft alleen jar pad opgeven in Hadoop env-configuratie.
    - Azure Data Lake Store-bestandssysteem is verpakt vanaf Hadoop 3.0.0-alpha1. Als uw Hadoop-cluster lager dan deze versie is, moet u handmatig importeren ADLS gerelateerde jar-pakketten (azure-datalake-store.jar) in het cluster op basis van [hier](https://hadoop.apache.org/releases.html), en de jar-pad opgeven in Hadoop env config.

4. Bereid een tijdelijke map in HDFS. Deze tijdelijke map wordt gebruikt voor het opslaan van DistCp shell-script, zodat het KB-niveau ruimte wordt innemen.
5. Zorg ervoor dat het gebruikersaccount dat is opgegeven in de gekoppelde Service HDFS gemachtigd om te een)-toepassing in Yarn; verzenden (b) de machtiging submap maken, lezen/schrijven bestanden onder de tijdelijke map hebben.

### <a name="configurations"></a>Configuraties

Hieronder volgt een voorbeeld van de configuratie van activiteit kopiëren om gegevens te kopiëren uit HDFS naar Azure Blob DistCp gebruiken:

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromHDFSToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "HDFSDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HdfsSource",
                "distcpSettings": {
                    "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
                    "tempScriptPath": "/usr/hadoop/tempscript",
                    "distcpOptions": "-strategy dynamic -map 100"
                }
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Kerberos-verificatie voor HDFS-connector gebruiken

Er zijn twee opties voor het instellen van de on-premises-omgeving om Kerberos-verificatie gebruiken in HDFS-connector. U kunt dat de beter aansluit op uw situatie.
* Optie 1: [Zelfgehoste Cloudintegratieruntime machine toevoegen in Kerberos-realm](#kerberos-join-realm)
* Optie 2: [Wederzijdse vertrouwensrelatie tussen Windows-domein- en Kerberos-realm inschakelen](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Optie 1: Zelfgehoste Cloudintegratieruntime machine toevoegen in Kerberos-realm

#### <a name="requirements"></a>Vereisten

* De machine zelfgehoste Cloudintegratieruntime moet deelnemen aan het Kerberos-realm en kan niet deelnemen aan een Windows-domein.

#### <a name="how-to-configure"></a>Configureren

**Op de machine zelfgehoste Cloudintegratieruntime:**

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

#### <a name="requirements"></a>Vereisten

*   De machine zelfgehoste Cloudintegratieruntime moet een Windows-domein toevoegen.
*   U moet zijn gemachtigd om de instellingen van de domeincontroller te werken.

#### <a name="how-to-configure"></a>Configureren

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

2.  Voorbereiden van een principal met de naam **krbtgt/REALM.COM@AD.COM** in de KDC-server met de volgende opdracht uit:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  In **hadoop.security.auth_to_local** HDFS-serviceconfiguratie-bestand, toe te voegen `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Op de domeincontroller:**

1.  Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Vertrouwensrelatie van Windows-domein voor Kerberos-Realm. [wachtwoord] is het wachtwoord voor de principal **krbtgt/REALM.COM@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Schakel versleuteling algoritme dat wordt gebruikt in Kerberos.

    1. Ga aan de Server Manager > Groepsbeleidsbeheer > domein > groepsbeleidsobjecten > standaard of beleid van het actieve domein en bewerken.

    2. In de **Editor voor Groepsbeleidsbeheer** pop-upvenster, Ga naar Computerconfiguratie > beleidsregels > Windows-instellingen > Beveiligingsinstellingen > lokaal beleid > beveiligingsopties, en configureer **netwerk beveiliging: Voor Kerberos toegestane versleutelingstypen configureren**.

    3. Selecteer het versleutelingsalgoritme die u wilt gebruiken wanneer verbinding met KDC. Over het algemeen kunt u gewoon alle opties selecteren.

        ![Configuratie-versleutelingstypen voor Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Gebruik **Ksetup** opdracht voor het opgeven van het versleutelingsalgoritme op de specifieke REALM moet worden gebruikt.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Maak de toewijzing tussen het domeinaccount en Kerberos-principal om Kerberos-principal gebruiken in Windows-domein.

    1. Start de beheerprogramma's > **Active Directory: gebruikers en Computers**.

    2. Geavanceerde functies configureren door te klikken op **weergave** > **geavanceerde functies**.

    3. Zoek de account die u wilt maken, toewijzingen en om weer te geven met de rechtermuisknop op **naamstoewijzingen** > klikt u op **Kerberos-namen** tabblad.

    4. Voeg een principal van de realm.

        ![Toewijzen van beveiligings-id](media/connector-hdfs/map-security-identity.png)

**Op de machine zelfgehoste Cloudintegratieruntime:**

* Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Configureer de HDFS-connector op basis **Windows-verificatie** samen met uw domeinaccount of Kerberos-Principal verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie voor informatie over de configuratie.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
