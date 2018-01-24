---
title: "Gegevens kopiëren van Azure Data Factory met HDFS | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van een cloud of on-premises HDFS-bron naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 
ms.author: jingwang
ms.openlocfilehash: 80892d5f87c1f7a588023e4baa5de79bd73329a5
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-and-to-hdfs-using-azure-data-factory"></a>Gegevens kopiëren van en naar Azure Data Factory met HDFS
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-hdfs-connector.md)
> * [Versie 2 - Preview](connector-hdfs.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar HDFS. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [HDFS-connector in V1](v1/data-factory-hdfs-connector.md).


## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit HDFS kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als bronnen/put wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze HDFS-connector:

- Het kopiëren van bestanden met behulp van **Windows** (Kerberos) of **anoniem** verificatie.
- Het kopiëren van bestanden met behulp van **webhdfs** protocol of **ingebouwde DistCp** ondersteunen.
- Kopiëren van bestanden als-is of bij het parseren van/genereren van bestanden met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van/naar een HDFS die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](concepts-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor HDFS.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HDFS gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Hdfs**. | Ja |
| url |URL naar de HDFS |Ja |
| authenticationType | Toegestane waarden zijn: **anoniem**, of **Windows**. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| userName |Gebruikersnaam voor Windows-verificatie. Geef voor Kerberos-verificatie, `<username>@<domain>.com`. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. Dit veld markeren als SecureString. |Ja (voor Windows-verificatie) |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld: anonieme verificatie gebruikt**

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de HDFS-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren uit HDFS, **FileShare**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **bestandsshare** |Ja |
| folderPath | Pad naar de map. Bijvoorbeeld: map/submap / |Ja |
| fileName | Geef de naam van het bestand in de **folderPath** als u wilt kopiëren uit een specifiek bestand. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de status van de gegevensset verwijst naar alle bestanden in de map als bron. |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren van bestanden met een specifieke indeling, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

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
            "fileName": "myfile.csv.gz",
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

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door de HDFS-bron- en sink ondersteund.

### <a name="hdfs-as-source"></a>HDFS als bron

Om gegevens te kopiëren uit HDFS, stelt u het brontype in de kopieerbewerking naar **HdfsSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HdfsSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. Opmerking Wanneer recursieve is ingesteld op true en sink is bestandsgebaseerde opslag, lege map/subbewerkingen-folder niet worden gekopieerd/gemaakt op de sink.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |
| distcpSettings | Eigenschappengroep bij gebruik van HDFS DistCp. | Nee |
| resourceManagerEndpoint | Het eindpunt Yarn ResourceManager | Ja, als DistCp |
| tempScriptPath | Pad naar een map gebruikt voor het opslaan van tijdelijke DistCp opdrachtscript. Het scriptbestand wordt gegenereerd door de Data Factory en wordt verwijderd nadat de taak kopiëren voltooid. | Ja, als DistCp |
| distcpOptions | Aanvullende opties voor het opgegeven DistCp-opdracht. | Nee |

**Voorbeeld: HDFS-bron in de kopieeractiviteit middels de verwijderen**

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

Meer informatie over het gebruik van DistCp om gegevens te kopiëren uit HDFS efficiënt uit de volgende sectie.

## <a name="use-distcp-to-copy-data-from-hdfs"></a>DistCp gebruiken om gegevens te kopiëren uit HDFS

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) is een systeemeigen Hadoop-opdrachtregelprogramma voor gedistribueerde kopiëren in een Hadoop-cluster. Wanneer een opdracht Distcp uitvoert, het eerst alle bestanden worden gekopieerd, maakt u meerdere taken van de kaart in de Hadoop-cluster wordt een lijst en elke taak kaart doet binaire kopiëren van bron opvangen.

Activiteit ondersteuning DistCp met kopiëren van bestanden als kopiëren-is in de Azure-Blob (inclusief [tijdelijke kopie](copy-activity-performance.md) of Azure Data Lake Store in dat geval uw cluster power in plaats van op de Runtime Self-hosted-integratie kunnen volledig gebruikmaken . Dit biedt betere doorvoer van kopiëren met name als uw cluster zeer belangrijk is. Op basis van uw configuratie in Azure Data Factory, kopieeractiviteit automatisch maken van een opdracht distcp, dienen bij de Hadoop-cluster en controleren van de status van het exemplaar.

### <a name="prerequsites"></a>Prerequsites

DistCp gebruiken om te kopiëren bestanden als-is uit HDFS Azure-Blob (inclusief gefaseerde kopiëren) of Azure Data Lake Store Zorg ervoor dat uw Hadoop-cluster voldoet aan de onderstaande vereisten:

1. MapReduce en Yarn-services zijn ingeschakeld.
2. Yarn-versie is 2.5 of hoger.
3. HDFS-server is geïntegreerd met uw gegevensopslag doel - Azure Blob- of Azure Data Lake Store:

    - Azure Blob-bestandssysteem is standaard ondersteund sinds Hadoop 2.7. U hoeft alleen te jar pad opgeeft in Hadoop env config.
    - Azure Data Lake Store bestandssysteem wordt verpakt vanaf Hadoop 3.0.0-alpha1. Als uw Hadoop-cluster lager dan deze versie is, moet u handmatig importeren ADLS gerelateerde jar-pakketten (azure datalake store.jar) in een cluster met [hier](https://hadoop.apache.org/releases.html), en pad van de jar in Hadoop env configuratie opgeven.

4. Bereid een tijdelijke map in HDFS. Deze tijdelijke map wordt gebruikt voor het opslaan van DistCp shell-script, zodat deze KB-niveau inneemt.
5. Zorg ervoor dat het gebruikersaccount dat is opgegeven in de gekoppelde Service HDFS gemachtigd om te een)-toepassing in Yarn; verzenden b) de machtiging submap maken, lezen/schrijven bestanden onder boven de tijdelijke map hebben.

### <a name="configurations"></a>Configuraties

Hieronder volgt een voorbeeld van de activiteit configuratie gegevens uit HDFS kopiëren naar Azure Blob met DistCp:

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

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Kerberos-verificatie gebruiken voor HDFS-connector

Er zijn twee opties voor het instellen van de on-premises omgeving om het gebruik van Kerberos-verificatie in HDFS-connector. U kunt de beter past bij uw aanvraag.
* Optie 1: [Join Self-hosted integratie Runtime-machine in Kerberos-realm](#kerberos-join-realm)
* Optie 2: [wederzijdse vertrouwensrelatie tussen Windows-domein en Kerberos-realm inschakelen](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Optie 1: Join Self-hosted integratie Runtime-machine in Kerberos-realm

#### <a name="requirements"></a>Vereisten

* De machine Self-hosted integratie Runtime moet deelnemen aan het Kerberos-realm en kan niet deelnemen aan een Windows-domein.

#### <a name="how-to-configure"></a>Het configureren van

**Op de machine Self-hosted integratie Runtime:**

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

#### <a name="requirements"></a>Vereisten

*   De machine Self-hosted integratie Runtime moet lid worden van een Windows-domein.
*   U moet de machtiging voor het bijwerken van de domeincontroller-instellingen.

#### <a name="how-to-configure"></a>Het configureren van

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

        ![Configuratie-versleutelingstypen voor Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Gebruik **Ksetup** opdracht voor het opgeven van het versleutelingsalgoritme in de specifieke REALM moet worden gebruikt.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  De toewijzing tussen de domeinaccount en Kerberos-principal maken om te gebruiken van Kerberos-principal in de Windows-domein.

    1. Start de beheerprogramma's > **Active Directory: gebruikers en Computers**.

    2. Geavanceerde functies configureren door te klikken op **weergave** > **geavanceerde functies**.

    3. Zoek de account waarmee u wilt toewijzingen maken en klik met de rechtermuisknop om weer te geven **naamstoewijzingen** > klikt u op **Kerberos-namen** tabblad.

    4. Een principal van de realm toevoegen.

        ![Beveiligings-id toewijzen](media/connector-hdfs/map-security-identity.png)

**Op de machine Self-hosted integratie Runtime:**

* Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Configureert de HDFS-connector met **Windows-verificatie** samen met uw domeinaccount of Kerberos-Principal verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie over configuratie-informatie.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).