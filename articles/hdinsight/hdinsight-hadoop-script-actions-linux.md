---
title: Ontwikkeling van scriptacties met HDInsight op basis van Linux - Azure
description: Informatie over het gebruik van Bash-scripts naar HDInsight op basis van Linux-clusters aanpassen. Het script action-functie van HDInsight kunt u uitvoeren van scripts tijdens of na het maken van clusters. Scripts kunnen worden gebruikt om de cluster-configuratie-instellingen wijzigen of andere software geïnstalleerd.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: jasonh
ms.openlocfilehash: 35d54970e41a7e1533f3d5adc09fc60dc1bbd68d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600486"
---
# <a name="script-action-development-with-hdinsight"></a>Ontwikkeling van scriptacties met HDInsight

Informatie over het aanpassen van uw HDInsight-cluster met behulp van Bash-scripts. Scriptacties zijn een manier om aan te passen HDInsight tijdens of na het maken van clusters.

> [!IMPORTANT]
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="what-are-script-actions"></a>Wat zijn scriptacties

Scriptacties zijn Bash-scripts die door Azure wordt uitgevoerd op de clusterknooppunten op wijzigingen in de configuratie maken of software installeren. Een scriptactie wordt uitgevoerd als hoofdmap en biedt volledige toegangsrechten voor de clusterknooppunten.

Scriptacties kunnen worden toegepast met de volgende methoden:

| Gebruik deze methode om een script toepassen... | Tijdens het cluster maken... | Op een cluster wordt uitgevoerd... |
| --- |:---:|:---:|
| Azure Portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure CLI 1.0 |&nbsp; |✓ |
| HDInsight .NET-SDK |✓ |✓ |
| Azure Resource Manager-sjabloon |✓ |&nbsp; |

Zie voor meer informatie over het gebruik van deze methoden om toe te passen scriptacties [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Aanbevolen procedures voor het ontwikkelen van script

Wanneer u een aangepast script voor een HDInsight-cluster ontwikkelt, zijn er enkele aanbevolen procedures waarmee u rekening moet houden:

* [Doel van de Hadoop-versie](#bPS1)
* [Doel van de versie van het besturingssysteem](#bps10)
* [Stabiele koppelingen naar scriptbronnen](#bPS2)
* [Gebruik vooraf gecompileerde bronnen](#bPS4)
* [Zorg ervoor dat het cluster aanpassing-script idempotent is](#bPS3)
* [Zorg voor hoge beschikbaarheid van de clusterarchitectuur](#bPS5)
* [Configureren van de aangepaste onderdelen voor het gebruik van Azure Blob-opslag](#bPS6)
* [Schrijven van gegevens in de STDOUT en STDERR](#bPS7)
* [ASCII-bestanden opslaan met LF regeleinden](#bps8)
* [Logica voor opnieuw proberen gebruiken om te herstellen van tijdelijke fouten](#bps9)

> [!IMPORTANT]
> Scriptacties moeten worden voltooid binnen 60 minuten of het proces is mislukt. Het script wordt uitgevoerd tijdens het inrichten van knooppunt, samen met andere processen instellen en configureren. Concurrentie voor resources, zoals CPU-tijd of netwerk bandbreedte kan ertoe leiden dat het script duurt langer dan het geval in uw ontwikkelingsomgeving is voltooien.

### <a name="bPS1"></a>Doel van de Hadoop-versie

Verschillende versies van HDInsight hebben verschillende versies van Hadoop-services en onderdelen worden geïnstalleerd. Als het script wordt een specifieke versie van een service of het onderdeel verwacht, moet u het script alleen gebruiken met de versie van HDInsight met de vereiste onderdelen. U vindt informatie over de onderdeel-versies die zijn opgenomen in HDInsight met behulp van de [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md) document.

### <a name="bps10"></a> Doel van de versie van het besturingssysteem

HDInsight op basis van Linux is gebaseerd op de Ubuntu Linux-distributie. Verschillende versies van HDInsight, is afhankelijk van verschillende versies van Ubuntu, die het gedrag van uw script kan wijzigen. Bijvoorbeeld: HDInsight 3.4 en eerdere versies zijn gebaseerd op Ubuntu-versies die Upstart gebruiken. Versie 3.5 en hoger zijn gebaseerd op Ubuntu 16.04, dat gebruikmaakt van Systemd. Systemd en Upstart zijn afhankelijk van verschillende opdrachten, zodat het script moet worden geschreven naar het werken met beide.

Een ander belangrijk verschil tussen HDInsight 3.4 en 3.5 is dat `JAVA_HOME` verwijst nu naar Java 8.

U kunt de versie van het besturingssysteem controleren met behulp van `lsb_release`. De volgende code toont hoe om te bepalen of het script wordt uitgevoerd op Ubuntu 14 of 16:

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

U vindt de volledige script waarin deze fragmenten op https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Zie voor de versie van Ubuntu die wordt gebruikt door HDInsight, de [HDInsight componentversie](hdinsight-component-versioning.md) document.

Zie voor meer informatie over de verschillen tussen Systemd en Upstart, [Systemd voor Upstart gebruikers](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Stabiele koppelingen naar scriptbronnen

Het script en de bijbehorende resources moeten blijven beschikbaar tijdens de levensduur van het cluster. Deze resources zijn vereist als de nieuwe knooppunten worden toegevoegd aan het cluster tijdens het schalen herverdelen.

De aanbevolen procedure is om te downloaden en te archiveren alles in een Azure Storage-account voor uw abonnement.

> [!IMPORTANT]
> Het opslagaccount dat wordt gebruikt, moet het standaardopslagaccount voor het cluster of een openbaar, alleen-lezen-container op een andere storage-account.

Bijvoorbeeld, de voorbeelden die is geleverd door Microsoft worden opgeslagen in de [ https://hdiconfigactions.blob.core.windows.net/ ](https://hdiconfigactions.blob.core.windows.net/) storage-account. Deze locatie is een openbaar, alleen-lezen-container onderhouden door het HDInsight-team.

### <a name="bPS4"></a>Gebruik vooraf gecompileerde bronnen

Voorkom bewerkingen die bronnen uit de broncode compileren, verminderen de tijd die nodig zijn om uit te voeren van het script. Bijvoorbeeld, vooraf compileren resources en op te slaan in een Azure Storage-account-blob in hetzelfde Datacenter als HDInsight.

### <a name="bPS3"></a>Zorg ervoor dat het cluster aanpassing-script idempotent is

Scripts moeten idempotent zijn. Als het script wordt meerdere keren uitgevoerd, moet deze het cluster retourneren naar dezelfde toestand telkens.

Bijvoorbeeld, dat een script dat Hiermee wijzigt u configuratiebestanden niet dubbele vermeldingen moet toevoegen als meerdere keren uitgevoerd.

### <a name="bPS5"></a>Zorg voor hoge beschikbaarheid van de clusterarchitectuur

Linux gebaseerde HDInsight-clusters bieden twee hoofdknooppunten die actief zijn binnen het cluster en scriptacties uitgevoerd op beide knooppunten. Als de onderdelen die u installeert slechts één hoofdknooppunt verwacht, moet u niet de onderdelen installeren op beide hoofdknooppunten.

> [!IMPORTANT]
> Services die worden geleverd als onderdeel van HDInsight zijn ontworpen voor failover tussen de twee hoofdknooppunten, indien nodig. Deze functionaliteit is niet uitgebreid naar aangepaste onderdelen zijn geïnstalleerd via scriptacties. Als u hoge beschikbaarheid voor aangepaste onderdelen nodig hebt, moet u uw eigen failover-mechanisme implementeren.

### <a name="bPS6"></a>Configureren van de aangepaste onderdelen voor het gebruik van Azure Blob-opslag

Onderdelen die u op het cluster installeert wellicht een standaardconfiguratie die gebruikmaakt van Hadoop Distributed File System (HDFS)-opslag. HDInsight maakt gebruik van Azure Storage of Data Lake Store als standaardopslag. Beide bieden een HDFS compatibele-bestandssysteem dat gegevens actief blijft zelfs als het cluster wordt verwijderd. U wilt configureren van onderdelen die u installeert voor het gebruik van WASB of ADL in plaats van HDFS.

Voor de meeste bewerkingen hoeft u niet om op te geven van het bestandssysteem. Bijvoorbeeld, kopieert de volgende de giraph-examples.jar bestand uit het lokale bestandssysteem aan het cluster:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

In dit voorbeeld wordt de `hdfs` opdracht transparant maakt gebruik van de standaardopslag voor het cluster. Voor bepaalde bewerkingen moet u mogelijk de URI opgeven. Bijvoorbeeld, `adl:///example/jars` voor Data Lake Store of `wasb:///example/jars` voor Azure Storage.

### <a name="bPS7"></a>Schrijven van gegevens in de STDOUT en STDERR

HDInsight-logboeken die worden geschreven naar de STDOUT en STDERR-scriptuitvoer. U kunt deze informatie met behulp van de Ambari-Webgebruikersinterface kunt weergeven.

> [!NOTE]
> Ambari is alleen beschikbaar als het cluster is gemaakt. Als u een scriptactie tijdens het maken van een cluster, en mislukt het maken, Zie de sectie probleemoplossing [aanpassen HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) voor andere manieren om toegang tot geregistreerde gegevens.

De meeste hulpprogramma's en installatiepakketten schrijven al gegevens naar STDOUT en STDERR, maar u kunt aanvullende logboekregistratie toevoegen. Gebruiken voor het verzenden van tekst naar STDOUT, `echo`. Bijvoorbeeld:

```bash
echo "Getting ready to install Foo"
```

Standaard `echo` verzendt de tekenreeks naar STDOUT. Als u wilt de opdracht geven stderr, toevoegen `>&2` voordat `echo`. Bijvoorbeeld:

```bash
>&2 echo "An error occurred installing Foo"
```

Dit leidt informatie geschreven naar de STDOUT, stderr (2) in plaats daarvan. Zie voor meer informatie over i/o-omleiding [ http://www.tldp.org/LDP/abs/html/io-redirection.html ](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Zie voor meer informatie over het weergeven van informatie die wordt geregistreerd door scriptacties [aanpassen HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a> ASCII-bestanden opslaan met LF regeleinden

Bash-scripts moeten worden opgeslagen als ASCII-indeling met lijnen door LF beëindigd. Bestanden die zijn opgeslagen als UTF-8 of CRLF gebruiken als het einde van de regel kunnen mislukken met de volgende fout:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a> Logica voor opnieuw proberen gebruiken om te herstellen van tijdelijke fouten

Bij het downloaden van bestanden, het installeren van pakketten met apt-get, of andere bewerkingen die gegevens via het internet verzenden, wordt de actie kan mislukken door tijdelijke netwerkfouten. De externe resource die u wilt communiceren kan bijvoorbeeld zijn momenteel failover wordt uitgevoerd naar een back-knooppunt.

Als u wilt uw script tegen tijdelijke fouten, kunt u de logica voor opnieuw proberen implementeren. De volgende functie ziet u hoe u logica voor opnieuw proberen implementeren. Het opnieuw probeert de bewerking drie keer voordat deze is mislukt.

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

De volgende voorbeelden laten zien hoe u deze functie wilt gebruiken.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Help-methoden voor aangepaste scripts

Script actie helpermethoden zijn hulpprogramma's die u gebruiken kunt tijdens het schrijven van aangepaste scripts. Deze methoden zijn opgenomen in de[ https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh ](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script. Gebruik de volgende om te downloaden en ze als onderdeel van uw script gebruiken:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

De volgende hulpprogramma's beschikbaar voor gebruik in uw script:

| Helper-gebruik | Beschrijving |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Downloadt een bestand van de bron-URI naar het opgegeven pad. Standaard wordt er een bestaand bestand niet overschreven. |
| `untar_file TARFILE DESTDIR` |Haalt een tar-bestand (met behulp van `-xf`) naar de doelmap. |
| `test_is_headnode` |Als die worden uitgevoerd op een cluster hoofdknooppunt, return 1; anders wordt is 0. |
| `test_is_datanode` |Als het huidige knooppunt is een gegevensknooppunt (werkrol), als resultaat een 1; anders wordt is 0. |
| `test_is_first_datanode` |Als het huidige knooppunt is de eerste gegevens (worker) knooppunt (met de naam workernode0) retourneren een 1; anders wordt is 0. |
| `get_headnodes` |Retourneert de volledig gekwalificeerde domeinnaam van de hoofdknooppunten in het cluster. Namen worden gescheiden door komma's. Een lege tekenreeks wordt geretourneerd bij fout. |
| `get_primary_headnode` |Hiermee haalt u de volledig gekwalificeerde domeinnaam van het primaire hoofdknooppunt. Een lege tekenreeks wordt geretourneerd bij fout. |
| `get_secondary_headnode` |Hiermee haalt u de volledig gekwalificeerde domeinnaam van het secundaire hoofdknooppunt. Een lege tekenreeks wordt geretourneerd bij fout. |
| `get_primary_headnode_number` |Hiermee haalt u de numeriek achtervoegsel van het primaire hoofdknooppunt. Een lege tekenreeks wordt geretourneerd bij fout. |
| `get_secondary_headnode_number` |Hiermee haalt u de numeriek achtervoegsel van het secundaire hoofdknooppunt. Een lege tekenreeks wordt geretourneerd bij fout. |

## <a name="commonusage"></a>Algemene gebruikspatronen

Deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die u kunt tegenkomen tijdens het schrijven van uw eigen aangepaste scripts.

### <a name="passing-parameters-to-a-script"></a>Parameters doorgeven aan een script

In sommige gevallen kan het script parameters vereist. Bijvoorbeeld, u moet mogelijk het beheerderswachtwoord voor het cluster bij het gebruik van de Ambari REST-API.

Parameters doorgegeven aan het script worden aangeduid als *positionele parameters*, en zijn toegewezen aan `$1` voor de eerste parameter, `$2` voor het tweede, en dus beschikbaar. `$0` bevat de naam van het script zelf.

Waarden doorgegeven aan het script als parameters moeten tussen enkele aanhalingstekens ('). Hiermee zorgt u ervoor dat de doorgegeven waarde wordt beschouwd als een letterlijke tekenreeks.

### <a name="setting-environment-variables"></a>Omgevingsvariabelen instellen

Instellen van een omgevingsvariabele wordt uitgevoerd door de volgende instructie:

    VARIABLENAME=value

VARIABLENAME is waar de naam van de variabele. Gebruiken voor toegang tot de variabele, `$VARIABLENAME`. Bijvoorbeeld, als u wilt een waarde die is geleverd door een positionele parameter als een omgevingsvariabele met de naam wachtwoord toewijst, moet u de volgende instructie gebruiken:

    PASSWORD=$1

Daaropvolgende toegang tot de informatie kan vervolgens gebruiken `$PASSWORD`.

Omgevingsvariabelen instellen in het script bestaan alleen binnen het bereik van het script. In sommige gevallen moet u mogelijk toevoegen systeembrede omgevingsvariabelen die behouden blijven nadat het script is voltooid. Om toe te voegen systeembrede omgevingsvariabelen, voeg de variabele `/etc/environment`. Voegt bijvoorbeeld de volgende instructie toe `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten worden opgeslagen in een van de volgende locaties:

* Een __Azure Storage-account__ dat is gekoppeld aan het cluster.

* Een __extra opslagaccount__ die zijn gekoppeld aan het cluster.

* Een __openbaar leesbare URI__. Bijvoorbeeld, een URL naar gegevens die zijn opgeslagen in OneDrive, Dropbox of andere bestandshostingservice.

* Een __Azure Data Lake Store-account__ dat is gekoppeld aan het HDInsight-cluster. Zie voor meer informatie over het gebruik van Azure Data Lake Store met HDInsight [Quick Start: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

    > [!NOTE]
    > De service-principal die HDInsight gebruikt voor toegang tot Data Lake Store moet leestoegang hebben tot het script.

Resources die worden gebruikt door het script moeten ook openbaar beschikbaar zijn.

De bestanden opslaan in een Azure Storage-account of een Azure Data Lake Store biedt snel toegang, als zowel binnen het Azure-netwerk.

> [!NOTE]
> De URI-indeling die wordt gebruikt om te verwijzen naar het script verschilt afhankelijk van de service wordt gebruikt. Voor storage-accounts die zijn gekoppeld aan het HDInsight-cluster, gebruikt u `wasb://` of `wasbs://`. Gebruik voor openbaar leesbare URI's, `http://` of `https://`. Gebruik voor Data Lake Store, `adl://`.

### <a name="checking-the-operating-system-version"></a>De versie van besturingssysteem controleren

Verschillende versies van HDInsight, is afhankelijk van specifieke versies van Ubuntu. Mogelijk zijn er verschillen tussen versies van het besturingssysteem die u in het script moet controleren. U wilt bijvoorbeeld een binair bestand dat is gekoppeld aan de versie van Ubuntu installeren.

Gebruiken om te controleren of de versie van het besturingssysteem, `lsb_release`. Bijvoorbeeld, is het volgende script laat zien hoe u verwijzen naar een specifieke tar-bestand, afhankelijk van de versie van het besturingssysteem:

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

Hier volgen de stappen nemen bij de voorbereiding om een script te implementeren:

* Plaats de bestanden die de aangepaste scripts in een locatie die toegankelijk is voor de clusterknooppunten is tijdens de implementatie bevatten. Bijvoorbeeld, de standaardopslag voor het cluster. Bestanden kunnen ook worden opgeslagen in een openbaar leesbare hosting-services.
* Controleer of het script is idempotent zijn. In dat geval kunt het script meerdere keren op hetzelfde knooppunt worden uitgevoerd.
* Gebruik een map/Tmp directory tijdelijk bestand te houden van de gedownloade bestanden die worden gebruikt door de scripts en schoon ze nadat scripts hebt uitgevoerd.
* Als het niveau van de OS of Hadoop-service-configuratiebestanden zijn gewijzigd, kunt u HDInsight-services opnieuw starten.

## <a name="runScriptAction"></a>Het uitvoeren van een scriptactie

U kunt scriptacties gebruiken om aan te passen van HDInsight-clusters met behulp van de volgende methoden:

* Azure Portal
* Azure PowerShell
* Azure Resource Manager-sjablonen
* De HDInsight .NET SDK.

Zie voor meer informatie over het gebruik van elke methode [over het gebruik van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Voorbeelden van aangepaste scripts

Microsoft biedt voorbeelden van scripts om onderdelen te installeren op een HDInsight-cluster. Zie de volgende koppelingen voor meer voorbeeld scriptacties.

* [Installeren en Hue gebruiken op HDInsight-clusters](hdinsight-hadoop-hue-linux.md)
* [Solr installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md)
* [Giraph installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md)
* [Installeren of upgraden van Mono op HDInsight-clusters](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Problemen oplossen

Hier volgen de fouten die optreden kunnen bij het gebruik van scripts die u hebt ontwikkeld:

**Fout**: `$'\r': command not found`. Soms wordt gevolgd door `syntax error: unexpected end of file`.

*Oorzaak*: deze fout wordt veroorzaakt wanneer de regels in een script CRLF eindigen. UNIX-systemen verwacht alleen LF als het einde van regel.

Dit probleem treedt meestal wanneer het script is geschreven in een Windows-omgeving, zoals CRLF is een algemene regel beëindigen voor veel teksteditors op Windows.

*Resolutie*: als deze optie in een teksteditor, selecteert u Unix-indeling of LF voor het einde van regel. U kunt ook de volgende opdrachten gebruiken voor een Unix-systeem de CRLF wijzigen in een LF:

> [!NOTE]
> De volgende opdrachten zijn grofweg gelijk in dat ze moeten de regeleinden CRLF LF wijzigen. Selecteer een op basis van de hulpprogramma's die beschikbaar zijn op uw systeem.

| Opdracht | Opmerkingen |
| --- | --- |
| `unix2dos -b INFILE` |Het oorspronkelijke bestand is opgeslagen met een. BAK-extensie |
| `tr -d '\r' < INFILE > OUTFILE` |UITVOERBESTAND bevat een versie met alleen LF uitgangen |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Hiermee wijzigt u het bestand rechtstreeks |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |UITVOERBESTAND bevat een versie met alleen-uitgangen LF. |

**Fout**: `line 1: #!/usr/bin/env: No such file or directory`.

*Oorzaak*: deze fout treedt op wanneer het script is opgeslagen als UTF-8 met een Byte Order Mark (BOM).

*Resolutie*: sla het bestand als ASCII- of als UTF-8 zonder een stuklijst. U kunt ook de volgende opdracht uit op een Linux- of Unix-systeem te maken van een bestand zonder de stuklijst:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Vervang `INFILE` met het bestand met de stuklijst. `OUTFILE` moet een nieuwe bestandsnaam, waarin het script zonder de stuklijst.

## <a name="seeAlso"></a>Volgende stappen

* Meer informatie over het [aanpassen HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md)
* Gebruik de [HDInsight .NET SDK-verwijzing](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight) voor meer informatie over het maken van .NET-toepassingen die HDInsight beheren
* Gebruik de [HDInsight REST-API](https://msdn.microsoft.com/library/azure/mt622197.aspx) voor informatie over het gebruik van REST beheeracties uitvoeren op HDInsight-clusters.
