---
title: Controleren en beheren van Hadoop met Ambari REST-API - Azure HDInsight
description: Leer hoe u Ambari gebruiken om te controleren en beheren van Hadoop-clusters in Azure HDInsight. In dit document leert u hoe u de Ambari REST-API met HDInsight-clusters worden opgenomen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: hrasheed
ms.openlocfilehash: 37d4c4dbbe623d68edf1de5454d7752a81fc2bd2
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58801876"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-rest-api"></a>HDInsight-clusters beheren met behulp van de Apache Ambari REST-API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Informatie over het gebruik van de Apache Ambari REST-API voor het beheren en controleren van Apache Hadoop-clusters in Azure HDInsight.

## <a id="whatis"></a>Wat is Apache Ambari
[Apache Ambari](https://ambari.apache.org) vereenvoudigt het beheer en bewaking van Hadoop-clusters met een eenvoudig te gebruiken webinterface ondersteund door de [REST-API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).  Ambari wordt geboden door standaard met HDInsight op basis van Linux-clusters.

## <a name="prerequisites"></a>Vereisten
* **Een Hadoop-cluster op HDInsight**. Zie [aan de slag met HDInsight op Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Bash in Ubuntu in Windows 10**.  De voorbeelden in dit artikel gebruikt u de Bash-shell op Windows 10. Zie [Windows-subsysteem voor Linux-installatie handleiding voor Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) voor de installatiestappen.  Andere [Unix shells](https://www.gnu.org/software/bash/) ook werkt.  De voorbeelden, met enkele kleine wijzigingen kunnen worden gebruikt voor een Windows-opdrachtprompt.  U kunt ook Windows PowerShell gebruiken.

* **jq**, een opdrachtregelprogramma JSON-processor.  Zie [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

* **Windows PowerShell**.  U kunt ook gebruiken [Bash](https://www.gnu.org/software/bash/).

## <a name="base-uri-for-ambari-rest-api"></a>Basis-URI voor de Ambari Rest-API

 De basis-URI voor de Ambari REST-API op HDInsight is `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`, waarbij `CLUSTERNAME` is de naam van uw cluster.  Clusternamen in de URI's zijn **hoofdlettergevoelig**.  De naam van het cluster in het gedeelte van de FQDN-naam (Fully Qualified Domain Name) van de URI (CLUSTERNAME.azurehdinsight.net) is niet hoofdlettergevoelig, zijn andere exemplaren in de URI zijn hoofdlettergevoelig.

## <a name="authentication"></a>Authentication

Verbinding maken met Ambari op HDInsight vereist HTTPS. De accountnaam van beheerder gebruiken (de standaardwaarde is **admin**) en het wachtwoord die u hebt opgegeven tijdens het maken van clusters.

## <a name="examples"></a>Voorbeelden

### <a name="setup-preserve-credentials"></a>Setup (behouden referenties)
Uw referenties in om te voorkomen dat deze opnieuw invoeren voor elke voorbeeld behouden.  Naam van het cluster worden bewaard in een afzonderlijke stap.

**EEN. Bash**  
Het onderstaande script bewerken door te vervangen `PASSWORD` met uw huidige wachtwoord.  Voer de opdracht.

```bash
export password='PASSWORD'
```  

**B. PowerShell**  

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>De naam correct omhuld cluster identificeren
De werkelijke behuizing van de naam van het cluster is mogelijk anders dan u verwacht, afhankelijk van hoe het cluster is gemaakt.  De stappen die hier wordt het werkelijke hoofdlettergebruik weergeven en vervolgens opslaan in een variabele voor alle opeenvolgende voorbeelden.

De scripts hieronder om te vervangen bewerken `CLUSTERNAME` met de clusternaam van uw. Voer de opdracht. (De naam van het cluster voor de FQDN-naam is niet hoofdlettergevoelig).

**EEN. Bash**  

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

### <a name="parsing-json-data"></a>Het parseren van JSON-gegevens

Het volgende voorbeeld wordt [jq](https://stedolan.github.io/jq/) of [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json) voor de JSON-antwoorddocument parseren en weergeven van alleen de `health_report` gegevens uit de resultaten.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" \
| jq '.Clusters.health_report'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```   

### <a name="example-get-the-fqdn-of-cluster-nodes"></a> De FQDN-naam van de clusterknooppunten ophalen

Als u werkt met HDInsight, moet u mogelijk kent de volledig gekwalificeerde domeinnaam (FQDN) van een clusterknooppunt. U kunt eenvoudig de FQDN-naam voor de verschillende knooppunten in het cluster met behulp van de volgende voorbeelden ophalen:

**Alle knooppunten**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq '.items[].Hosts.host_name'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```

**Hoofdknooppunten**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Worker-knooppunten**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Zookeeper-knooppunten**

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
| jq ".host_components[].HostRoles.host_name"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

### <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a> Het interne IP-adres van de clusterknooppunten ophalen

De IP-adressen die wordt geretourneerd door de voorbeelden in deze sectie zijn niet rechtstreeks toegankelijk via internet. Ze zijn alleen toegankelijk binnen het virtuele Azure-netwerk waarin het HDInsight-cluster.

Zie voor meer informatie over het werken met HDInsight en virtuele netwerken [mogelijkheden voor HDInsight uitbreiden met behulp van een aangepaste Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

Als u het IP-adres zoekt, moet u de interne volledig gekwalificeerde domeinnaam (FQDN) van de clusterknooppunten weten. Zodra u de FQDN-naam hebt, krijgt u vervolgens het IP-adres van de host. De volgende voorbeelden eerst Ambari op te vragen voor de FQDN-naam van alle knooppunten van de host en vervolgens Ambari op te vragen voor het IP-adres van elke host.

```bash
for HOSTNAME in $(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```  

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" 
$resp = Invoke-WebRequest -Uri $uri -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds -UseBasicParsing
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

### <a name="get-the-default-storage"></a>Profiteer van de standaardopslag

Wanneer u een HDInsight-cluster maakt, moet u een Azure Storage-Account of een Data Lake-opslag gebruiken als de standaardopslag voor het cluster. Ambari kunt u deze informatie ophalen nadat het cluster is gemaakt. Bijvoorbeeld, als u wilt lezen/schrijven van gegevens naar de container buiten HDInsight.

De standaardconfiguratie voor opslag ophalen in de volgende voorbeelden uit het cluster:

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]  
> Deze voorbeelden retourneren de eerste configuratie toegepast op de server (`service_config_version=1`) die deze informatie bevat. Als u een waarde die is gewijzigd na het maken van een cluster kunt ophalen, moet u de configuratieversies weergeven en ophalen van de meest recente versie.

De geretourneerde waarde is vergelijkbaar met een van de volgende voorbeelden:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` -Deze waarde geeft aan dat het cluster een Azure Storage-account wordt gebruikt voor standaardopslag. De `ACCOUNTNAME` waarde is de naam van het opslagaccount. De `CONTAINER` gedeelte is de naam van de blob-container in het opslagaccount. De container is de hoofdmap van het HDFS-compatibele opslag voor het cluster.

* `abfs://CONTAINER@ACCOUNTNAME.dfs.core.windows.net` -Deze waarde geeft aan dat het cluster een Azure Data Lake Storage Gen2 wordt gebruikt voor standaardopslag. De `ACCOUNTNAME` en `CONTAINER` waarden hebben de dezelfde betekenis als voor Azure Storage die eerder is vermeld.

* `adl://home` -Deze waarde geeft aan dat het cluster een Azure Data Lake Storage Gen1 wordt gebruikt voor standaardopslag.

    Als u de naam van het Data Lake Storage zoekt, kunt u de volgende voorbeelden gebruiken:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    De geretourneerde waarde is vergelijkbaar met `ACCOUNTNAME.azuredatalakestore.net`, waarbij `ACCOUNTNAME` is de naam van de Data Lake Storage-account.

    Als u wilt zoeken in de map in Data Lake-opslag met de opslag voor het cluster, moet u de volgende voorbeelden gebruiken:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```  

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    De geretourneerde waarde is vergelijkbaar met `/clusters/CLUSTERNAME/`. Deze waarde is een pad in het Data Lake Storage-account. Dit pad is de hoofdmap van de compatibele HDFS-bestandssysteem voor het cluster.  

> [!NOTE]  
> De [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) cmdlet geleverd door [Azure PowerShell](/powershell/azure/overview) geeft ook de opslaginformatie voor het cluster.


### <a name="get-all-configurations"></a> Alle configuraties ophalen

Haal de configuraties die beschikbaar voor uw cluster zijn.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName?fields=Clusters/desired_configs"
```

```powershell
$respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
    -Credential $creds -UseBasicParsing
$respObj.Content
```

In dit voorbeeld retourneert een JSON-document met de huidige configuratie (aangeduid met de *tag* waarde) voor de onderdelen worden geïnstalleerd op het cluster. Het volgende voorbeeld wordt een fragment uit de gegevens die worden geretourneerd van een Spark-cluster.
   
```json
"jupyter-site" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-client-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
```

### <a name="get-configuration-for-specific-component"></a>Configuratie voor een specifiek onderdeel ophalen

Opvragen van de configuratie voor het onderdeel dat u geïnteresseerd bent in. Vervang in het volgende voorbeeld wordt `INITIAL` geretourneerd met de waarde van het label van de vorige aanvraag.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
    -Credential $creds -UseBasicParsing
$resp.Content
```

In dit voorbeeld retourneert een JSON-document met de huidige configuratie voor de `livy2-conf` onderdeel.

### <a name="update-configuration"></a>Configuratie bijwerken

1. Maak `newconfig.json`.  
   Wijzig en voer de onderstaande opdrachten:

   * Vervang `livy2-conf` met de gewenste component.
   * Vervang `INITIAL` met de werkelijke waarde opgehaald voor `tag` van [ophalen van alle configuraties](#get-all-configurations).

     **EEN. Bash**  
     ```bash
     curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" \
     | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json

     ```

     **B. powershell**  
     Maakt gebruik van het PowerShell-script [jq](https://stedolan.github.io/jq/).  Bewerken `C:\HD\jq\jq-win64` in overeenstemming met uw werkelijke pad en de versie van de onderstaande [jq](https://stedolan.github.io/jq/).

     ```powershell
     $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
     $now = Get-Date
     $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
     $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
       -Credential $creds -UseBasicParsing
     $resp.Content | C:\HD\jq\jq-win64 --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
     ```

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
           "tag": "version1552064778014",
           "type": "livy2-conf",
           "properties": {
             "livy.environment": "production",
             "livy.impersonation.enabled": "true",
             "livy.repl.enableHiveContext": "true",
             "livy.server.csrf_protection.enabled": "true",
               ....
           },
         },
       }
     }
     ```

2. Bewerken `newconfig.json`.  
   Open de `newconfig.json` waarden document en wijzigen/toevoegen in de `properties` object. Het volgende voorbeeld wordt de waarde van `"livy.server.csrf_protection.enabled"` van `"true"` naar `"false"`.

        "livy.server.csrf_protection.enabled": "false",

    Sla het bestand wanneer u klaar bent en wijzigingen.

3. Indienen `newconfig.json`.  
   Gebruik de volgende opdrachten om in te dienen de bijgewerkte configuratie van Ambari.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```  

    Deze opdrachten verzenden de inhoud van de **newconfig.json** bestand aan het cluster als de nieuwe gewenste configuratie. De aanvraag retourneert een JSON-document. De **versionTag** -element in dit document moet overeenkomen met de versie die u hebt ingediend, en de **peeringconfiguraties** -object bevat de wijzigingen van de configuratie die u hebt aangevraagd.

### <a name="restart-a-service-component"></a>Een serviceonderdeel opnieuw starten

Op dit moment als u de Ambari-Webgebruikersinterface bekijkt, het Spark-service geeft aan dat deze opnieuw worden opgestart moet voordat de nieuwe configuratie van kracht. Gebruik de volgende stappen uit de service opnieuw te starten.

1. Schakel de onderhoudsmodus voor de service Spark2 gebruikt u de volgende:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```

2. Controleer of in de onderhoudsmodus  

    Deze opdrachten wordt een JSON-document verzenden naar de server die ingeschakeld in de onderhoudsmodus wordt. U kunt controleren of de service is nu in de onderhoudsmodus met behulp van de volgende aanvraag:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```

    De geretourneerde waarde `ON`.

3. Vervolgens gebruikt u de volgende de Spark2-service uitschakelen:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://girouxSpark.azurehdinsight.net/api/v1/clusters/girouxspark/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
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

4. Controleer of de aanvraag.  
    De onderstaande opdracht bewerken door te vervangen `29` met de daadwerkelijke waarde voor `id` geretourneerd uit de vorige stap.  De volgende opdrachten ophalen de status van de aanvraag:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Een reactie van `COMPLETED` geeft aan dat de aanvraag is voltooid.

5. Nadat de vorige aanvraag is voltooid, moet u het volgende gebruiken om de Spark2-service te starten.
   
    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```

    De service maakt nu gebruik van de nieuwe configuratie.

6. Gebruik tot slot de volgende uitschakelen in de onderhoudsmodus.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'

    ```

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledig overzicht van de REST-API, [Apache Ambari-API-verwijzing V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

