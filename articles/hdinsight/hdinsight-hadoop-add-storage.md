---
title: Aanvullende Azure storage-accounts toevoegen aan HDInsight
description: Informatie over het toevoegen van aanvullende Azure storage-accounts aan een bestaand HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: hrasheed
ms.openlocfilehash: 373851c406d95a2e458c017cb311bd5cc4e5b30f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664287"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>Extra opslagaccounts toevoegen aan HDInsight

Meer informatie over het gebruik scriptacties om toe te voegen extra Azure-opslag *accounts* naar HDInsight. De stappen in dit document een opslag toevoegen *account* aan een bestaand HDInsight op basis van Linux-cluster. In dit artikel is van toepassing op opslag *accounts* (niet het cluster standaardopslagaccount), en geen extra opslag zoals [Azure Data Lake Storage Gen1](hdinsight-hadoop-use-data-lake-store.md) en [Azure Data Lake Storage Gen2 ](hdinsight-hadoop-use-data-lake-storage-gen2.md).

> [!IMPORTANT]  
> De informatie in dit document is over het toevoegen van extra opslag aan een cluster nadat deze is gemaakt. Zie voor meer informatie over het toevoegen van storage-accounts tijdens het maken van een cluster [clusters instellen in HDInsight met Apache Hadoop, Apache Spark en Apache Kafka](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="prerequisites"></a>Vereisten

* Een Hadoop-cluster op HDInsight. Zie [aan de slag met HDInsight op Linux](./hadoop/apache-hadoop-linux-tutorial-get-started.md).
* Storage-accountnaam en de sleutel. Zie [opslagaccountinstellingen in Azure portal beheren](../storage/common/storage-account-manage.md).
* [De naam correct omhuld cluster](hdinsight-hadoop-manage-ambari-rest-api.md#identify-correctly-cased-cluster-name).
* Als u PowerShell gebruikt, moet u de AZ-module.  Zie [overzicht van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* Als u de Azure CLI nog niet hebt geïnstalleerd, Zie [Azure-opdrachtregelinterface (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).
* Als u bash of een windows-opdrachtprompt, u moet ook **jq**, een opdrachtregelprogramma JSON-processor.  Zie [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/). Zie voor bash in Ubuntu in Windows 10 [Windows-subsysteem voor Linux-installatie handleiding voor Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="how-it-works"></a>Hoe werkt het?

Met dit script worden de volgende parameters:

* __Azure storage-accountnaam__: De naam van het opslagaccount om toe te voegen aan het HDInsight-cluster. Nadat het script is uitgevoerd, kan HDInsight lezen en schrijven van gegevens die zijn opgeslagen in dit storage-account.

* __Azure storage-accountsleutel__: Een sleutel die toegang tot het opslagaccount verleent.

* __-p__ (optioneel): Als u opgeeft, wordt de sleutel is niet versleuteld en wordt opgeslagen in het bestand core-site.xml als tekst zonder opmaak.

Tijdens de verwerking voert het script de volgende acties:

* Als het opslagaccount al in de core-site.xml-configuratie voor het cluster bestaat, het script wordt afgesloten en er is geen verdere acties worden uitgevoerd.

* Hiermee wordt gecontroleerd of het opslagaccount bestaat en toegankelijk via de sleutel.

* De sleutel met behulp van de cluster-referentie gecodeerd.

* Het opslagaccount dat aan het bestand core-site.xml toegevoegd.

* Stopt en start u de [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), en [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) services. Deze services starten en stoppen, kunnen ze het nieuwe opslagaccount gebruiken.

> [!WARNING]  
> Met behulp van een storage-account in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="the-script"></a>Het script

__Scriptlocatie__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Vereisten voor__:  Het script moet worden toegepast op de __hoofdknooppunten__. U hoeft niet te markeren met dit script als __persistente__, zoals de Ambari-configuratie van het cluster rechtstreeks worden bijgewerkt.

## <a name="to-use-the-script"></a>Het gebruik van het script

Met dit script kan worden gebruikt vanuit de Azure PowerShell, Azure CLI of Azure portal.

### <a name="powershell"></a>PowerShell

Met behulp van [indienen AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/submit-azhdinsightscriptaction). Vervang `CLUSTERNAME`, `ACCOUNTNAME`, en `ACCOUNTKEY` met de juiste waarden.

```powershell
# Update these parameters
$clusterName = "CLUSTERNAME"
$parameters = "ACCOUNTNAME ACCOUNTKEY"

$scriptActionName = "addStorage"
$scriptActionUri = "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh"

# Execute script
Submit-AzHDInsightScriptAction `
    -ClusterName $clusterName `
    -Name $scriptActionName `
    -Uri $scriptActionUri `
    -NodeTypes "headnode" `
    -Parameters $parameters
```

### <a name="azure-cli"></a>Azure-CLI

Met behulp van [az hdinsight-scriptactie uitvoeren](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-execute).  Vervang `CLUSTERNAME`, `RESOURCEGROUP`, `ACCOUNTNAME`, en `ACCOUNTKEY` met de juiste waarden.

```cli
az hdinsight script-action execute ^
    --name CLUSTERNAME ^
    --resource-group RESOURCEGROUP ^
    --roles headnode ^
    --script-action-name addStorage ^
    --script-uri "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh" ^
    --script-parameters "ACCOUNTNAME ACCOUNTKEY"
```

### <a name="azure-portal"></a>Azure Portal

Zie [een scriptactie van toepassing op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster).

## <a name="known-issues"></a>Bekende problemen

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Storage-accounts niet weergegeven in Azure portal of hulpprogramma 's

Wanneer u het HDInsight-cluster in Azure portal bekijkt, selecteert de __Opslagaccounts__ item onder __eigenschappen__ storage-accounts die zijn toegevoegd via deze scriptactie niet wordt weergegeven. Azure PowerShell en Azure CLI weergeven niet het extra opslagaccount de.

De opslaginformatie wordt niet weergegeven omdat het script alleen het core-site.xml-configuratie van het cluster wijzigt. Deze informatie wordt niet gebruikt bij het ophalen van de clustergegevens met behulp van Azure management API's.

Als u wilt weergeven van gegevens over het opslagaccount toegevoegd aan het cluster met behulp van dit script, gebruikt u de Ambari REST-API. Gebruik de volgende opdrachten om op te halen van deze informatie voor uw cluster:

### <a name="powershell"></a>PowerShell

Vervang `CLUSTERNAME` met de naam van het cluster correct omhuld. Eerst bepalen de config-versie van de service wordt gebruikt met de onderstaande opdracht:

```powershell
# getting service_config_version
$clusterName = "CLUSTERNAME"

$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_service_config_versions/HDFS" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.desired_service_config_versions.HDFS.service_config_version
```

Vervang `ACCOUNTNAME` met de werkelijke namen. Vervang vervolgens `4` met de werkelijke service config-versie en voer de opdracht. Wanneer u wordt gevraagd, typt u het wachtwoord voor clusteraanmelding.

```powershell
# Update values
$accountName = "ACCOUNTNAME"
$version = 4

$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=$version" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$accountName.blob.core.windows.net"
```

### <a name="bash"></a>Bash
Vervang `myCluster` met de naam van het cluster correct omhuld.

```bash
export CLUSTERNAME='myCluster'

curl --silent -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_service_config_versions/HDFS" \
| jq ".Clusters.desired_service_config_versions.HDFS[].service_config_version" 
```

Vervang `myAccount` met de naam van het werkelijke opslag. Vervang vervolgens `4` met de werkelijke service config-versie en voer de opdracht:

```bash
export ACCOUNTNAME='"fs.azure.account.key.myAccount.blob.core.windows.net"'
export VERSION='4'

curl --silent -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=$VERSION" \
| jq ".items[].configurations[].properties[$ACCOUNTNAME] | select(. != null)"
```

### <a name="cmd"></a>cmd

Vervang `CLUSTERNAME` met de juiste wijze omhuld clusternaam in beide scripts. Eerst bepalen de config-versie van de service wordt gebruikt met de onderstaande opdracht:

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_service_config_versions/HDFS" | ^
jq-win64 ".Clusters.desired_service_config_versions.HDFS[].service_config_version" 
```

Vervang `ACCOUNTNAME` met de naam van het werkelijke opslag. Vervang vervolgens `4` met de werkelijke service config-versie en voer de opdracht:

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=4" | ^
jq-win64 ".items[].configurations[].properties["""fs.azure.account.key.ACCOUNTNAME.blob.core.windows.net"""] | select(. != null)"
```
---

 Gegevens die zijn geretourneerd door deze opdracht wordt weergegeven die vergelijkbaar is met de volgende tekst:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Deze tekst is een voorbeeld van een versleutelde sleutel, die wordt gebruikt voor toegang tot het opslagaccount.

### <a name="unable-to-access-storage-after-changing-key"></a>Geen toegang krijgt tot opslag na het wijzigen van sleutel

Als u de sleutel voor een storage-account wijzigt, kan HDInsight niet langer toegang tot het opslagaccount. HDInsight maakt gebruik van een in cache opgeslagen kopie van de sleutel in de core-site.xml voor het cluster. De kopie van deze cache moet worden bijgewerkt zodat deze overeenkomt met de nieuwe sleutel.

Uitvoeren van de scriptactie opnieuw heeft __niet__ bijwerken van de sleutel, zoals het script wordt gecontroleerd of er al een vermelding voor het opslagaccount bestaat. Als er al een vermelding bestaat, wordt er geen wijzigingen aanbrengen.

U kunt dit probleem omzeilen, moet u de bestaande vermelding voor de storage-account verwijderen. Gebruik de volgende stappen uit om te verwijderen van de bestaande vermelding:

1. Open de Ambari-Webinterface voor uw HDInsight-cluster in een webbrowser. De URI is `https://CLUSTERNAME.azurehdinsight.net`. Vervang `CLUSTERNAME` door de naam van uw cluster.

    Wanneer u hierom wordt gevraagd, typt u de HTTP-aangemelde gebruiker en het wachtwoord voor uw cluster.

2. Selecteer in de lijst met services aan de linkerkant van de pagina __HDFS__. Selecteer vervolgens de __Peeringconfiguraties__ tabblad in het midden van de pagina.

3. In de __filteren...__  veld, voert u een waarde van __fs.azure.account__. Hiermee wordt de vermeldingen voor alle extra opslagaccounts die zijn toegevoegd aan het cluster. Er zijn twee soorten vermeldingen; __keyprovider__ en __sleutel__. Beide bevatten de naam van het opslagaccount dat als onderdeel van de naam van de sleutel.

    Hieronder vindt u voorbeelden van vermeldingen voor een opslagaccount met de naam __mijnopslag__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Nadat u hebt vastgesteld dat de sleutels voor het opslagaccount dat u wilt verwijderen, gebruikt u de rode '-' pictogram aan de rechterkant van het item te verwijderen. Gebruik vervolgens de __opslaan__ knop uw wijzigingen op te slaan.

5. Nadat wijzigingen zijn opgeslagen, moet u de scriptactie gebruiken om toe te voegen van de storage-account en de nieuwe waarde van de sleutel aan het cluster.

### <a name="poor-performance"></a>Slechte prestaties

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, kan slechte prestaties optreden. Toegang tot gegevens in een andere regio stuurt netwerkverkeer buiten de regionale Azure-datacenter en via het openbare internet, dit leiden latentie tot kan.

### <a name="additional-charges"></a>Extra kosten

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, merkt u wellicht aanvullende uitgaand verkeer kosten in rekening gebracht op uw Azure-facturering. Een kosten voor uitgaand verkeer toegepast wanneer gegevens een regionaal datacenter verlaat. Deze kosten wordt toegepast, zelfs als het verkeer dat bestemd voor een andere Azure-datacenter in een andere regio is.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u extra opslagaccounts toevoegen aan een bestaand HDInsight-cluster. Zie voor meer informatie over scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md)
