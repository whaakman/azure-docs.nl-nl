---
title: Met behulp van scriptacties - Azure HDInsight-Clusters aanpassen | Microsoft Docs
description: Informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: ec95b6d66c71b4278dd1e16807fcc75f5e8b1c36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>HDInsight op basis van Windows-clusters met behulp van de scriptactie aanpassen
**Actie script** kan worden gebruikt om aan te roepen [aangepaste scripts](hdinsight-hadoop-script-actions.md) tijdens het maken van het cluster voor het installeren van extra software op een cluster.

De informatie in dit artikel is specifiek voor HDInsight op basis van Windows-clusters. Zie voor Linux gebaseerde clusters [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

HDInsight-clusters kunnen worden aangepast op tal van andere manieren zoals inclusief extra Azure Storage-accounts, het wijzigen van de Hadoop configuratiebestanden (core site.xml, hive-site.xml, enz.) of algemene locaties in het cluster toe te voegen gedeeld bibliotheken (bijvoorbeeld Hive, Oozie). Deze aanpassingen kunnen worden uitgevoerd via Azure PowerShell, de Azure HDInsight .NET SDK of de Azure-portal. Zie voor meer informatie [maken Hadoop-clusters in HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Scriptactie in het proces voor het cluster maken
Scriptactie wordt alleen gebruikt als een cluster wordt momenteel wordt gemaakt. Het volgende diagram illustreert wanneer het Script wordt uitgevoerd tijdens het maken:

![Aanpassing van HDInsight-cluster en fasen tijdens het maken van het cluster][img-hdi-cluster-states]

Wanneer het script wordt uitgevoerd, wordt het cluster voert de **ClusterCustomization** fase. In deze fase wordt het script wordt uitgevoerd met het systeemaccount admin, parallel op de opgegeven knooppunten in het cluster, en biedt volledige beheerdersbevoegdheden op de knooppunten.

> [!NOTE]
> Hebt u beheerdersbevoegdheden op de clusterknooppunten tijdens de **ClusterCustomization** fase, kunt u het script bewerkingen zoals het stoppen en starten van services, met inbegrip van Hadoop-gerelateerde services uit te voeren. Dus als onderdeel van het script, moet u ervoor zorgen dat de Ambari-services en andere Hadoop-gerelateerde services actief zijn voordat het script is voltooid. Deze services zijn vereist om na te gaan met succes de gezondheid en status van het cluster terwijl deze wordt gemaakt. Als u een willekeurige configuratie op het cluster dat betrekking heeft op deze services wijzigt, moet u de Help-functies die beschikbaar zijn. Zie voor meer informatie over hulpfuncties [scriptactie ontwikkelen scripts voor HDInsight][hdinsight-write-script].
>
>

De uitvoer en de foutenlogboeken voor het script worden opgeslagen in het standaardopslagaccount dat u hebt opgegeven voor het cluster. De logboeken worden opgeslagen in een tabel met de naam van de **u < \cluster-name-fragment >< \time-stamp > bestand**. Dit zijn de cumulatieve logboeken van het script uitgevoerd op alle knooppunten (hoofdknooppunt en worker-knooppunten) in het cluster.

Elk cluster kan accepteren meerdere scriptacties die worden aangeroepen in de volgorde waarin ze worden opgegeven. Een script kan worden uitgevoerd op het hoofdknooppunt en/of de worker-knooppunten.

HDInsight biedt verschillende scripts voor het installeren van de volgende onderdelen op HDInsight-clusters:

| Naam | Script |
| --- | --- |
| **Spark installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeert en gebruikt Spark in HDInsight-clusters][hdinsight-install-spark]. |
| **R installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeert en gebruikt R op HDInsight-clusters][hdinsight-install-r]. |
| **Solr installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeert en gebruikt Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md). |
| - **Giraph installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeert en gebruikt Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md). |
| **Vooraf laden Hive-bibliotheken** |https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Zie [toevoegen Hive-bibliotheken op HDInsight-clusters](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Aanroep scripts met de Azure portal
**Vanuit de Azure-portal**

1. Beginnen met het maken van een cluster, zoals beschreven op [maken Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. Onder optionele configuratie voor de **scriptacties** blade, klikt u op **scriptactie toevoegen** om details te verstrekken over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de URI moet het script dat wordt opgeroepen voor het aanpassen van het cluster. s</td></tr>
        <tr><td>HEAD/Worker</td>
            <td>Geef de knooppunten (**Head** of **Worker**) op waarmee het script aanpassing wordt uitgevoerd.</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script.</td></tr>
    </table>

    Druk op ENTER om toe te voegen van meer dan één scriptactie meerdere om onderdelen te installeren op het cluster.
3. Klik op **Selecteer** Sla de configuratie van de actie script en doorgaan met het maken van het cluster.

## <a name="call-scripts-using-azure-powershell"></a>Aanroep scripts met Azure PowerShell
Deze volgende PowerShell-script laat zien hoe Spark installeren op Windows gebaseerde HDInsight-cluster.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Andere software wilt installeren, moet u het scriptbestand in het script vervangen:

Wanneer u wordt gevraagd, typt u de referenties voor het cluster. Het kan enkele minuten duren voordat het cluster is gemaakt.

## <a name="call-scripts-using-net-sdk"></a>Aanroepen van scripts die met .NET SDK
Het volgende voorbeeld laat zien hoe Spark installeren op Windows gebaseerde HDInsight-cluster. Andere software wilt installeren, moet u het scriptbestand in de code te vervangen.

**Maken van een HDInsight-cluster met Spark**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanaf de Nuget Package Manager-Console.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. Gebruik de volgende using-instructies in het bestand Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Plaats de code in de klasse met de volgende opties:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
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
5. Druk op **F5** om de toepassing uit te voeren.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open-source software gebruikt op HDInsight-clusters
De Microsoft Azure HDInsight-service is een flexibel platform waarmee u big data-toepassingen in de cloud maken met behulp van een ecosysteem van open-source technologieën gevormd rond Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source technologieën, zoals beschreven in de **ondersteunen bereik** sectie van de <a href="http://azure.microsoft.com/support/faq/" target="_blank">ondersteunen Veelgestelde vragen over Azure-website</a>. De HDInsight-service biedt een extra verificatieniveau van ondersteuning voor enkele van de onderdelen, zoals hieronder wordt beschreven.

Er zijn twee soorten open source-onderdelen die beschikbaar in de HDInsight-service zijn:

* **Ingebouwde onderdelen** -deze onderdelen vooraf zijn geïnstalleerd op HDInsight-clusters en geef de kernfunctionaliteit van het cluster. Bijvoorbeeld: YARN ResourceManager, de Hive-query language (HiveQL) en de bibliotheek Mahout behoren tot deze categorie. Een volledige lijst met clusteronderdelen is beschikbaar in [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight?](hdinsight-component-versioning.md) </a>.
* **Aangepaste onderdelen** -u, als een gebruiker van het cluster kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte.

Ingebouwde-onderdelen worden volledig ondersteund en Microsoft Support kunt opsporen en oplossen van problemen met betrekking tot deze onderdelen.

> [!WARNING]
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunt opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Dit kan leiden tot het oplossen van het probleem of vraag of u benaderen beschikbare kanalen voor de open-source technologieën waar grondige kennis van deze technologie kan worden gevonden. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

De HDInsight-service biedt verschillende manieren om te gebruiken van aangepaste onderdelen. Ongeacht hoe een onderdeel gebruikt of geïnstalleerd op het cluster, geldt hetzelfde niveau van ondersteuning. Hieronder volgt een lijst van de meest voorkomende manieren dat aangepaste onderdelen op HDInsight-clusters kunnen worden gebruikt:

1. Verzending van taak - Hadoop- of andere typen taken die worden uitgevoerd of het gebruik van aangepaste onderdelen kan worden verzonden naar het cluster.
2. Aanpassing van de cluster - tijdens het maken van het cluster, kunt u aanvullende instellingen en aangepaste onderdelen die worden geïnstalleerd op de clusterknooppunten opgeven.
3. Steekproeven - voor populaire aangepaste onderdelen, Microsoft en anderen kunnen voorbeelden van hoe deze onderdelen kunnen worden gebruikt op de HDInsight-clusters bieden. Deze voorbeelden worden geleverd zonder ondersteuning.

## <a name="develop-script-action-scripts"></a>Scriptactie-scripts ontwikkelen
Zie [scriptactie ontwikkelen scripts voor HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Zie ook
* [Hadoop-clusters maken in HDInsight] [ hdinsight-provision-cluster] vindt u instructies voor het maken van een HDInsight-cluster met behulp van andere aangepaste opties.
* [Scriptactie-scripts ontwikkelen voor HDInsight][hdinsight-write-script]
* [Installeren en gebruiken van Spark in HDInsight-clusters][hdinsight-install-spark]
* [Installeren en gebruiken van R op HDInsight-clusters][hdinsight-install-r]
* [Installeren en gebruiken van Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
* [Installeren en gebruiken van Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fasen tijdens het maken van het cluster"
