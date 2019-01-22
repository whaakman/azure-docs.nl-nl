---
title: Een Windows-PC gebruiken met Hadoop op HDInsight - Azure
description: Werken vanaf een Windows-PC in Hadoop op HDInsight. Query-clusters met PowerShell, Visual Studio en Linux-hulpprogramma's en beheren. Ontwikkeling van big data-oplossingen met .NET.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 05/17/2017
ms.openlocfilehash: d336d659a15f5b4cb1cbc917a8ae10ab8224d029
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426315"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Werken in het Apache Hadoop-ecosysteem in HDInsight vanuit een Windows-PC

Meer informatie over de ontwikkeling en opties op de Windows-PC voor het werken in het Apache Hadoop-ecosysteem in HDInsight. 

HDInsight is gebaseerd op Apache Hadoop- en Hadoop-onderdelen, open-source-technologieën die zijn ontwikkeld in Linux. HDInsight versie 3.4 en hoger wordt de Ubuntu Linux-distributie gebruikt als het onderliggende besturingssysteem voor het cluster. U kunt echter werken met HDInsight vanaf een Windows-client of Windows-ontwikkelomgeving.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>PowerShell gebruiken voor implementatie en beheer taken
Azure PowerShell is een scriptomgeving die u gebruiken kunt om te beheren en automatiseren van de implementatie en beheertaken in HDInsight vanuit Windows.

Voorbeelden van taken die u met PowerShell uitvoeren kunt:

* [Maken van clusters met behulp van PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).
* [Apache Hive-query's uitvoeren met behulp van PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).
* [Clusters beheren met PowerShell](hdinsight-administer-use-powershell.md).

Volg de stappen voor het [Azure Powershell installeren en configureren](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) naar de meest recente versie. Als u scripts die worden gewijzigd hebt moeten voor gebruik van de nieuwe cmdlets voor Azure Resource Manager, [migreren naar Azure Resource Manager gebaseerde ontwikkelingsprogramma's voor HDInsight-clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Hulpprogramma's die u kunt uitvoeren in een browser
De volgende hulpprogramma's hebben een webinterface die wordt uitgevoerd in een browser:
* **[Azure Cloud Shell (preview)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  is een interactieve, opdrachtregel-shell die wordt uitgevoerd in uw browser en vanuit Azure portal.
* **[Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)**  is een beheer en controle hulpprogramma is beschikbaar in de Azure portal die kan worden gebruikt voor het beheren van verschillende soorten taken, zoals:
    * [Gebruik Apache Ambari met de REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Apache Hive-weergave in Apache Ambari](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Apache Tez weergave in Apache Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Data Lake (Hadoop) Tools voor Visual Studio
Data Lake Tools voor Visual Studio gebruiken om te implementeren en beheren van Storm-topologieën. Data Lake-Tools installeert ook de SDK voor SCP.NET, zodat u kunt C# Storm-topologieën ontwikkelen met Visual Studio.

Voordat u met de volgende voorbeelden verdergaat [installeren en probeer het Data Lake Tools voor Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md). 

Voorbeelden van taken die u met Visual Studio en Data Lake Tools voor Visual Studio uitvoeren kunt:
* [Implementeren en beheren van Storm-topologieën vanuit Visual Studio](storm/apache-storm-deploy-monitor-topology-linux.md)
* [C#-topologieën ontwikkelen voor Storm met Visual Studio](storm/apache-storm-develop-csharp-visual-studio-topology.md). De bits bevatten voorbeeldsjablonen voor Storm-topologieën die u verbinding met databases, zoals Azure Cosmos DB en SQL-Database maken kunt.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio en de .NET SDK 

U kunt Visual Studio met de .NET SDK gebruiken voor het beheren van clusters en big data-toepassingen ontwikkelen. U kunt andere IDE's gebruiken voor de volgende taken, maar voorbeelden worden weergegeven in Visual Studio.

Voorbeelden van taken die u met de .NET SDK in Visual Studio kunt doen:
* [Clusters maken en gebruiken in HDInsight vanuit een .NET Framework-toepassing](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).
* [Apache Hive-query's uitvoeren met de .NET SDK](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).
* [Gebruik C# door de gebruiker gedefinieerde functies met Apache Hive en Apache Pig streaming op Apache Hadoop](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md).

> [!TIP]
> Als u .NET-oplossingen met HDInsight op basis van een Windows-clusters uitvoert, is een goed moment om het plannen van een migratie naar op basis van Linux-clusters. Zie voor meer informatie, [migreren .NET-oplossing voor HDInsight op Linux gebaseerde HDInsight op basis van Windows](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA en Eclipse IDE voor Spark-clusters
Beide [Intellij IDEA](https://www.jetbrains.com/idea/download) en de [Eclipse IDE](https://www.eclipse.org/downloads/) kunnen worden gebruikt voor:
* Ontwikkelen en het verzenden van een Scala Spark-toepassing in een HDInsight Spark-cluster.
* Toegang tot resources Spark-cluster.
* Ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren.

Deze artikelen wordt beschreven hoe: 
* Intellij IDEA: [Apache Spark-toepassingen met behulp van de Azure Toolkit voor Intellij-invoegtoepassing en de SDK Scala maken.](spark/apache-spark-intellij-tool-plugin.md)
* Eclipse IDE of Scala IDE voor Eclipse: [Maken van Apache Spark-toepassingen en de Azure Toolkit voor Eclipse](spark/apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Notitieblokken op Spark voor gegevensanalisten 
Apache Spark-clusters in HDInsight omvat Apache Zeppelin-notebooks en kernels die kunnen worden gebruikt met Jupyter-notebooks. 

* [Meer informatie over het kernels op Apache Spark-clusters met Jupyter-notebooks gebruiken om Spark-toepassingen te testen](spark/apache-spark-zeppelin-notebook.md)
* [Informatie over het gebruik van Apache Zeppelin-notebooks op Apache Spark-clusters op Spark-taken uitvoeren](spark/apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Linux-gebaseerde hulpprogramma's en technologieën uitgevoerd op Windows

Als er een situatie waarbij moet u een hulpprogramma of -technologie die is alleen beschikbaar in Linux gebruiken, houd rekening met de volgende opties:

* **Bash (bèta) in Windows 10** een subsysteem voor Linux biedt op Windows. Bash kunt u rechtstreeks uitvoeren van Linux-hulpprogramma's zonder te onderhouden van een specifieke Linux-installatie. [Installeren en uitvoeren van de bètaversie van Bash op Windows 10](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Docker voor Windows** biedt toegang tot veel Linux-gebaseerde hulpprogramma's en rechtstreeks vanuit Windows kan worden uitgevoerd. U kunt bijvoorbeeld Docker gebruiken om uit te voeren van de client Beeline voor Hive rechtstreeks vanuit Windows. U kunt ook Docker gebruiken om uit te voeren van een lokaal Jupyter-notitieblok en op afstand verbinding maken met Spark in HDInsight. [Aan de slag met Docker voor Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](https://mobaxterm.mobatek.net/)**  kunt u grafisch bladeren naar het bestandssysteem van het cluster via een SSH-verbinding.

## <a name="next-steps"></a>Volgende stappen
Als u geen ervaring hebt met werken in op basis van Linux-clusters, ziet u de volgende artikelen:
* [Apache Hadoop, Apache Kafka, Apache Spark of andere clusters instellen](hdinsight-hadoop-provision-linux-clusters.md)
* [Tips voor het HDInsight-clusters in Linux](hdinsight-hadoop-linux-information.md)
