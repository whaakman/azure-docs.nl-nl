---
title: Bewaken en beheren van Hadoop met Ambari REST-API - Azure HDInsight | Microsoft Docs
description: Informatie over het Ambari gebruiken om te controleren en beheren in Azure HDInsight Hadoop-clusters. In dit document leert u hoe u de Ambari REST-API die deel uitmaakt van een HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 7960d83bce22d4f671d61e9aaf55561bc24308f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>HDInsight-clusters beheren met behulp van de Ambari REST-API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Informatie over het gebruik van de Ambari REST-API te beheren en bewaken in Azure HDInsight Hadoop-clusters.

Apache Ambari vereenvoudigt het beheer en bewaking van een Hadoop-cluster met een eenvoudige web-UI en REST-API gebruiken. Ambari is opgenomen op HDInsight-clusters die gebruikmaken van de Linux-besturingssysteem. U kunt Ambari gebruiken om te controleren van het cluster en configuratiewijzigingen aanbrengen.

## <a id="whatis"></a>Wat is Ambari

[Apache Ambari](http://ambari.apache.org) biedt webgebruikersinterface die kunnen worden gebruikt voor het inrichten, beheren en controleren van Hadoop-clusters. Ontwikkelaars kunnen deze mogelijkheden integreren in hun toepassingen met behulp van de [Ambari REST-API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari is opgegeven met HDInsight op basis van Linux-clusters standaard.

## <a name="how-to-use-the-ambari-rest-api"></a>Het gebruik van de Ambari REST-API

> [!IMPORTANT]
> De informatie en voorbeelden in dit document vereisen een HDInsight-cluster dat gebruik maakt van Linux-besturingssysteem. Zie voor meer informatie [aan de slag met HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

De voorbeelden in dit document zijn beschikbaar voor zowel de Willemsen shell (bash) en PowerShell. De voorbeelden zijn getest met GNU bash 4.3.11 bash, maar moet samen met andere houders Unix. De PowerShell-voorbeelden zijn getest met PowerShell 5.0, maar moeten samenwerken met PowerShell 3.0 of hoger.

Als de __Bourne shell__ (Bash), hebt u het volgende zijn geïnstalleerd:

* [cURL](http://curl.haxx.se/): cURL is een hulpprogramma waarmee kan worden gebruikt voor het werken met de REST-API's vanaf de opdrachtregel. In dit document, wordt deze gebruikt om te communiceren met de Ambari REST-API.

Of Bash of PowerShell gebruikt, moet u ook hebt [jq](https://stedolan.github.io/jq/) geïnstalleerd. Jq is een hulpprogramma voor het werken met JSON-documenten. Het wordt gebruikt in **alle** de voorbeelden Bash en **één** van de PowerShell-voorbeelden.

### <a name="base-uri-for-ambari-rest-api"></a>Basis-URI voor de Ambari Rest API

De basis-URI voor de Ambari REST-API op HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!IMPORTANT]
> De naam van het cluster in het gedeelte van de FQDN-naam (Fully Qualified Domain Name) van de URI (CLUSTERNAME.azurehdinsight.net) is niet hoofdlettergevoelig, zijn andere exemplaren in de URI zijn hoofdlettergevoelig. Bijvoorbeeld, als de naam van uw cluster `MyCluster`, de volgende geldige URI's zijn:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> De volgende URI's retourneren een foutmelding omdat het tweede exemplaar van de naam is niet hoofdlettergevoelig.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Authentication

Verbinding maken met Ambari op HDInsight HTTPS is vereist. De naam van de Administrator-account gebruiken (de standaardwaarde is **admin**) en het wachtwoord die u hebt opgegeven tijdens het maken van het cluster.

## <a name="examples-authentication-and-parsing-json"></a>Voorbeelden: Verificatie en bij het parseren van JSON

De volgende voorbeelden laten zien hoe u een GET-aanvraag met de base Ambari REST-API:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> De Bash-voorbeelden in dit document moeten de volgende veronderstellingen:
>
> * De aanmeldingsnaam voor het cluster is de standaardwaarde van `admin`.
> * `$PASSWORD`het wachtwoord voor de opdracht van de aanmelding HDInsight bevat. U kunt deze waarde instellen met behulp van `PASSWORD='mypassword'`.
> * `$CLUSTERNAME`bevat de naam van het cluster. U kunt deze waarde instellen door gebruik te maken`set CLUSTERNAME='clustername'`

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> De PowerShell-voorbeelden in dit document moeten de volgende veronderstellingen:
>
> * `$creds`is een referentieobject dat de beheerderaanmelding en het wachtwoord voor het cluster bevat. U kunt deze waarde instellen met behulp van `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` en de referenties opgeeft wanneer u wordt gevraagd.
> * `$clusterName`is een tekenreeks met de naam van het cluster. U kunt deze waarde instellen met behulp van `$clusterName="clustername"`.

Beide voorbeelden retourneren een JSON-document dat met de informatie is vergelijkbaar met het volgende voorbeeld begint:

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

### <a name="parsing-json-data"></a>Bij het parseren van JSON-gegevens

Het volgende voorbeeld wordt `jq` parseren van het JSON-antwoorddocument en alleen wordt weergegeven de `health_report` informatie uit de resultaten.

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 en hoger biedt de `ConvertFrom-Json` cmdlet, waarmee het JSON-document converteert naar een object dat is eenvoudiger om te werken in PowerShell. Het volgende voorbeeld wordt `ConvertFrom-Json` om weer te geven alleen de `health_report` informatie uit de resultaten.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Tijdens het meeste voorbeelden in dit document `ConvertFrom-Json` om elementen van het antwoorddocument te geven de [Update Ambari configuratie](#example-update-ambari-configuration) voorbeeld jq wordt gebruikt. Jq wordt in dit voorbeeld gebruikt om een nieuwe sjabloon uit de JSON-antwoorddocument samen te stellen.

Zie voor een volledig overzicht van de REST API [Ambari-API-verwijzing V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Voorbeeld: De FQDN van de clusterknooppunten ophalen

Als u werkt met HDInsight, moet u wellicht weten de volledig gekwalificeerde domeinnaam (FQDN) van een clusterknooppunt. De FQDN-naam voor de verschillende knooppunten in het cluster met behulp van de volgende voorbeelden kunt u eenvoudig ophalen:

* **Alle knooppunten**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **HEAD-knooppunten**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
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
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
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
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Voorbeeld: De interne IP-adres van de clusterknooppunten ophalen

> [!IMPORTANT]
> De IP-adressen die wordt geretourneerd door de voorbeelden in deze sectie zijn niet rechtstreeks toegankelijk via het internet. Ze zijn alleen toegankelijk vanuit het Azure-netwerk met het HDInsight-cluster.
>
> Zie voor meer informatie over het werken met HDInsight en virtuele netwerken [mogelijkheden uitbreiden HDInsight met behulp van een aangepaste Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

Als u het IP-adres zoekt, moet u weten de interne volledig gekwalificeerde domeinnaam (FQDN) van de clusterknooppunten. Zodra u de FQDN-naam hebt, krijgt u het IP-adres van de host. De volgende voorbeelden eerst Ambari zoeken naar de FQDN van alle knooppunten van de host vervolgens Ambari zoeken naar het IP-adres van elke host.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

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

## <a name="example-get-the-default-storage"></a>Voorbeeld: De opslag standaard ophalen

Wanneer u een HDInsight-cluster maakt, moet u een Azure Storage-Account of een Data Lake Store als de standaard-opslag gebruiken voor het cluster. Ambari kunt u deze informatie ophalen nadat het cluster is gemaakt. Bijvoorbeeld als u wilt lezen/schrijven gegevens naar de container buiten HDInsight.

De standaardconfiguratie voor opslag ophalen in de volgende voorbeelden uit het cluster:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Deze voorbeelden retourneert de eerste configuratie is toegepast op de server (`service_config_version=1`) die deze informatie bevat. Als u een waarde die is gewijzigd na het maken van het cluster ophaalt, moet u wellicht de configuratieversies lijst en de meest recente versie op te halen.

De geretourneerde waarde is vergelijkbaar met een van de volgende voorbeelden:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Deze waarde geeft aan dat het cluster een Azure Storage-account wordt gebruikt voor standaard-opslag. De `ACCOUNTNAME` waarde is de naam van het opslagaccount. De `CONTAINER` gedeelte is de naam van de blob-container in het opslagaccount. De container is de hoofdmap van de HDFS-compatibele opslagruimte voor het cluster.

* `adl://home`-Deze waarde geeft aan dat het cluster een Azure Data Lake Store wordt gebruikt voor standaard-opslag.

    De naam te zoeken Data Lake Store-account, gebruik de volgende voorbeelden:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    De geretourneerde waarde is vergelijkbaar met `ACCOUNTNAME.azuredatalakestore.net`, waarbij `ACCOUNTNAME` is de naam van het Data Lake Store-account.

    Als u wilt zoeken naar de map in Data Lake Store met de opslagruimte voor het cluster, gebruik de volgende voorbeelden:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    De geretourneerde waarde is vergelijkbaar met `/clusters/CLUSTERNAME/`. Deze waarde is een pad in het Data Lake Store-account. Dit pad is de hoofdmap van het systeem HDFS compatibel bestand voor het cluster. 

> [!NOTE]
> De `Get-AzureRmHDInsightCluster` cmdlet geleverd door [Azure PowerShell](/powershell/azure/overview) geeft ook de storage-gegevens voor het cluster.


## <a name="example-get-configuration"></a>Voorbeeld: Get-configuratie

1. Haal de configuraties die beschikbaar voor uw cluster zijn.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Het volgende voorbeeld wordt een JSON-document met de huidige configuratie (geïdentificeerd door de *tag* waarde) voor de onderdelen die zijn geïnstalleerd op het cluster. Het volgende voorbeeld is een fragment van de gegevens van een Spark-clustertype geretourneerd.
   
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

2. De configuratie voor het onderdeel dat u geïnteresseerd bent in ophalen. Vervang in het volgende voorbeeld wordt `INITIAL` geretourneerd met de waarde van het label van de vorige aanvraag.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Het volgende voorbeeld wordt een JSON-document met de huidige configuratie voor de `core-site` onderdeel.

## <a name="example-update-configuration"></a>Voorbeeld: Update-configuratie

1. De huidige configuratie Ambari worden opgeslagen als 'gewenste configuratie' ophalen:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Het volgende voorbeeld wordt een JSON-document met de huidige configuratie (geïdentificeerd door de *tag* waarde) voor de onderdelen die zijn geïnstalleerd op het cluster. Het volgende voorbeeld is een fragment van de gegevens van een Spark-clustertype geretourneerd.
   
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
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
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
    > Vervang **spark-thrift-sparkconf** en **INITIËLE** met het onderdeel en code die u wilt ophalen van de configuratie voor.
   
    Jq wordt gebruikt voor het inschakelen van de gegevens opgehaald uit HDInsight naar een nieuwe configuratiesjabloon. Deze voorbeelden wordt met name de volgende acties uitvoeren:
   
    * Hiermee maakt u een unieke waarde met de tekenreeks 'versie' en de datum die wordt opgeslagen in `newtag`.

    * Maakt een document hoofdmap voor de nieuwe gewenste configuratie.

    * Met deze eigenschap wordt de inhoud van de `.items[]` matrix en toegevoegd onder de **desired_config** element.

    * Hiermee verwijdert u de `href`, `version`, en `Config` elementen, als deze elementen niet nodig zijn voor het verzenden van een nieuwe configuratie.

    * Voegt een `tag` element met een waarde van `version#################`. Het numerieke gedeelte is gebaseerd op de huidige datum. Elke configuratie moet een unieke code hebben.
     
    Ten slotte de gegevens wordt opgeslagen in de `newconfig.json` document. De documentstructuur ziet er ongeveer als volgt uitzien:
     
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

3. Open de `newconfig.json` wijzigen/toevoegen en documenten waarden in de `properties` object. Het volgende voorbeeld wordt de waarde van `"spark.yarn.am.memory"` van `"1g"` naar `"3g"`. Voegt ook `"spark.kryoserializer.buffer.max"` met een waarde van `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Sla het bestand als u klaar bent wijzigingen aanbrengen.

4. Gebruik de volgende opdrachten in de bijgewerkte configuratie om Ambari te verzenden.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
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
   
    Deze opdrachten verzenden van de inhoud van de **newconfig.json** bestand aan het cluster als de nieuwe gewenste configuratie. De aanvraag retourneert een JSON-document. De **versionTag** element in dit document moet overeenkomen met de versie die u hebt ingediend, en de **configs** object bevat de configuratiewijzigingen die u hebt aangevraagd.

### <a name="example-restart-a-service-component"></a>Voorbeeld: Start opnieuw op een serviceonderdeel

Op dit punt als u de Ambari-webgebruikersinterface bekijkt, de Spark-service geeft aan dat deze opnieuw worden opgestart moet voordat de nieuwe configuratie van kracht. Gebruik de volgende stappen uit de service te starten.

1. Gebruik de volgende onderhoudsmodus voor de Spark-service inschakelen:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
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
   
    Deze opdrachten wordt een JSON-document verzenden naar de server die Hiermee schakelt u onderhoudsmodus. U kunt controleren of de service nu in de onderhoudsmodus met de volgende aanvraag:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    De geretourneerde waarde is `ON`.

2. Gebruik de volgende vervolgens de service uitschakelen:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
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
    > De `href` waarde die door deze URI geretourneerd met behulp van het interne IP-adres van het clusterknooppunt. Als u wilt gebruiken van buiten het cluster, kunt u het gedeelte '10.0.0.18:8080' vervangen door de FQDN-naam van het cluster. 
    
    De volgende opdrachten de status van de aanvraag niet ophalen:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Een antwoord van `COMPLETED` geeft aan dat de aanvraag is voltooid.

3. Zodra de vorige aanvraag is voltooid, moet u het volgende gebruiken om de service te starten.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
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

4. Gebruik tot slot de volgende onderhoudsmodus uit te schakelen.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
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

Zie voor een volledig overzicht van de REST API [Ambari-API-verwijzing V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

