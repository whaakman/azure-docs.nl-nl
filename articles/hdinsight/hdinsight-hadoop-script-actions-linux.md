---
title: Scriptactieontwikkeling met HDInsight op basis van Linux - Azure | Microsoft Docs
description: Informatie over het Bash-scripts gebruiken voor het aanpassen van Linux gebaseerde HDInsight-clusters. Het script actie-onderdeel van HDInsight kunt u scripts uitvoeren tijdens of na het maken van het cluster. Scripts kunnen worden gebruikt voor het cluster configuratie-instellingen wijzigen of aanvullende software installeren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: ddf5db3e61633c45e388e161e165637521803094
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="script-action-development-with-hdinsight"></a>Scripts actie te ontwikkelen met HDInsight

Informatie over het aanpassen van uw HDInsight-cluster met behulp van Bash-scripts. Scriptacties zijn een manier om aan te passen HDInsight tijdens of na het maken van het cluster.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="what-are-script-actions"></a>Wat zijn scriptacties

Scriptacties zijn Bash-scripts die Azure wordt uitgevoerd op de clusterknooppunten configuratiewijzigingen aanbrengen of software installeren. Een scriptactie wordt uitgevoerd als hoofdmap en biedt volledige toegangsrechten voor de clusterknooppunten.

Scriptacties kunnen worden toegepast met de volgende methoden:

| Gebruik deze methode om een script te passen... | Tijdens het cluster maken... | Op een cluster uitgevoerd... |
| --- |:---:|:---:|
| Azure Portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure-CLI |&nbsp; |✓ |
| HDInsight .NET-SDK |✓ |✓ |
| Azure Resource Manager-sjabloon |✓ |&nbsp; |

Zie voor meer informatie over het gebruik van deze methoden om toe te passen scriptacties [aanpassen HDInsight-clusters met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Aanbevolen procedures voor het ontwikkelen van scripts

Wanneer u een aangepast script voor een HDInsight-cluster ontwikkelt, zijn er enkele aanbevolen procedures rekening moet houden:

* [Doel de Hadoop-versie](#bPS1)
* [Doel-versie van het besturingssysteem](#bps10)
* [Stabiele koppelingen naar scriptbronnen die bevatten](#bPS2)
* [Vooraf gecompileerde resources gebruiken](#bPS4)
* [Zorg ervoor dat het cluster aanpassing script idempotent](#bPS3)
* [Zorg ervoor dat hoge beschikbaarheid van de cluster-architectuur](#bPS5)
* [De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren](#bPS6)
* [Schrijven van gegevens naar STDOUT en STDERR](#bPS7)
* [ASCII-bestanden opslaan met regeleinden LF](#bps8)
* [Pogingslogica voor gebruik van tijdelijke fouten wilt herstellen](#bps9)

> [!IMPORTANT]
> Scriptacties moeten worden voltooid binnen 60 minuten of het proces is mislukt. Het script wordt uitgevoerd tijdens het inrichten van knooppunt tegelijk met andere processen installatie en configuratie. Concurrentie voor resources, zoals CPU-tijd of netwerk bandbreedte kan ertoe leiden dat het script duurt langer dan in uw ontwikkelomgeving te voltooien.

### <a name="bPS1"></a>Doel de Hadoop-versie

Verschillende versies van HDInsight hebben verschillende versies van Hadoop-services en -onderdelen geïnstalleerd. Als uw script een specifieke versie van een service of het onderdeel verwacht, moet u het script alleen gebruiken met de versie van HDInsight met de vereiste onderdelen. U vindt informatie over het onderdeel-versies die zijn opgenomen in HDInsight met behulp van de [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md) document.

### <a name="bps10"></a>Versie van het besturingssysteem zijn gericht

Linux gebaseerde HDInsight is gebaseerd op het distributiepunt Ubuntu Linux. Verschillende versies van HDInsight, is afhankelijk van verschillende versies van Ubuntu, die de werking van uw script kunnen wijzigen. Bijvoorbeeld, HDInsight 3,4 en eerder zijn gebaseerd op Ubuntu-versies die Upstart gebruiken. Versie 3.5 en hoger zijn gebaseerd op Ubuntu 16.04 dat gebruikmaakt van Systemd. Systemd en Upstart zijn afhankelijk van andere opdrachten, zodat uw script werkt met beide worden geschreven.

HDInsight 3.4 en 3.5 nog een belangrijk verschil is dat `JAVA_HOME` wijst nu Java 8.

U kunt de versie van het besturingssysteem controleren met behulp van `lsb_release`. De volgende code toont hoe u bepaalt of het script wordt uitgevoerd op Ubuntu 14 of 16:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

U vindt de volledige script dat deze fragmenten op https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh bevat.

Zie voor de versie van Ubuntu die wordt gebruikt door HDInsight, het [HDInsight componentversie](hdinsight-component-versioning.md) document.

Zie voor informatie over de verschillen tussen Systemd en Upstart, [Systemd voor Upstart gebruikers](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Stabiele koppelingen naar scriptbronnen die bevatten

Het script en de bijbehorende bronnen moeten beschikbaar zijn tijdens de levensduur van het cluster blijven. Deze resources zijn vereist als nieuwe knooppunten worden toegevoegd aan het cluster tijdens het schalen van bewerkingen.

De aanbevolen procedure is om te downloaden en te archiveren alles in een Azure Storage-account voor uw abonnement.

> [!IMPORTANT]
> Het opslagaccount gebruikt, moet het standaardopslagaccount voor het cluster of een openbare, alleen-lezen container op andere storage-account.

Bijvoorbeeld, de voorbeelden die is geleverd door Microsoft worden opgeslagen in de [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage-account. Deze locatie is een openbaar, alleen-lezen container onderhouden door het HDInsight-team.

### <a name="bPS4"></a>Vooraf gecompileerde resources gebruiken

Als u de tijd die nodig is voor het script uitvoert, te voorkomen dat bewerkingen die bronnen van broncode compileren. Bijvoorbeeld, vooraf gecompileerd bronnen en deze opslaan op een Azure Storage-account blob in hetzelfde Datacenter als HDInsight.

### <a name="bPS3"></a>Zorg ervoor dat het cluster aanpassing script idempotent

Scripts moet idempotent. Als het script wordt meerdere keren uitgevoerd, moet deze het cluster geretourneerd naar dezelfde toestand elke keer.

Bijvoorbeeld dat een script dat wordt gewijzigd van configuratiebestanden geen dubbele vermeldingen moet toevoegen als meerdere keren uitgevoerd.

### <a name="bPS5"></a>Zorg ervoor dat hoge beschikbaarheid van de cluster-architectuur

Linux gebaseerde HDInsight-clusters bieden twee hoofdknooppunten die actief zijn binnen het cluster, en scriptacties uitgevoerd op beide knooppunten. Als de onderdelen die u installeert slechts één hoofdknooppunt verwacht, installeer niet de onderdelen op beide hoofdknooppunten.

> [!IMPORTANT]
> Services die worden geleverd als onderdeel van HDInsight zijn ontworpen om failover tussen de twee hoofdknooppunten indien nodig. Deze functionaliteit is niet uitgebreid met aangepaste onderdelen zijn geïnstalleerd via scriptacties. Als u hoge beschikbaarheid voor aangepaste onderdelen nodig hebt, moet u uw eigen mechanisme failover implementeren.

### <a name="bPS6"></a>De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren

Onderdelen die u op het cluster installeert wellicht een standaardconfiguratie waarmee Hadoop Distributed File System (HDFS) opslag gebruikt. HDInsight gebruikt Azure Storage of Data Lake Store als de standaard-opslag. Beide biedt een compatibel bestandssysteem van HDFS dat gegevens behouden blijft zelfs als het cluster wordt verwijderd. U moet wellicht onderdelen die u installeert voor het gebruik van WASB of ADL in plaats van HDFS configureren.

Voor de meeste bewerkingen hoeft u niet om op te geven van het bestandssysteem. Bijvoorbeeld kopieert de volgende de giraph examples.jar bestand uit het lokale bestandssysteem aan het cluster:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

In dit voorbeeld wordt de `hdfs` transparant gebruikt de clusteropslag standaard opdracht. Voor bepaalde bewerkingen moet u mogelijk de URI opgeven. Bijvoorbeeld: `adl:///example/jars` voor Data Lake Store of `wasb:///example/jars` voor Azure Storage.

### <a name="bPS7"></a>Schrijven van gegevens naar STDOUT en STDERR

HDInsight logboeken scriptuitvoer die naar STDOUT en STDERR worden geschreven. U kunt deze gegevens met behulp van de Ambari-webgebruikersinterface weergeven.

> [!NOTE]
> Ambari is alleen beschikbaar als het cluster is gemaakt. Als u een scriptactie tijdens het maken van het cluster en mislukt het maken, Zie de sectie probleemoplossing [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) voor andere manieren van de toegang tot geregistreerde gegevens.

De meeste hulpprogramma's en installatiepakketten schrijven al gegevens naar STDOUT en STDERR, maar u kunt aanvullende logboekregistratie toevoegen. Tekst om naar te verzenden STDOUT, gebruik `echo`. Bijvoorbeeld:

```bash
echo "Getting ready to install Foo"
```

Standaard `echo` verzendt de tekenreeks naar STDOUT. Voeg deze rechtstreeks in STDERR, `>&2` voordat `echo`. Bijvoorbeeld:

```bash
>&2 echo "An error occurred installing Foo"
```

U wordt doorgestuurd naar STDOUT in STDERR (2) in plaats daarvan geschreven informatie. Zie voor meer informatie over i/o-omleiding [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Zie voor meer informatie over het weergeven van informatie die wordt geregistreerd door scriptacties [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a>ASCII-bestanden opslaan met regeleinden LF

Bash-scripts moeten worden opgeslagen als ASCII-indeling met regels die door LF is beëindigd. Bestanden die worden opgeslagen als UTF-8 of CRLF gebruiken als het beëindigen van de regel kunnen mislukken met de volgende fout:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Pogingslogica voor gebruik van tijdelijke fouten wilt herstellen

Bij het downloaden van bestanden, installatie van pakketten via apt get- of andere acties die gegevens via internet verzenden, wordt de actie mislukken vanwege tijdelijke netwerkfouten. De externe bron die u wilt communiceren kan bijvoorbeeld een herstelbewerking worden niet via een back-knooppunt.

Als u wilt uw script robuuste tijdelijke fouten, kunt u Pogingslogica implementeren. De volgende functie laat zien hoe Pogingslogica implementeren. Het opnieuw probeert de bewerking driemaal voordat deze is mislukt.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

De volgende voorbeelden laten zien hoe deze functie wilt gebruiken.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Help-methoden voor aangepaste scripts

Script actie Help-methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze methoden zijn opgenomen in de[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script. Gebruik de volgende te downloaden en gebruiken ze als onderdeel van uw script:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

De volgende hulpprogramma's beschikbaar voor gebruik in uw script:

| Gebruik helper | Beschrijving |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Een bestand downloadt van de bron-URI naar het opgegeven pad. Standaard wordt er een bestaand bestand niet overschreven. |
| `untar_file TARFILE DESTDIR` |Extraheert een tar-bestand (met behulp van `-xf`) naar de doelmap. |
| `test_is_headnode` |Indien uitgevoerd op een cluster hoofdknooppunt, return 1; anders wordt 0. |
| `test_is_datanode` |Als het huidige knooppunt gegevensknooppunt (worker) is, retourneert een 1; anders wordt 0. |
| `test_is_first_datanode` |Als het huidige knooppunt is de eerste gegevens (worker) knooppunt (benoemde workernode0) geretourneerd met een 1; anders wordt 0. |
| `get_headnodes` |Retourneert de volledig gekwalificeerde domeinnaam van de headnodes in het cluster. De namen zijn gescheiden door komma's. Een lege tekenreeks geretourneerd bij fout. |
| `get_primary_headnode` |Hiermee wordt de volledig gekwalificeerde domeinnaam van de primaire headnode opgehaald. Een lege tekenreeks geretourneerd bij fout. |
| `get_secondary_headnode` |Hiermee wordt de volledig gekwalificeerde domeinnaam van de secundaire headnode opgehaald. Een lege tekenreeks geretourneerd bij fout. |
| `get_primary_headnode_number` |Het numerieke achtervoegsel van de primaire headnode opgehaald. Een lege tekenreeks geretourneerd bij fout. |
| `get_secondary_headnode_number` |Het numerieke achtervoegsel van de secundaire headnode opgehaald. Een lege tekenreeks geretourneerd bij fout. |

## <a name="commonusage"></a>Algemene gebruikspatronen

Deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die u tegenkomen kunt tijdens het schrijven van uw eigen aangepaste scripts.

### <a name="passing-parameters-to-a-script"></a>Parameters doorgeven aan een script

In sommige gevallen kan het script parameters vereist. Bijvoorbeeld, moet u mogelijk het beheerderswachtwoord voor het cluster wanneer u de Ambari REST-API.

Parameters doorgegeven aan het script worden aangeduid als *positieparameters*, en zijn toegewezen aan `$1` voor de eerste parameter `$2` voor de tweede pagina, en dus eenmalige aanmelding. `$0`bevat de naam van het script zelf.

Waarden worden doorgegeven aan het script als parameters moeten tussen enkele aanhalingstekens ('). Hiermee zorgt u ervoor dat de doorgegeven waarde behandeld als een letterlijke waarde.

### <a name="setting-environment-variables"></a>Omgevingsvariabelen instellen

Instellen van een omgevingsvariabele wordt uitgevoerd door de volgende instructie:

    VARIABLENAME=value

VARIABELENAAM is waar de naam van de variabele. Gebruiken voor toegang tot de variabele moet `$VARIABLENAME`. Bijvoorbeeld, als een waarde die is opgegeven door een positionele parameter als omgevingsvariabele wachtwoord wilt toewijzen, moet u de volgende instructie gebruiken:

    PASSWORD=$1

Daaropvolgende toegang tot de gegevens kunt vervolgens `$PASSWORD`.

Omgevingsvariabelen instellen binnen het script bestaan alleen binnen het bereik van het script. In sommige gevallen moet u wellicht toevoegen hele systeem omgevingsvariabelen die blijft van kracht nadat het script is voltooid. Voor het hele systeem omgevingsvariabelen toevoegen, voeg de variabele `/etc/environment`. Bijvoorbeeld de volgende instructie voegt `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten worden opgeslagen in een van de volgende locaties:

* Een __Azure Storage-account__ dat is gekoppeld aan het cluster.

* Een __extra opslagaccount__ die zijn gekoppeld aan het cluster.

* Een __openbaar leesbaar URI__. Bijvoorbeeld, een URL naar gegevens die zijn opgeslagen op OneDrive of Dropbox andere hostingservice bestand.

* Een __Azure Data Lake Store-account__ dat is gekoppeld aan het HDInsight-cluster. Zie voor meer informatie over het gebruik van Azure Data Lake Store met HDInsight [een HDInsight-cluster maken met Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > De service-principal die hdinsight gebruikt voor toegang tot Data Lake Store moet leestoegang hebben tot het script.

Resources die worden gebruikt door het script moeten ook openbaar beschikbaar zijn.

De bestanden worden opgeslagen in een Azure Storage-account of een Azure Data Lake Store biedt snel toegang, als beide binnen het Azure-netwerk.

> [!NOTE]
> De indeling van de URI verwijzen naar het script is afhankelijk van de service wordt gebruikt. Gebruik voor storage-accounts die zijn gekoppeld aan het HDInsight-cluster, `wasb://` of `wasbs://`. Gebruik voor openbaar leesbaar URI `http://` of `https://`. Gebruik voor Data Lake Store `adl://`.

### <a name="checking-the-operating-system-version"></a>Controleren van de versie van besturingssysteem

Verschillende versies van HDInsight, is afhankelijk van specifieke versies van Ubuntu. Mogelijk zijn er verschillen tussen versies van het besturingssysteem die u in uw script controleren moet. U wilt bijvoorbeeld een binair bestand dat is gebonden aan de versie van Ubuntu installeren.

Gebruiken om te controleren de versie van het besturingssysteem, `lsb_release`. Bijvoorbeeld, is het volgende script laat zien hoe een verwijzing naar een specifieke tar-bestand, afhankelijk van de versie van het besturingssysteem:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Controlelijst voor het implementeren van een scriptactie

Hier volgen de stappen nemen bij het voorbereiden voor het implementeren van een script:

* Plaats de bestanden die de aangepaste scripts op een locatie die toegankelijk is voor de clusterknooppunten tijdens de implementatie bevatten. Bijvoorbeeld, de opslag standaard voor het cluster. Bestanden kunnen ook worden opgeslagen in openbaar leesbaar hosting-services.
* Controleer of het script impotent. In dat geval kunt het script meerdere keren op hetzelfde knooppunt worden uitgevoerd.
* Gebruik een tijdelijk bestand directory map te houden van de gedownloade bestanden gebruikt door de scripts en vervolgens opschonen van nadat scripts hebt uitgevoerd.
* Als de OS-niveau instellingen of configuratiebestanden voor Hadoop-service zijn gewijzigd, kan u HDInsight-services opnieuw starten.

## <a name="runScriptAction"></a>Het uitvoeren van een scriptactie

U kunt scriptacties gebruiken voor het aanpassen van HDInsight-clusters met behulp van de volgende methoden:

* Azure Portal
* Azure PowerShell
* Azure Resource Manager-sjablonen
* De HDInsight .NET SDK.

Zie voor meer informatie over het gebruik van elke methode [het gebruik van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Aangepast script-voorbeelden

Microsoft biedt voorbeeldscripts om onderdelen te installeren op een HDInsight-cluster. Zie de volgende koppelingen voor meer voorbeeld scriptacties.

* [Installeren en gebruiken van Hue op HDInsight-clusters](hdinsight-hadoop-hue-linux.md)
* [Installeren en gebruiken van Solr op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md)
* [Installeren en gebruiken van Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md)
* [Installeren of upgraden van Mono op HDInsight-clusters](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Problemen oplossen

Hieronder vindt u fouten tegenkomen wanneer u met behulp van scripts die u hebt ontwikkeld:

**Fout**: `$'\r': command not found`. Soms gevolgd door `syntax error: unexpected end of file`.

*Oorzaak*: deze fout wordt veroorzaakt wanneer de regels in een script met CRLF eindigen. UNIX-systemen verwacht alleen LF als het beëindigen van de regel.

Dit probleem treedt meestal wanneer het script is gemaakt op een Windows-omgeving, zoals CRLF is een algemene regel die eindigt voor veel teksteditors in Windows.

*Resolutie*: als het is een optie in een teksteditor, selecteert u Unix-indeling of LF voor het beëindigen van de regel. U kunt ook de volgende opdrachten op een Unix-systeem de CRLF wijzigen in een LF:

> [!NOTE]
> De volgende opdrachten zijn grofweg gelijk, omdat ze de regeleinden CRLF naar LF wijzigen moeten. Selecteer een op basis van de hulpprogramma's die beschikbaar zijn op uw systeem.

| Opdracht | Opmerkingen |
| --- | --- |
| `unix2dos -b INFILE` |Het oorspronkelijke bestand is een back-up met een. De extensie BAK |
| `tr -d '\r' < INFILE > OUTFILE` |UITVOERBESTAND bevat een versie met alleen LF uitgangen |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Hiermee wijzigt u het bestand rechtstreeks |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |UITVOERBESTAND bevat een versie met alleen LF uitgangen. |

**Fout**: `line 1: #!/usr/bin/env: No such file or directory`.

*Oorzaak*: deze fout treedt op wanneer het script is opgeslagen als UTF-8 met een Byte Order Mark (BOM).

*Resolutie*: het bestand opslaan als ASCII of als UTF-8 zonder een stuklijst. U kunt ook de volgende opdracht uit op een Linux- of Unix-systeem voor het maken van een bestand zonder de stuklijst:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Vervang `INFILE` met het bestand met de stuklijst. `OUTFILE`moet u een nieuwe bestandsnaam, waarin het script zonder de stuklijst.

## <a name="seeAlso"></a>Volgende stappen

* Meer informatie over hoe [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md)
* Gebruik de [HDInsight .NET SDK-naslaginformatie](https://msdn.microsoft.com/library/mt271028.aspx) voor meer informatie over het maken van .NET-toepassingen die HDInsight beheren
* Gebruik de [HDInsight REST-API](https://msdn.microsoft.com/library/azure/mt622197.aspx) voor meer informatie over het gebruik van REST beheeracties uitvoeren op HDInsight-clusters.
