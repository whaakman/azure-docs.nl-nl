---
title: HDInsight Clusters aanpassen met scriptacties - Azure
description: Informatie over het aanpassen van HDInsight-clusters met behulp van scriptacties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/05/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: ad59decab7233c74e13468b0cf0b11fdb5485d07
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722348"
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>HDInsight op basis van Windows clusters aanpassen met Script Action
**Actie script** kan worden gebruikt om aan te roepen [aangepaste scripts](hdinsight-hadoop-script-actions.md) tijdens het maken van het cluster voor het installeren van extra software op een cluster.

De informatie in dit artikel is specifiek voor HDInsight op basis van een Windows-clusters. Zie voor Linux gebaseerde clusters [aanpassen Linux gebaseerde HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]  
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

HDInsight-clusters kunnen worden aangepast in tal van andere manieren, zoals aanvullende Azure Storage-accounts wijzigen waaronder de [Apache Hadoop](https://hadoop.apache.org/) configuratiebestanden (core-site.xml, hive-site.xml, enzovoort) of toe te voegen gedeeld bibliotheken (bijvoorbeeld [Apache Hive](https://hive.apache.org/), [Apache Oozie](https://oozie.apache.org/)) in veelgebruikte locaties in het cluster. Deze aanpassingen kunnen worden gedaan via Azure PowerShell, Azure HDInsight .NET SDK of de Azure-portal. Zie voor meer informatie, [Apache Hadoop-clusters maken in HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Script Action tijdens het maken van het cluster
Script Action wordt alleen gebruikt terwijl een cluster wordt momenteel wordt gemaakt. Het volgende diagram illustreert wanneer scriptactie wordt uitgevoerd tijdens het maakproces:

![HDInsight-cluster aanpassen en -fasen tijdens het maken van clusters][img-hdi-cluster-states]

Wanneer het script wordt uitgevoerd, wordt het cluster krijgt de **ClusterCustomization** fase. In dit stadium het script wordt uitgevoerd onder het account system admin, parallel op de opgegeven knooppunten in het cluster, en biedt volledige beheerdersbevoegdheden op de knooppunten.

> [!NOTE]  
> Omdat u beheerdersbevoegdheden op de clusterknooppunten tijdens hebt de **ClusterCustomization** fase, kunt u het script gebruiken om uit te voeren bewerkingen zoals het stoppen en starten van services, waaronder Hadoop-gerelateerde services. Dus als onderdeel van het script, moet u ervoor zorgen dat de Ambari-services en andere Hadoop-gerelateerde services actief zijn voordat het script is voltooid. Deze services zijn vereist om na te gaan met succes de status en de status van het cluster terwijl deze wordt gemaakt. Als u geen configuratie op het cluster die gevolgen heeft voor deze services wijzigt, moet u de Help-functies die worden geleverd. Zie voor meer informatie over ondersteunende functies [ontwikkelen Script Action-scripts voor HDInsight][hdinsight-write-script].
>
>

De uitvoer en de foutenlogboeken van het script worden opgeslagen in het standaard Opslagaccount dat u hebt opgegeven voor het cluster. De logboeken worden opgeslagen in een tabel met de naam van de **u < \cluster-name-fragment >< \time-stamp > bestand**. Dit zijn de cumulatieve logboeken van het script uitgevoerd op alle knooppunten (hoofdknooppunt en worker-knooppunten) in het cluster.

Elk cluster accepteert meerdere scriptacties die worden aangeroepen in de volgorde waarin ze zijn opgegeven. Een script kan worden uitgevoerd op het hoofdknooppunt en/of de worker-knooppunten.

HDInsight biedt verschillende scripts voor het installeren van de volgende onderdelen in HDInsight-clusters:

| Name | Script |
| --- | --- |
| **Apache Spark installeren** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`. Zie [installeren en gebruiken Apache Spark in HDInsight-clusters][hdinsight-install-spark]. |
| **R installeren** | `https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1`. Zie [installeren en gebruiken R op HDInsight-clusters](r-server/r-server-hdinsight-manage.md#install-additional-r-packages-on-the-cluster). |
| **Apache Solr installeren** | `https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1`. Zie [installeren en gebruiken Apache Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md). |
| **Apache Giraph installeren** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`. Zie [installeren en gebruiken Apache Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md). |
| **Apache Hive-bibliotheken vooraf laden** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`. Zie [toevoegen Apache Hive-bibliotheken op HDInsight-clusters](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Aanroepen van scripts met Azure portal
**Vanuit Azure portal**

1. Beginnen met het maken van een cluster, zoals beschreven op [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. Onder optionele configuratie voor de **scriptacties** blade, klikt u op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Name</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>Script-URI</td>
            <td>Hiermee geeft u de URI naar het script dat wordt aangeroepen voor het aanpassen van het cluster. s</td></tr>
        <tr><td>HEAD/Worker</td>
            <td>Geef de knooppunten (**Head** of **Worker**) op waarmee het script aanpassing wordt uitgevoerd.</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script.</td></tr>
    </table>

    Druk op ENTER om toe te voegen van meer dan één scriptactie voor het installeren van meerdere onderdelen op het cluster.
3. Klik op **Selecteer** aan de configuratie van de actie script opslaan en doorgaan met het maken van clusters.

## <a name="call-scripts-using-azure-powershell"></a>Aanroepen van scripts met Azure PowerShell
Deze volgende PowerShell-script laat zien hoe u Spark installeren op Windows gebaseerde HDInsight-cluster.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Connect-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

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
        Connect-AzureRmAccount
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


Andere om software te installeren, moet u het scriptbestand in het script vervangen:

Wanneer u wordt gevraagd, typt u de referenties voor het cluster. Het kan enkele minuten duren voordat het cluster is gemaakt.

## <a name="call-scripts-using-net-sdk"></a>Aanroepen van scripts met behulp van .NET SDK
Het volgende voorbeeld ziet u hoe u Apache Spark op basis van HDInsight-cluster van Windows installeren. Als u wilt andere software installeert, moet u het scriptbestand in de code vervangen.

**Een HDInsight-cluster maken met Spark**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanuit de Nuget Package Manager-Console.

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
4. Plaats de code in de klasse met de volgende:

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

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source-software die wordt gebruikt op HDInsight-clusters
De Microsoft Azure HDInsight-service is een flexibel platform waarmee u big data-toepassingen in de cloud met behulp van een ecosysteem van open-source-technologieën die zijn gevormd rond Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source-technologieën, zoals beschreven in de **reikwijdte voor ondersteuning voor** sectie van de <a href="https://azure.microsoft.com/support/faq/" target="_blank">ondersteuning voor veelgestelde vragen over Azure-website</a>. De HDInsight-service biedt een extra beveiligingsniveau van de ondersteuning voor enkele van de onderdelen, zoals hieronder wordt beschreven.

Er zijn twee typen van open source-componenten die beschikbaar in de HDInsight-service zijn:

* **Ingebouwde onderdelen** -deze onderdelen vooraf zijn geïnstalleerd op HDInsight-clusters en bieden de kernfunctionaliteit van het cluster. Bijvoorbeeld, behoren Apache Hadoop YARN ResourceManager, de Hive-query-taal (HiveQL) en de Apache Mahout-bibliotheek tot deze categorie. Een volledige lijst van clusteronderdelen van het is beschikbaar in [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight?](hdinsight-component-versioning.md) </a>.
* **Aangepaste onderdelen** -u, als een gebruiker van het cluster kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte.

Ingebouwde onderdelen volledig worden ondersteund en Microsoft Support helpt om te isoleren en het oplossen van problemen met betrekking tot deze onderdelen.

> [!WARNING]  
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunnen opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Dit kan resulteren in het oplossen van het probleem of vraag of u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Er zijn bijvoorbeeld veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ https://apache.org ](https://apache.org), bijvoorbeeld: [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/).
>
>

De HDInsight-service biedt verschillende manieren om te gebruiken van aangepaste onderdelen. Ongeacht hoe een onderdeel gebruikt of is geïnstalleerd op het cluster, hetzelfde niveau van ondersteuning is van toepassing. Hieronder volgt een lijst van de meest gebruikte manieren dat aangepaste onderdelen in HDInsight-clusters kunnen worden gebruikt:

1. Taken verzenden - Hadoop- of andere typen taken die worden uitgevoerd of het gebruik van aangepaste onderdelen kan worden verzonden naar het cluster.
2. Aanpassing van de cluster - tijdens het maken van een cluster, kunt u aanvullende instellingen en aangepaste onderdelen die worden geïnstalleerd op de clusterknooppunten opgeven.
3. Voorbeelden van-voor populaire aangepaste onderdelen, Microsoft en andere mogelijk voorbeelden van hoe deze onderdelen kunnen worden gebruikt in de HDInsight-clusters bieden. Deze voorbeelden worden geleverd zonder ondersteuning.

## <a name="develop-script-action-scripts"></a>Script Action-scripts ontwikkelen
Zie [ontwikkelen Script Action-scripts voor HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Zie ook
* [Apache Hadoop-clusters maken in HDInsight] [ hdinsight-provision-cluster] vindt u instructies voor het maken van een HDInsight-cluster met behulp van andere aangepaste opties.
* [Script Action-scripts ontwikkelen voor HDInsight][hdinsight-write-script]
* [Installeren en gebruiken van Apache Spark op HDInsight-clusters][hdinsight-install-spark]
* [Installeren en gebruiken van Apache Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
* [Installeren en Apache Giraph gebruikt op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fasen tijdens het maken van clusters"
