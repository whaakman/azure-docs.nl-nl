---
title: HDInsight-cluster upgraden naar een nieuwere versie-Azure | Microsoft Docs
description: Meer informatie over het upgraden van HDInsight-cluster naar een nieuwere versie.
services: hdinsight
documentationcenter: ''
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: 8845a049ebcda59bc0e6fd26618c33f51565e0ca
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225486"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>HDInsight-cluster upgraden naar een nieuwere versie
Als u wilt profiteren van de nieuwste functies voor HDInsight, is het raadzaam dat HDInsight-clusters worden bijgewerkt naar de meest recente versie. Volg de onderstaande richtlijnen om te upgraden van uw HDInsight-cluster versies.

> [!NOTE]
> Zie voor meer informatie over de ondersteunde versies van HDInsight [HDInsight onderdeel versies](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Upgrade uitvoeren van taken
De werkstroom om HDInsight-Cluster te upgraden is als volgt.

![Diagram van werkstroom bijwerken](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Lees elke sectie van dit document om te begrijpen van wijzigingen die mogelijk vereist bij het upgraden van uw HDInsight-cluster.
2. Maak een cluster als een test/kwaliteit assurance-omgeving. Zie voor meer informatie over het maken van een cluster [informatie over het maken van HDInsight op basis van Linux-clusters](hdinsight-hadoop-provision-linux-clusters.md)
3. Bestaande taken, gegevensbronnen en sinks kopiëren naar de nieuwe omgeving. Zie [Copy Data te testomgeving](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) voor meer informatie.
4. Voer validatietests om ervoor te zorgen dat uw taken werken zoals verwacht op het nieuwe cluster.


Nadat u hebt gecontroleerd dat alles werkt zoals verwacht, plant u uitvaltijd voor de migratie. Tijdens deze uitval, moet u de volgende acties uitvoeren:

1.  Back-up van alle tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten. Bijvoorbeeld, als u gegevens hebt opgeslagen rechtstreeks op een hoofdknooppunt.
2.  Het bestaande cluster verwijderen.
3.  Een cluster maken in hetzelfde VNET-subnet met de meest recente (of ondersteunde) HDI-versie met behulp van de dezelfde standaard-gegevensarchief die het voorgaande cluster gebruikt. Hiermee wordt het nieuwe cluster om door te gaan werken met uw bestaande productiegegevens.
4.  Importeer geen tijdelijke gegevens die u back-up gemaakt.
5.  Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het maken van HDInsight op basis van Linux-clusters](hdinsight-hadoop-provision-linux-clusters.md)
* [Verbinding maken met HDInsight met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Een Linux-gebaseerde cluster beheren met Ambari](hdinsight-hadoop-manage-ambari.md)

