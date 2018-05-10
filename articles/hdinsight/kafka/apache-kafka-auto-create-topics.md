---
title: Automatische onderwerp maken in Apache Kafka - Azure HDInsight | Microsoft Docs
description: Informatie over het configureren van Apache Kafka in HDInsight voor het automatisch maken van onderwerpen. U kunt Kafka configureren door auto.create.topics.enable op waar via Ambari of tijdens het maken van clusters met PowerShell of de Resource Manager-sjablonen.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/18/2018
ms.author: larryfr
ms.openlocfilehash: fa5dd7533259c794671cd16231fd3f530173bfa3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Apache Kafka op HDInsight voor het automatisch maken van onderwerpen configureren

Standaard wordt Kafka op HDInsight niet ingeschakeld voor het maken van automatische onderwerp. U kunt het automatisch onderwerp maken voor bestaande clusters met Ambari inschakelen. U kunt ook automatisch onderwerp maken inschakelen bij het maken van een nieuw Kafka-cluster met een Azure Resource Manager-sjabloon.

## <a name="ambari-web-ui"></a>Ambari-webgebruikersinterface

Als u wilt maken van automatische onderwerp op een bestaand cluster via de Ambari-Webgebruikersinterface inschakelt, gebruiken de volgende stappen uit:

1. Van de [Azure-portal](https://portal.azure.com), selecteert u het cluster Kafka.

2. Van de __Cluster overzicht__, selecteer __Cluster-dashboard__. 

    ![Afbeelding van de portal met cluster-dashboard geselecteerd](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Selecteer vervolgens __HDInsight-cluster-dashboard__. Wanneer u wordt gevraagd, worden geverifieerd met behulp van de aanmeldingsreferenties (admin) voor het cluster.

    ![Afbeelding van de HDInsight-cluster dashboard vermelding](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Selecteer de Kafka-service in de lijst aan de linkerkant van de pagina.

    ![Lijst met Services](./media/apache-kafka-auto-create-topics/service-list.png)

4. Selecteer Configs in het midden van de pagina.

    ![Tabblad voor service-configuratie](./media/apache-kafka-auto-create-topics/service-config.png)

5. Voer een waarde van in het filterveld `auto.create`. 

    ![Afbeelding van het filterveld](./media/apache-kafka-auto-create-topics/filter.png)

    Dit filtert de lijst met eigenschappen en geeft de `auto.create.topics.enable` instelling.

6. Wijzig de waarde van `auto.create.topics.enable` naar `true`, en selecteer vervolgens opslaan. Een opmerking toevoegen en selecteert u vervolgens opslaan.

    ![Afbeelding van de vermelding auto.create.topics.enable](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. De service Kafka selecteert, selecteert u __opnieuw__, en selecteer vervolgens __opnieuw alle betrokken__. Wanneer u wordt gevraagd, selecteert u __bevestigen start opnieuw alle__.

    ![Afbeelding van de selectie van opnieuw opstarten](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]
> U kunt ook instellen Ambari waarden met de Ambari REST-API. Dit is meestal moeilijker, hebt u meerdere REST-aanroepen voor het ophalen van de huidige configuratie, wijzigt u het, enzovoort. Zie voor meer informatie de [HDInsight-clusters beheren met de Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md) document.

## <a name="resource-manager-templates"></a>Resource Manager-sjablonen

Wanneer u een Kafka-cluster met een Azure Resource Manager-sjabloon maakt, kunt u rechtstreeks instellen `auto.create.topics.enable` door toe te voegen in een `kafka-broker`. De volgende JSON-fragment laat zien hoe u deze waarde instelt op `true`:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u automatische onderwerp maken voor Kafka op HDInsight. Zie de volgende koppelingen voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)