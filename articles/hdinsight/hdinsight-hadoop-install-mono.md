---
title: Mono op HDInsight - Azure installeren of bijwerken
description: Leer hoe u een specifieke versie van Mono gebruiken met HDInsight-cluster. Mono wordt gebruikt voor het uitvoeren van .NET-toepassingen op Linux gebaseerde HDInsight-clusters.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.custom: hdinsightactive
ms.openlocfilehash: 9daf0d45c9bb24192750991e7bf84fa3614c970d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011031"
---
# <a name="install-or-update-mono-on-hdinsight"></a>Mono op HDInsight installeren of bijwerken

Informatie over het installeren van een specifieke versie van [Mono](https://www.mono-project.com) op HDInsight 3.4 of hoger.

Mono op HDInsight 3.4 en hoger is geïnstalleerd en wordt gebruikt voor het uitvoeren van .NET-toepassingen. Zie voor informatie over de versie van Mono opgenomen in elke versie van HDInsight, [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md). Voor het installeren van een andere versie op het cluster, gebruikt u de scriptactie in dit document. 

## <a name="how-it-works"></a>Hoe werkt het?

Met dit script accepteert de volgende parameter:

* __Mono versienummer__: de versie van Mono installeren. De versie moet beschikbaar zijn vanuit [ https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/ ](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

Het script wordt de volgende Mono-pakketten geïnstalleerd:

* __Mono-voltooien__

* __ca-certificates-mono__

## <a name="the-script"></a>Het script

__Scriptlocatie__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Vereisten voor__:

* Het script moet worden toegepast op de __hoofdknooppunten__ en __worker-knooppunten__.

## <a name="to-use-the-script"></a>Het gebruik van het script

Zie voor meer informatie over het gebruik van dit script met HDInsight, de [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document. U kunt het script via de Azure-portal, Azure PowerShell of de klassieke Azure-CLI gebruiken.

Tijdens het script actie document te volgen, gebruikt u de volgende URI:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

De Mono-versie die is geïnstalleerd, gebruikt u het versienummer in de __Parameters__ veld. Voer bijvoorbeeld `5.4` Mono 5.4 installeren.

> [!NOTE]
> Wanneer HDInsight met dit script configureert, markeert u het script als __persistente__. Deze instelling kunt HDInsight om toe te passen van het script op de worker-knooppunten die zijn toegevoegd via het schalen herverdelen.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een upgrade uitvoert of een specifieke versie van Mono installeren op HDInsight. Zie de volgende documenten voor meer informatie over het gebruik van .NET-toepassingen met Mono op HDInsight:

* [Gebruik .NET voor het streamen van MapReduce in HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [.NET gebruiken met Hive en Pig in HDInsight](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)
* [C#-oplossingen met Storm op HDInsight ontwikkelen](storm/apache-storm-develop-csharp-visual-studio-topology.md)
* [.NET-oplossingen migreren naar HDInsight op basis van Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Zie voor meer informatie over het gebruik van scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md)
