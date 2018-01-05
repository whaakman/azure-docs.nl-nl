---
title: Hadoop-clusters in HDInsight met .NET SDK - Azure beheren | Microsoft Docs
description: Informatie over het uitvoeren van beheertaken voor het Hadoop-clusters in HDInsight met behulp van HDInsight .NET SDK.
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: fd134765-c2a0-488a-bca6-184d814d78e9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jgao
ms.openlocfilehash: d881d47e26460d3fff89c01245bba4c608dc8b08
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Hadoop-clusters in HDInsight beheren met behulp van de .NET SDK
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informatie over het beheren van HDInsight-clusters met [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).

**Vereisten**

Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-to-azure-hdinsight"></a>Verbinding maken met Azure HDInsight

U moet de volgende NuGet-pakketten:

```
Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
Install-Package Microsoft.Azure.Management.ResourceManager -Pre
Install-Package Microsoft.Azure.Management.HDInsight
```

Het volgende codevoorbeeld ziet u hoe verbinding maken met Azure voordat u HDInsight-clusters onder uw Azure-abonnement kunt beheren.

```csharp
using System;
using Microsoft.Azure;
using Microsoft.Azure.Management.HDInsight;
using Microsoft.Azure.Management.HDInsight.Models;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace HDInsightManagement
{
    class Program
    {
        private static HDInsightManagementClient _hdiManagementClient;
        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

        static void Main(string[] args)
        {
            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            // insert code here

            System.Console.WriteLine("Press ENTER to continue");
            System.Console.ReadLine();
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
    }
}
```

U wordt gevraagd wanneer dit programma wordt uitgevoerd.  Als u niet dat de volgende prompt verschijnt wilt, Zie [niet-interactieve verificatie .NET HDInsight-toepassingen maken](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Clusters maken
Zie [maken Linux gebaseerde clusters in HDInsight met behulp van de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Lijst met clusters
Het volgende codefragment bevat de clusters en sommige eigenschappen:

```csharp
var results = _hdiManagementClient.Clusters.List();
foreach (var name in results.Clusters) {
    Console.WriteLine("Cluster Name: " + name.Name);
    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
    Console.WriteLine("\t Cluster location: " + name.Location);
    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
}
```

## <a name="delete-clusters"></a>Clusters verwijderen
Gebruik het volgende codefragment om te verwijderen van een cluster synchroon of asynchroon: 

```csharp
_hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
_hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
```

## <a name="scale-clusters"></a>Clusters schalen
Het schalen van de functie cluster kunt u het aantal worker-knooppunten gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de pagina eigenschappen controleren.  Zie [lijst en geeft weer clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
> 
> 

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type van ondersteund door de HDInsight-cluster:

* Hadoop
  
    U kunt naadloos Verhoog het aantal worker-knooppunten van een Hadoop-cluster dat wordt uitgevoerd zonder enige impact op alle taken die in behandeling of wordt uitgevoerd. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor schaal worden probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.
  
    Wanneer een Hadoop-cluster wordt verkleind door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw gestart. Dit zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking uit te schalen. U kunt echter de taken verzenden zodra de bewerking voltooid is.
* HBase
  
    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking uit te schalen. U kunt echter ook handmatig de regionale servers verdelen door aan te melden bij de headnode van cluster en het gebruik van de volgende opdrachten vanuit een opdrachtpromptvenster:
  
    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```
* Storm
  
    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde voltooiing van de bewerking uit te schalen, moet u de topologie opnieuw verdelen.
  
    Herverdeling kan worden uitgevoerd op twee manieren:
  
  * Storm-webgebruikersinterface
  * Hulpprogramma voor opdrachtregelinterface (CLI)
    
    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.
    
    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:
    
    ![HDInsight Storm scale deel opnieuw](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Hier volgt een voorbeeld van hoe u met de opdracht CLI opnieuw verdelen van de Storm-topologie:
    
    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Het volgende codefragment wordt getoond hoe het formaat van een cluster synchroon of asynchroon:

```csharp
_hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
_hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
```

## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebt RESTful eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services verleend om toegang te krijgen. U kunt in te trekken/verlenen toegang. Voor het intrekken van:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = false,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

Om toegang te verlenen:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = enable,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

> [!NOTE]
> Door de toegang verlenen/intrekken, stelt u de cluster-gebruikersnaam en wachtwoord.
> 
> 

Dit kan ook worden gedaan via de Portal. Zie [HDInsight beheren met behulp van de Azure-portal][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-gebruikersreferenties updaten
Dit is dezelfde procedure als [Grant/revoke HTTP toegang](#grant/revoke-access). Als het cluster heeft de HTTP-toegang is verleend, moet u het eerst intrekken.  Vervolgens toegang verlenen en de met nieuwe HTTP gebruikersgegevens.

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
Het volgende codefragment laat zien hoe u de naam van het standaardopslagaccount en de standaard opslagaccountsleutel voor een cluster.

```csharp
var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
foreach (var key in results.Configuration.Keys)
{
    Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
}
```

## <a name="submit-jobs"></a>Verzenden van taken
**MapReduce-taken verzenden**

Zie [uitvoeren Hadoop-MapReduce-voorbeelden in HDInsight](hadoop/apache-hadoop-run-samples-linux.md).

**Hive-taken verzenden** 

Zie [uitvoeren Hive-query's met .NET SDK](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).

**Pig-taken verzenden**

Zie [uitvoeren Pig-taken met .NET SDK](hadoop/apache-hadoop-use-pig-dotnet-sdk.md).

**Om Sqoop taken te verzenden**

Zie [Sqoop gebruiken met HDInsight](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md).

**Oozie-taken verzenden**

Zie [Oozie gebruiken met Hadoop om te definiëren en uitvoeren van een werkstroom in HDInsight](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure Blob-opslag
Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Zie ook
* [HDInsight .NET SDK-documentatie](https://msdn.microsoft.com/library/mt271028.aspx)
* [HDInsight beheren met behulp van de Azure-portal][hdinsight-admin-portal]
* [HDInsight met behulp van een opdrachtregelinterface beheren][hdinsight-admin-cli]
* [HDInsight-clusters maken][hdinsight-provision]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Aan de slag met Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


