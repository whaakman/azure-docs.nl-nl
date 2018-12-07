---
title: Controleren en beheren van Hadoop met Ambari REST-API - Azure HDInsight
description: Leer hoe u Ambari gebruiken om te controleren en beheren van Hadoop-clusters in Azure HDInsight. In dit document leert u hoe u de Ambari REST-API met HDInsight-clusters worden opgenomen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 08b8d980b46ccbbce47c4f165a621daa14a95fa2
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012909"
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>HDInsight-clusters beheren met behulp van de Ambari REST-API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Leer hoe u de Ambari REST-API gebruiken om te beheren en bewaken van Hadoop-clusters in Azure HDInsight.

Apache Ambari vereenvoudigt het beheer en bewaking van een Hadoop-cluster met een eenvoudige web-UI en de REST-API gebruiken. Ambari is opgenomen in HDInsight-clusters die gebruikmaken van de Linux-besturingssysteem. U kunt Ambari gebruiken om te controleren van het cluster en configuratiewijzigingen aanbrengen.

## <a id="whatis"></a>Wat is Ambari

[Apache Ambari](http://ambari.apache.org) biedt web-UI die kan worden gebruikt om te beheren en bewaken van Hadoop-clusters. Ontwikkelaars kunnen deze mogelijkheden integreren in hun toepassingen met behulp van de [Ambari REST-API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari wordt geboden door standaard met HDInsight op basis van Linux-clusters.

## <a name="how-to-use-the-ambari-rest-api"></a>Het gebruik van de Ambari REST-API

> [!IMPORTANT]
> De informatie en voorbeelden in dit document moeten een HDInsight-cluster dat gebruik maakt van Linux-besturingssysteem. Zie voor meer informatie, [aan de slag met HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

De voorbeelden in dit document zijn beschikbaar voor zowel de Bourne shell (bash) en PowerShell. De bash-voorbeelden zijn getest met GNU bash versie 4.3.11, maar moeten samen met andere shells Unix. De PowerShell-voorbeelden zijn getest met PowerShell 5.0, maar moeten werken met PowerShell 3.0 of hoger.

Als de __Bourne shell__ (Bash), hebt u het volgende zijn geïnstalleerd:

* [cURL](https://curl.haxx.se/): cURL is een hulpprogramma dat kan worden gebruikt om te werken met REST-API's vanaf de opdrachtregel. In dit document, wordt deze gebruikt om te communiceren met de Ambari REST-API.

Of u Bash of PowerShell gebruikt, moet u ook hebt [jq](https://stedolan.github.io/jq/) geïnstalleerd. Jq is een hulpprogramma voor het werken met JSON-documenten. Het wordt gebruikt in **alle** de voorbeelden van Bash en **één** van de PowerShell-voorbeelden.

### <a name="base-uri-for-ambari-rest-api"></a>Basis-URI voor de Ambari Rest-API

De basis-URI voor de Ambari REST-API op HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!IMPORTANT]
> De naam van het cluster in het gedeelte van de FQDN-naam (Fully Qualified Domain Name) van de URI (CLUSTERNAME.azurehdinsight.net) is niet hoofdlettergevoelig, zijn andere exemplaren in de URI zijn hoofdlettergevoelig. Bijvoorbeeld, als de naam van uw cluster `MyCluster`, de volgende geldige URI's zijn:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> De volgende URI's, wordt er een fout geretourneerd omdat het tweede exemplaar van de naam is niet hoofdlettergevoelig.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Verificatie

Verbinding maken met Ambari op HDInsight vereist HTTPS. De accountnaam van beheerder gebruiken (de standaardwaarde is **admin**) en het wachtwoord die u hebt opgegeven tijdens het maken van clusters.

## <a name="examples-authentication-and-parsing-json"></a>Voorbeelden: Verificatie- en JSON parseren

De volgende voorbeelden laten zien hoe u een GET-aanvraag indient voor de basis Ambari REST-API:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> De Bash-voorbeelden in dit document moeten u de volgende veronderstellingen:
>
> * De aanmeldingsnaam voor het cluster is de standaardwaarde van `admin`.
> * `$CLUSTERNAME` bevat de naam van het cluster. U kunt deze waarde instellen met behulp van `set CLUSTERNAME='clustername'`
> * Voer desgevraagd het wachtwoord voor de clusteraanmelding (beheerder).

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> De PowerShell-voorbeelden in dit document moeten u de volgende veronderstellingen:
>
> * `$creds` is een referentieobject met de beheerdersaanmelding bij en het wachtwoord voor het cluster. U kunt deze waarde instellen met behulp van `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` en referenties wanneer hierom wordt gevraagd.
> * `$clusterName` is een tekenreeks zijn met de naam van het cluster. U kunt deze waarde instellen met behulp van `$clusterName="clustername"`.

Beide voorbeelden retourneert een JSON-document dat met informatie uit die lijkt op het volgende voorbeeld begint:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>Het parseren van JSON-gegevens

Het volgende voorbeeld wordt `jq` voor de JSON-antwoorddocument parseren en weergeven van alleen de `health_report` gegevens uit de resultaten.

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 en hoger biedt de `ConvertFrom-Json` cmdlet, waarmee het JSON-document naar een object dat is het eenvoudiger om te werken converteert met vanuit PowerShell. Het volgende voorbeeld wordt `ConvertFrom-Json` om weer te geven alleen de `health_report` gegevens uit de resultaten.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Hoewel de meeste voorbeelden in dit document `ConvertFrom-Json` om elementen uit het antwoorddocument te geven de [Update Ambari configuratie](#example-update-ambari-configuration) praktijkvoorbeelden jq. Jq wordt in dit voorbeeld gebruikt om een nieuwe sjabloon uit de JSON-antwoorddocument samen te stellen.

Zie voor een volledig overzicht van de REST-API, [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Voorbeeld: De FQDN-naam van de clusterknooppunten ophalen

Als u werkt met HDInsight, moet u mogelijk kent de volledig gekwalificeerde domeinnaam (FQDN) van een clusterknooppunt. U kunt eenvoudig de FQDN-naam voor de verschillende knooppunten in het cluster met behulp van de volgende voorbeelden ophalen:

* **Alle knooppunten**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **Hoofdknooppunten**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Worker-knooppunten**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Zookeeper-knooppunten**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Voorbeeld: Het interne IP-adres van de clusterknooppunten ophalen

> [!IMPORTANT]
> De IP-adressen die wordt geretourneerd door de voorbeelden in deze sectie zijn niet rechtstreeks toegankelijk via internet. Ze zijn alleen toegankelijk binnen het virtuele Azure-netwerk waarin het HDInsight-cluster.
>
> Zie voor meer informatie over het werken met HDInsight en virtuele netwerken [mogelijkheden voor HDInsight uitbreiden met behulp van een aangepaste Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

Als u het IP-adres zoekt, moet u de interne volledig gekwalificeerde domeinnaam (FQDN) van de clusterknooppunten weten. Zodra u de FQDN-naam hebt, krijgt u vervolgens het IP-adres van de host. De volgende voorbeelden eerst Ambari op te vragen voor de FQDN-naam van alle knooppunten van de host en vervolgens Ambari op te vragen voor het IP-adres van elke host.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

> [!TIP]
> Vorige voorbeelden wordt u gevraagd om het wachtwoord. In dit voorbeeld wordt uitgevoerd de `curl` meerdere keren opdracht zodat het wachtwoord is opgegeven als `$PASSWORD` om te voorkomen dat meerdere stappen.

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-the-default-storage"></a>Voorbeeld: De standaardopslag ophalen

Wanneer u een HDInsight-cluster maakt, moet u een Azure Storage-Account of een Data Lake Store als standaardopslag gebruiken voor het cluster. Ambari kunt u deze informatie ophalen nadat het cluster is gemaakt. Bijvoorbeeld, als u wilt lezen/schrijven van gegevens naar de container buiten HDInsight.

De standaardconfiguratie voor opslag ophalen in de volgende voorbeelden uit het cluster:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Deze voorbeelden retourneren de eerste configuratie toegepast op de server (`service_config_version=1`) die deze informatie bevat. Als u een waarde die is gewijzigd na het maken van een cluster kunt ophalen, moet u de configuratieversies weergeven en ophalen van de meest recente versie.

De geretourneerde waarde is vergelijkbaar met een van de volgende voorbeelden:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` -Deze waarde geeft aan dat het cluster een Azure Storage-account wordt gebruikt voor standaardopslag. De `ACCOUNTNAME` waarde is de naam van het opslagaccount. De `CONTAINER` gedeelte is de naam van de blob-container in het opslagaccount. De container is de hoofdmap van het HDFS-compatibele opslag voor het cluster.

* `adl://home` -Deze waarde geeft aan dat het cluster een Azure Data Lake Store wordt gebruikt voor standaardopslag.

    Als u wilt zoeken in de naam van de Data Lake Store-account, moet u de volgende voorbeelden gebruiken:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    De geretourneerde waarde is vergelijkbaar met `ACCOUNTNAME.azuredatalakestore.net`, waarbij `ACCOUNTNAME` is de naam van het Data Lake Store-account.

    Als u zoekt de map in Data Lake Store met de opslag voor het cluster, moet u de volgende voorbeelden gebruiken:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    De geretourneerde waarde is vergelijkbaar met `/clusters/CLUSTERNAME/`. Deze waarde is een pad in het Data Lake Store-account. Dit pad is de hoofdmap van de compatibele HDFS-bestandssysteem voor het cluster. 

> [!NOTE]
> De `Get-AzureRmHDInsightCluster` cmdlet geleverd door [Azure PowerShell](/powershell/azure/overview) geeft ook de opslaginformatie voor het cluster.


## <a name="example-get-configuration"></a>Voorbeeld: Get-configuratie

1. Haal de configuraties die beschikbaar voor uw cluster zijn.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    In dit voorbeeld retourneert een JSON-document met de huidige configuratie (aangeduid met de *tag* waarde) voor de onderdelen worden geïnstalleerd op het cluster. Het volgende voorbeeld wordt een fragment uit de gegevens die worden geretourneerd van een Spark-cluster.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. Opvragen van de configuratie voor het onderdeel dat u geïnteresseerd bent in. Vervang in het volgende voorbeeld wordt `INITIAL` geretourneerd met de waarde van het label van de vorige aanvraag.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    In dit voorbeeld retourneert een JSON-document met de huidige configuratie voor de `core-site` onderdeel.

## <a name="example-update-configuration"></a>Voorbeeld: Update-configuratie

1. Haal de huidige configuratie, waarbij Ambari worden opgeslagen als de 'gewenste configuratie':

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    In dit voorbeeld retourneert een JSON-document met de huidige configuratie (aangeduid met de *tag* waarde) voor de onderdelen worden geïnstalleerd op het cluster. Het volgende voorbeeld wordt een fragment uit de gegevens die worden geretourneerd van een Spark-cluster.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    In deze lijst, moet u de naam van het onderdeel kopiëren (bijvoorbeeld **spark\_thrift\_sparkconf** en de **tag** waarde.

2. De configuratie voor het onderdeel en de tag ophalen met behulp van de volgende opdrachten:
   
    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > Vervang **spark-thrift-sparkconf** en **INITIËLE** met het onderdeel en de code die u wilt ophalen van de configuratie voor.
   
    Jq wordt gebruikt voor het inschakelen van de gegevens opgehaald uit HDInsight in een nieuwe configuratiesjabloon. Deze voorbeelden wordt met name de volgende acties uitvoeren:
   
    * Hiermee maakt u een unieke waarde met de tekenreeks 'versie' en de datum die is opgeslagen in `newtag`.

    * Hiermee maakt u een document hoofdmap voor de nieuwe gewenste configuratie.

    * Met deze eigenschap wordt de inhoud van de `.items[]` matrix en voegt deze onder de **desired_config** element.

    * Hiermee verwijdert u de `href`, `version`, en `Config` elementen, als deze elementen niet nodig zijn om in te dienen een nieuwe configuratie.

    * Voegt een `tag` -element met een waarde van `version#################`. Het numerieke gedeelte is gebaseerd op de huidige datum. Elke configuratie moet een unieke code hebben.
     
    Ten slotte de gegevens worden opgeslagen in de `newconfig.json` document. De documentstructuur ziet er ongeveer als volgt uitzien:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Open de `newconfig.json` waarden document en wijzigen/toevoegen in de `properties` object. Het volgende voorbeeld wordt de waarde van `"spark.yarn.am.memory"` van `"1g"` naar `"3g"`. Ook wordt toegevoegd `"spark.kryoserializer.buffer.max"` met een waarde van `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Sla het bestand wanneer u klaar bent en wijzigingen.

4. Gebruik de volgende opdrachten om in te dienen de bijgewerkte configuratie van Ambari.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Deze opdrachten verzenden de inhoud van de **newconfig.json** bestand aan het cluster als de nieuwe gewenste configuratie. De aanvraag retourneert een JSON-document. De **versionTag** -element in dit document moet overeenkomen met de versie die u hebt ingediend, en de **peeringconfiguraties** -object bevat de wijzigingen van de configuratie die u hebt aangevraagd.

### <a name="example-restart-a-service-component"></a>Voorbeeld: Een serviceonderdeel opnieuw starten

Op dit moment als u de Ambari-Webgebruikersinterface bekijkt, het Spark-service geeft aan dat deze opnieuw worden opgestart moet voordat de nieuwe configuratie van kracht. Gebruik de volgende stappen uit de service opnieuw te starten.

1. Schakel de onderhoudsmodus voor de Spark-service gebruikt u de volgende:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Deze opdrachten wordt een JSON-document verzenden naar de server die ingeschakeld in de onderhoudsmodus wordt. U kunt controleren of de service is nu in de onderhoudsmodus met behulp van de volgende aanvraag:
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    De geretourneerde waarde `ON`.

2. Vervolgens gebruikt u de volgende de service uitschakelen:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    Het antwoord is vergelijkbaar met het volgende voorbeeld:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > De `href` waarde die door deze URI geretourneerd met behulp van het interne IP-adres van het clusterknooppunt. Voor het gebruik van buiten het cluster, kunt u het gedeelte '10.0.0.18:8080' vervangen door de FQDN-naam van het cluster. 
    
    De volgende opdrachten ophalen de status van de aanvraag:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Een reactie van `COMPLETED` geeft aan dat de aanvraag is voltooid.

3. Nadat de vorige aanvraag is voltooid, moet u het volgende gebruiken om de service te starten.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    De service maakt nu gebruik van de nieuwe configuratie.

4. Gebruik tot slot de volgende uitschakelen in de onderhoudsmodus.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledig overzicht van de REST-API, [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

