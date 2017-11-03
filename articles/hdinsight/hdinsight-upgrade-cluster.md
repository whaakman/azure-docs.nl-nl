---
title: Upgrade HDInsight-cluster naar een nieuwere versie-Azure | Microsoft Docs
description: Meer informatie over het upgraden van HDInsight-cluster naar een nieuwere versie.
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: fa2e37bd922690322ccc3d8f68128180d013b701
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>HDInsight-cluster naar een nieuwere versie upgraden
Om te profiteren van de nieuwste functies van HDInsight, is het raadzaam dat HDInsight-clusters worden bijgewerkt naar de nieuwste versie. Volg de onderstaande richtlijnen voor het upgraden van uw HDInsight-cluster versies.

> [!NOTE]
> HDInsight-clusters versie 3.2 en 3.3 bijna intrekkingsdatum zijn. Zie voor informatie over de ondersteunde versie van HDInsight, [HDInsight onderdeel versies](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Upgradetaken
De werkstroom om bij te werken van HDInsight-Cluster is als volgt.

![Werkstroom voor serverupgrades diagram](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Elke sectie van dit document om te begrijpen wijzigingen die mogelijk vereist zijn bij een upgrade van uw HDInsight-cluster worden gelezen.
2. Maak een cluster als een test/quality assurance-omgeving. Zie voor meer informatie over het maken van een cluster [informatie over het maken van Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md)
3. Bestaande projecten, gegevensbronnen en put kopiëren naar de nieuwe omgeving. Zie [kopie gegevens naar testomgeving](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) voor meer informatie.
4. Voer de validatie testen om ervoor te zorgen dat uw taken werken zoals verwacht op het nieuwe cluster.


Zodra u hebt gecontroleerd dat alles werkt zoals verwacht, plant u uitvaltijd voor de migratie. Tijdens deze uitvaltijd, moet u de volgende acties uitvoeren:

1.  Back-up tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten. Bijvoorbeeld, als u gegevens hebt opgeslagen rechtstreeks op een hoofdknooppunt.
2.  Het bestaande cluster verwijderen.
3.  Maak een cluster in hetzelfde VNET subnet met de meest recente (of ondersteunde) HDI-versie met behulp van hetzelfde standaard gegevensarchief die eerder cluster gebruikt. Hiermee wordt het nieuwe cluster om te blijven werken met uw bestaande productiegegevens.
4.  Importeer tijdelijke gegevens die u een back-up.
5.  Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het maken van Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md)
* [Verbinding maken met HDInsight met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Beheer op basis van Linux clusters met Ambari](hdinsight-hadoop-manage-ambari.md)

