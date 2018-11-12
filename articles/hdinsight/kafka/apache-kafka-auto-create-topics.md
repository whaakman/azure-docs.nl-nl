---
title: Automatisch onderwerp maken in Apache Kafka - Azure HDInsight inschakelen
description: Informatie over het configureren van Apache Kafka op HDInsight onderwerpen automatisch worden gemaakt. U kunt Kafka configureren door auto.create.topics.enable op ' True ' via Ambari of tijdens het maken van clusters via PowerShell of Resource Manager-sjablonen.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/18/2018
ms.openlocfilehash: 65dd4b172c0913e11bb7f25909eb062e93a4a03a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015227"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Apache Kafka op HDInsight voor het automatisch maken van onderwerpen configureren

Kafka in HDInsight wordt standaard niet ingeschakeld automatisch onderwerp maken. U kunt automatisch onderwerp maken voor bestaande clusters met behulp van Ambari inschakelen. U kunt ook automatisch onderwerp maken inschakelen bij het maken van een nieuwe Kafka-cluster met behulp van een Azure Resource Manager-sjabloon.

## <a name="ambari-web-ui"></a>Ambari-Webgebruikersinterface

Om in te schakelen automatisch onderwerp maken op een bestaand cluster via de Ambari-Webgebruikersinterface, gebruikt u de volgende stappen uit:

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u het Kafka-cluster.

2. Uit de __Cluster overzicht__, selecteer __clusterdashboard__. 

    ![Afbeelding van de portal met clusterdashboard geselecteerd](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Selecteer vervolgens __HDInsight-clusterdashboard__. Wanneer u hierom wordt gevraagd, moet u verifiëren met behulp van de referenties voor clusteraanmelding (admin) voor het cluster.

    ![Afbeelding van de vermelding van HDInsight-cluster-dashboard](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Selecteer de Kafka-service in de lijst aan de linkerkant van de pagina.

    ![Lijst met Services](./media/apache-kafka-auto-create-topics/service-list.png)

4. Selecteer configuraties in het midden van de pagina.

    ![Tabblad voor service-configuratie](./media/apache-kafka-auto-create-topics/service-config.png)

5. Voer de waarde in het filterveld in `auto.create`. 

    ![Afbeelding van het filterveld](./media/apache-kafka-auto-create-topics/filter.png)

    Hiermee filtert u de lijst met eigenschappen en geeft de `auto.create.topics.enable` instelling.

6. Wijzig de waarde van `auto.create.topics.enable` naar `true`, en selecteer vervolgens opslaan. Een opmerking toevoegen en selecteert u vervolgens opslaan.

    ![Afbeelding van de vermelding auto.create.topics.enable](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Selecteer de Kafka-service, selecteer __opnieuw__, en selecteer vervolgens __opnieuw alle betrokken__. Wanneer u hierom wordt gevraagd, selecteert u __bevestigen opnieuw alle__.

    ![Afbeelding van opnieuw opstarten selecteren](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]
> U kunt ook instellen Ambari waarden met de Ambari REST-API. Dit is meestal moeilijker, als er meerdere REST-aanroepen naar de huidige configuratie ophalen, wijzigt u het, enzovoort. Zie voor meer informatie de [beheren HDInsight-clusters met behulp van de Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md) document.

## <a name="resource-manager-templates"></a>Resource Manager-sjablonen

Bij het maken van een Kafka-cluster met behulp van een Azure Resource Manager-sjabloon, kunt u rechtstreeks instellen `auto.create.topics.enable` door toe te voegen in een `kafka-broker`. De volgende JSON-fragment ziet u hoe u deze waarde instelt op `true`:

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

In dit document hebt u geleerd hoe u voor het maken van automatische onderwerp voor Kafka in HDInsight. Zie de volgende koppelingen voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
