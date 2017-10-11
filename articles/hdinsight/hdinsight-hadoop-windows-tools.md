---
title: Een Windows-PC gebruiken met Hadoop op HDInsight - Azure | Microsoft Docs
description: Werken vanuit een Windows-PC in Hadoop op HDInsight. Query-clusters met PowerShell, Visual Studio- en Linux-hulpprogramma's en beheren. Ontwikkel big data-oplossingen met .NET.
services: hdinsight
keywords: hadoop op windows, hadoop voor windows
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.openlocfilehash: e4f231c1f9b903d6cc7f2b062b30d2a072be8493
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="work-in-the-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Werken in het Hadoop-ecosysteem in HDInsight via een Windows-PC

Meer informatie over de ontwikkeling en opties op de Windows-computer voor het gebruik van het Hadoop-ecosysteem in HDInsight. 

HDInsight is gebaseerd op Apache Hadoop en Hadoop-onderdelen, open source-technologieën die zijn ontwikkeld op Linux. HDInsight versie 3.4 en hogere gebruikt de Ubuntu Linux-distributie als het onderliggende besturingssysteem voor het cluster. U kunt echter werken met HDInsight vanuit een Windows-client of een Windows-ontwikkelomgeving.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>PowerShell gebruiken voor implementatie en beheer van taken
Azure PowerShell is een scriptomgeving op servers die u gebruiken kunt om te beheren en de implementatie en beheertaken in HDInsight via Windows automatiseren.

Voorbeelden van taken die u met PowerShell uitvoeren kunt:

* [Maken van clusters met behulp van PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Uitvoeren van Hive-query's met behulp van PowerShell](hdinsight-hadoop-use-hive-powershell.md)
* [Clusters met PowerShell beheren](hdinsight-administer-use-powershell.md)

Volg de stappen voor het [Azure Powershell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) naar de nieuwste versie. Als u scripts die worden gewijzigd hebt moeten voor gebruik van de nieuwe cmdlets voor Azure Resource Manager, raadpleegt [migreren naar Azure Resource Manager gebaseerde ontwikkelprogramma's voor HDInsight-clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Hulpprogramma's die u kunt uitvoeren in een browser
De volgende hulpprogramma's hebben een webgebruikersinterface die in een browser wordt uitgevoerd:
* **[Azure Cloud-Shell (preview)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  is een interactieve, opdrachtregel-shell die wordt uitgevoerd in uw browser en vanuit de Azure-portal.
* **[Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)**  is een beheer en controle hulpprogramma beschikbaar in de Azure portal die kan worden gebruikt voor het beheren van verschillende soorten taken, zoals:
    * [Ambari gebruiken met de REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Weergave in de Ambari hive](hdinsight-hadoop-use-hive-ambari-view.md)
    * [Tez-weergave in de Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>(Hadoop) van Data Lake Tools voor Visual Studio
Data Lake Tools voor Visual Studio gebruiken om te implementeren en beheren van Storm-topologieën. Data Lake Tools installeert ook de SDK SCP.NET, zodat u kunt de C# Storm-topologieën met Visual Studio ontwikkelen.

Voordat u met de volgende voorbeelden verdergaat [installeren en probeer het Data Lake Tools voor Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md). 

Voorbeelden van taken die u met Visual Studio en Data Lake Tools voor Visual Studio doen kunt:
* [Implementeren en beheren van Storm-topologieën vanuit Visual Studio](hdinsight-storm-deploy-monitor-topology-linux.md)
* [C#-topologieën ontwikkelen voor Storm met Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md). De bits bevatten voorbeeld sjablonen voor Storm-topologieën die u verbinding met databases, zoals Azure Cosmos DB en SQL-Database maken kunt.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio en de .NET SDK 

U kunt Visual Studio met de .NET SDK gebruiken voor het beheren van clusters en big data-toepassingen te ontwikkelen. U kunt andere IDE gebruiken voor de volgende taken, maar voorbeelden worden weergegeven in Visual Studio.

Voorbeelden van taken die u met de .NET SDK in Visual Studio doen kunt:
* [Clusters maken en gebruiken in HDInsight vanuit een .NET Framework-toepassing](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Uitvoeren van Hive-query's met de .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [C# gebruiker gedefinieerde functies gebruiken met Hive en Pig streaming op Hadoop](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

> TIP als u .NET-oplossingen met HDInsight clusters op basis van Windows uitvoert, is het een goed moment om te migreren naar op basis van Linux-clusters. Zie voor meer informatie [migreren .NET-oplossing voor HDInsight op Linux gebaseerde HDInsight op basis van Windows](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA en Eclipse IDE voor Spark-clusters
Beide [Intellij IDEA](https://www.jetbrains.com/idea/download) en de [Eclipse IDE](https://www.eclipse.org/downloads/) kan worden gebruikt voor:
* Ontwikkelen en het verzenden van een Scala Spark-toepassing op een HDInsight Spark-cluster.
* Toegang tot bronnen van Spark-cluster.
* Ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren.

Deze artikelen laten zien hoe: 
* Intellij IDEA: [maken Spark scala-toepassingen met behulp van de Azure-werkset voor Intellij-invoegtoepassing en de Scala-SDK.](hdinsight-apache-spark-intellij-tool-plugin.md)
* Eclipse IDE- of IDE Scala voor Eclipse: [maken Spark scala-toepassingen en de Azure-werkset voor Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Notitieblokken op Spark voor gegevenswetenschappers 
Apache Spark-clusters in HDInsight omvat Zeppelin-notebooks en kernels die kunnen worden gebruikt met Jupyter-notebooks. 

* [Informatie over het gebruik van kernels op Spark-clusters met Jupyter-notebooks Spark scala-toepassingen testen](hdinsight-apache-spark-zeppelin-notebook.md)
* [Informatie over het gebruik van Zeppelin-notebooks op Spark-clusters Spark taken uitvoeren](hdinsight-apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Linux-gebaseerde hulpprogramma's en -technologieën in Windows worden uitgevoerd

Als u een situatie waarin moet u een hulpprogramma of de technologie die alleen beschikbaar op Linux ondervindt, kunt u de volgende opties:

* **Bash (bèta) op Windows 10** biedt een Linux-subsysteem in Windows. Bash, kunt u rechtstreeks Linux-hulpprogramma's uitvoeren zonder dat de installatie van een speciale Linux onderhouden. [Installeren en uitvoeren van de bètaversie Bash op Windows 10](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Docker voor Windows** biedt toegang tot veel Linux-gebaseerde hulpprogramma's en rechtstreeks vanuit Windows kan worden uitgevoerd. U kunt bijvoorbeeld Docker gebruiken om uit te voeren van de client Beeline voor Hive rechtstreeks vanuit Windows. U kunt Docker ook gebruiken om uit te voeren van een lokale Jupyter-notebook en extern verbinding maken met Spark in HDInsight. [Aan de slag met Docker voor Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  kunt u grafisch bladeren naar het bestandssysteem van het cluster via een SSH-verbinding.

## <a name="next-steps"></a>Volgende stappen
Als u geen ervaring met op Linux gebaseerde clusters werkt, raadpleegt u de volgende artikelen:
* [Hadoop, Kafka, Spark of andere clusters instellen](hdinsight-hadoop-provision-linux-clusters.md)
* [Tips voor het HDInsight-clusters op Linux](hdinsight-hadoop-linux-information.md)