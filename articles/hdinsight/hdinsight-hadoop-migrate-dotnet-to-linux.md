---
title: .NET gebruiken met Hadoop-MapReduce op Linux gebaseerde HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van .NET-toepassingen voor streaming MapReduce op Linux gebaseerde HDInsight.
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: 2657db61ff3161cd87bc97edfe5f84f8b29cbcfb
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>Migreren van .NET-oplossingen voor HDInsight op Linux gebaseerde HDInsight op basis van Windows

Linux gebaseerde HDInsight-clusters gebruik [Mono (https://mono-project.com)](https://mono-project.com) .NET-toepassingen uit te voeren. Mono kunt u .NET-componenten zoals MapReduce-toepassingen gebruiken met HDInsight op basis van Linux. Informatie over het migreren van .NET-oplossingen voor Windows gebaseerde HDInsight-clusters werkt met Mono op Linux gebaseerde HDInsight gemaakt in dit document.

## <a name="mono-compatibility-with-net"></a>Mono compatibiliteit met .NET

Mono versie 4.2.1 is opgenomen in HDInsight versie 3.6. Zie voor meer informatie over de versie van Mono opgenomen met HDInsight [HDInsight onderdeel versies](hdinsight-component-versioning.md). Zie het installeren van een specifieke versie van Mono de [installeren of bijwerken van Mono](hdinsight-hadoop-install-mono.md) document.

Zie voor meer informatie over de compatibiliteit tussen Mono en .NET de [Mono compatibiliteit (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) document.

> [!IMPORTANT]
> Het framework SCP.NET is compatibel met Mono. Zie voor meer informatie over het gebruik van SCP.NET met Mono [Gebruik Visual Studio C#-topologieën ontwikkelen voor Apache Storm op HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Analyse van geautomatiseerde draagbaarheid

De [.NET draagbaarheid Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) kan worden gebruikt om een rapport van compatibiliteitsproblemen tussen de toepassing en Mono te genereren. Gebruik de volgende stappen voor het configureren van de analyzer om te controleren van uw toepassing voor Mono draagbaarheid:

1. Installeer de [.NET draagbaarheid Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Selecteer de versie van Visual Studio te gebruiken tijdens de installatie.

2. Selecteer in Visual Studio 2015 __analyseren__ > __draagbaarheid Analyzer instellingen__, en zorg ervoor dat __4.5__ is ingeschakeld de __Mono__ sectie.

    ![4.5 Mono sectie voor de instellingen analyzer ingecheckt](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Selecteer __OK__ aan de configuratie op te slaan.

3. Selecteer __analyseren__ > __analyseren Assembly draagbaarheid__. Selecteer de assembly waarin uw oplossing en selecteer vervolgens __Open__ om te beginnen met de analyse.

4. Zodra de analyse is voltooid, selecteert u __analyseren__ > __analyserapporten weergeven__. In __draagbaarheid analyseresultaten__, selecteer __rapport openen__ om een rapport te openen.

    ![Draagbaarheid analyzer resultaten dialoogvenster](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> De analyzer kan geen catch elk probleem met uw oplossing. Bijvoorbeeld, een bestandspad van `c:\temp\file.txt` wordt beschouwd als OK als Mono wordt uitgevoerd op Windows. Hetzelfde pad is niet geldig voor een Linux-platform.

## <a name="manual-portability-analysis"></a>Handmatige draagbaarheid analyse

Voer een handmatige controle van uw code met de informatie in de [toepassing draagbaarheid (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) document.

## <a name="modify-and-build"></a>Wijzigen en maken

U kunt blijven gebruiken van Visual Studio voor het bouwen van uw .NET-oplossingen voor HDInsight. Echter, moet u ervoor zorgen dat het project is geconfigureerd voor gebruik van .NET Framework 4.5.

## <a name="deploy-and-test"></a>Implementeren en testen

Als u uw oplossing met behulp van de aanbevelingen van de .NET draagbaarheid Analyzer of van een handmatige analyse hebt gewijzigd, moet u het testen met HDInsight. Testen van de oplossing op een Linux gebaseerde HDInsight-cluster kan subtiele problemen onthullen die moeten worden gecorrigeerd. Het is raadzaam dat u aanvullende logboekregistratie inschakelen in uw toepassing bij het testen.

Zie de volgende documenten voor meer informatie over de toegang tot logboeken:

* [Toegang tot YARN-toepassingslogboeken in een HDInsight-cluster op basis van Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Volgende stappen

* [Gebruik C# met MapReduce in HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [C# gebruiker gedefinieerde functies gebruiken met Hive en Pig](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [C#-topologieën ontwikkelen voor Storm op HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md)