---
title: Migreren naar Azure Resource Manager-hulpprogramma's voor HDInsight | Microsoft Docs
description: Migreren naar Azure Resource Manager ontwikkelprogramma's voor HDInsight-clusters
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: e9fe49873a03867bdc55239eb4af623f0587f4f8
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migreren naar Azure Resource Manager gebaseerde ontwikkelprogramma's voor HDInsight-clusters

HDInsight is het van Azure Service Manager ASM-hulpprogramma's voor HDInsight. Als u hebt gebruikt Azure PowerShell, Azure CLI of de HDInsight .NET SDK werken met HDInsight-clusters, wordt u aangeraden de Azure Resource Manager ARM gebaseerde versies van PowerShell, CLI en .NET SDK gaan gebruiken. Dit artikel bevat verwijzingen over het migreren naar de nieuwe benadering van op basis van ARM. Waar van toepassing, wijst in dit artikel ook op de verschillen tussen de ARM en ASM benaderingen voor HDInsight.

> [!IMPORTANT]
> De ondersteuning voor ASM op basis van PowerShell, CLI en .NET SDK af te breken op **1 januari 2017**.
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a>Azure CLI migreren naar Azure Resource Manager
De Azure CLI nu standaard naar de modus Azure Resource Manager (ARM), tenzij u een upgrade van een vorige installatie uitvoert; u wilt in dit geval gebruiken de `azure config mode arm` opdracht overschakelen naar ARM-modus.

De basisopdrachten die de Azure CLI opgegeven om te werken met HDInsight met behulp van Azure Service Management (ASM) zijn hetzelfde als met ARM; Sommige parameters en schakelopties mogelijk wel nieuwe namen en er zijn veel nieuwe parameters beschikbaar wanneer u de ARM. Bijvoorbeeld, u kunt nu gebruiken `azure hdinsight cluster create` de Azure-netwerk dat moet worden gemaakt in een cluster of Hive en Oozie-metastore informatie te geven.

Basic-opdrachten voor het werken met HDInsight via Azure Resource Manager zijn:

* `azure hdinsight cluster create`-maakt een nieuwe HDInsight-cluster
* `azure hdinsight cluster delete`-een bestaand HDInsight-cluster worden verwijderd
* `azure hdinsight cluster show`-informatie weer over een bestaand cluster
* `azure hdinsight cluster list`-een lijst met HDInsight-clusters voor uw Azure-abonnement

Gebruik de `-h` overschakelen naar het inspecteren van de parameters en switches beschikbaar zijn voor elke opdracht.

### <a name="new-commands"></a>Nieuwe opdrachten
Nieuwe opdrachten die beschikbaar zijn met Azure Resource Manager zijn:

* `azure hdinsight cluster resize`-het aantal worker-knooppunten in het cluster dynamisch wordt gewijzigd
* `azure hdinsight cluster enable-http-access`-HTTPs toegang biedt tot het cluster (op standaard)
* `azure hdinsight cluster disable-http-access`-HTTPs-toegang tot het cluster wordt uitgeschakeld
* `azure hdinsight script-action`-bevat opdrachten voor het maken/beheren scriptacties op een cluster
* `azure hdinsight config`-bevat opdrachten voor het maken van een configuratiebestand dat kan worden gebruikt met de `hdinsight cluster create` opdracht om configuratie-informatie te geven.

### <a name="deprecated-commands"></a>Afgeschafte opdrachten
Als u de `azure hdinsight job` opdrachten voor het verzenden van taken naar uw HDInsight-cluster deze zijn niet beschikbaar is via de ARM-opdrachten. Als u programmatisch verzenden van taken naar HDInsight van scripts wilt, moet u in plaats daarvan de REST-API's geleverd door HDInsight. Zie de volgende documenten voor meer informatie over het verzenden van taken met REST API's.

* [MapReduce-taken uitvoeren met Hadoop in HDInsight met cURL](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [Hive-query's uitvoeren met Hadoop in HDInsight met cURL](hadoop/apache-hadoop-use-hive-curl.md)
* [Pig-taken uitvoeren met Hadoop in HDInsight met cURL](hadoop/apache-hadoop-use-pig-curl.md)

Voor informatie over andere manieren om uit te voeren MapReduce, Hive, en interactief varkens, Zie [MapReduce gebruiken met Hadoop op HDInsight](hadoop/hdinsight-use-mapreduce.md), [Hive gebruiken met Hadoop op HDInsight](hadoop/hdinsight-use-hive.md), en [Pig gebruiken met Hadoop op HDInsight](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Voorbeelden
**Maken van een cluster**

* Vorige opdracht (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Nieuwe opdracht (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Verwijderen van een cluster**

* Vorige opdracht (ASM)-`azure hdinsight cluster delete myhdicluster`
* Nieuwe opdracht (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Lijst met clusters**

* Vorige opdracht (ASM)-`azure hdinsight cluster list`
* Nieuwe opdracht (ARM)-`azure hdinsight cluster list`

> [!NOTE]
> Voor de opdracht lijst opgeven van de resource-groep met `-g` retourneert alleen de clusters in de opgegeven resourcegroep.
> 
> 

**Cluster-informatie weergeven**

* Vorige opdracht (ASM)-`azure hdinsight cluster show myhdicluster`
* Nieuwe opdracht (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure PowerShell migreren naar Azure Resource Manager
Algemene informatie over Azure PowerShell in de modus Azure Resource Manager (ARM) kan worden gevonden op [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

De ARM van Azure PowerShell-cmdlets kunnen worden geïnstalleerd side-by-side met de ASM-cmdlets. De cmdlets van de twee modi kunnen worden onderscheiden door hun namen.  De ARM-modus is *AzureRmHDInsight* in de namen van de cmdlets vergelijken met het *AzureHDInsight* in de ASM-modus.  Bijvoorbeeld: *nieuw AzureRmHDInsightCluster* vs. *Nieuwe AzureHDInsightCluster*. Parameters en switches wellicht nieuws namen en er zijn veel nieuwe parameters beschikbaar wanneer u ARM.  Meerdere cmdlets vereisen bijvoorbeeld dat een nieuwe switch aangeroepen *- ResourceGroupName*. 

Voordat u de HDInsight-cmdlets gebruiken kunt, moet u verbinding maken met uw Azure-account en een nieuwe resourcegroep maken:

* Login-AzureRmAccount of [Selecteer AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Zie [verifiëren van een service-principal met Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Nieuwe AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Nieuwe naam cmdlets
Overzicht van de HDInsight ASM-cmdlets in Windows PowerShell-console:

    help *azurermhdinsight*

De volgende tabel bevat de ASM-cmdlets en hun namen in de ARM-modus:

| ASM-cmdlets | ARM-cmdlets |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Voeg AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Voeg AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Voeg AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Voeg AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Verleen AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Verleen AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Aanroepen AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[Nieuwe AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[Nieuwe AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[Nieuwe AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[Nieuwe AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[Nieuwe AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[Nieuwe AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Nieuwe AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Verwijder AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[REVOKE AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[REVOKE AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Gebruik AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Wacht AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Nieuwe cmdLets
Hieronder vindt u de nieuwe cmdlets die alleen beschikbaar in de ARM-modus. 

**Scriptactie verwante cmdlets:**

* **Get-AzureRmHDInsightPersistedScriptAction**: opgehaald van de persistente scriptacties voor een cluster en worden ze weergegeven in chronologische volgorde of details voor een opgegeven persistente scriptacties actie opgehaald. 
* **Get-AzureRmHDInsightScriptActionHistory**: opgehaald van de geschiedenis van de scriptactie voor een cluster en details van een eerder uitgevoerde scriptactie opgehaald, wordt weergegeven in omgekeerde volgorde. 
* **Verwijder AzureRmHDInsightPersistedScriptAction**: Hiermee verwijdert u een actie persistent script van een HDInsight-cluster.
* **Set-AzureRmHDInsightPersistedScriptAction**: Hiermee stelt u een eerder uitgevoerde scriptactie moet een persistent script in te grijpen.
* **Indienen AzureRmHDInsightScriptAction**: een nieuwe scriptactie naar een Azure HDInsight-cluster worden verzonden. 

Zie voor meer informatie over het gebruiksinformatie [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter identiteit verwante cmdlets:**

* **Voeg AzureRmHDInsightClusterIdentity**: de identiteit van een cluster toegevoegd aan een configuratieobject cluster zodat het HDInsight-cluster, toegang Azure Data Lake Stores tot hebben. Zie [een HDInsight-cluster maken met Data Lake Store met Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Voorbeelden
**Cluster maken**

Oude opdracht (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Nieuwe opdracht (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Cluster verwijderen**

Oude opdracht (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Nieuwe opdracht (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Lijst met cluster**

Oude opdracht (ASM):

    Get-AzureHDInsightCluster

Nieuwe opdracht (ARM):

    Get-AzureRmHDInsightCluster 

**Cluster weergeven**

Oude opdracht (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Nieuwe opdracht (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Andere voorbeelden
* [HDInsight-clusters maken](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Indienen van Hive-taken](hadoop/apache-hadoop-use-hive-powershell.md)
* [Pig-taken verzenden](hadoop/apache-hadoop-use-pig-powershell.md)
* [Sqoop taken verzenden](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>Migreren naar de HDInsight op basis van ARM .NET SDK
De Azure Service Management-gebaseerde [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is nu verouderd. U wordt aangeraden gebruik van de Azure Resource Management-gebaseerde [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). De volgende HDInsight op basis van de ASM-pakketten worden afgeschaft.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Deze sectie bevat koppelingen naar meer informatie over het uitvoeren van bepaalde taken met de SDK op basis van ARM.

| Hoe... met behulp van de HDInsight op basis van ARM SDK | Koppelingen |
| --- | --- |
| Maken van HDInsight-clusters met .NET SDK |Zie [HDInsight-clusters maken met .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| Aanpassen van een cluster met de scriptactie met .NET SDK |Zie [aanpassen HDInsight Linux-clusters met behulp van de scriptactie](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Verifiëren van toepassingen die gebruikmaken van Azure Active Directory interactief met .NET SDK |Zie [uitvoeren Hive-query's met .NET SDK](hadoop/apache-hadoop-use-hive-dotnet-sdk.md). Het volgende codefragment in dit artikel maakt gebruik van de aanpak voor interactieve verificatie. |
| Verifiëren van toepassingen die gebruikmaken van Azure Active Directory niet-interactief met .NET SDK |Zie [niet-interactieve toepassingen voor HDInsight maken](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Verzenden van een Hive-taak met .NET SDK |Zie [indienen Hive-taken](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| Verzenden van een Pig-taak met .NET SDK |Zie [verzenden van Pig-taken](hadoop/apache-hadoop-use-pig-dotnet-sdk.md) |
| Verzenden van een Sqoop taak met .NET SDK |Zie [Sqoop verzenden van taken](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| Lijst met HDInsight-clusters met .NET SDK |Zie [lijst HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Schalen van HDInsight-clusters met .NET SDK |Zie [Scale HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| GRANT/revoke toegang tot HDInsight-clusters met .NET SDK |Zie [Grant/revoke toegang tot HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| HTTP-gebruikersreferenties updaten voor HDInsight-clusters met .NET SDK |Zie [Update HTTP gebruikersreferenties voor HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Het standaardopslagaccount vinden voor HDInsight-clusters met .NET SDK |Zie [vinden van het standaardopslagaccount voor HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| HDInsight-clusters met .NET SDK verwijderen |Zie [verwijderen HDInsight-clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Voorbeelden
Hieronder volgen enkele voorbeelden van hoe een bewerking is uitgevoerd met behulp van de SDK op basis van ASM en de equivalente codefragment voor de SDK op basis van ARM.

**Maken van een cluster CRUD-client**

* Vorige opdracht (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Nieuwe opdracht (ARM) (Service principal autorisatie)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Nieuwe opdracht (ARM) (gebruikersautorisatie)
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Maken van een cluster**

* Vorige opdracht (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Nieuwe opdracht (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**HTTP-toegang inschakelen**

* Vorige opdracht (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Nieuwe opdracht (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Verwijderen van een cluster**

* Vorige opdracht (ASM)
  
        client.DeleteCluster(dnsName);
* Nieuwe opdracht (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);

