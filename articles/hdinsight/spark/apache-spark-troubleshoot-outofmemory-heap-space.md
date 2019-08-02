---
title: Fout in Java-heap-ruimte bij het openen van Apache Spark geschiedenis server in azure HDInsight
description: Apache livy-server kan niet worden gestart met Java. lang. OutOfMemoryError in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/30/2019
ms.openlocfilehash: 5c2ae90bdca8512802c845a5ac58c3c4aeedd5b7
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667791"
---
# <a name="scenario-java-heap-space-error-when-trying-to-open-apache-spark-history-server-in-azure-hdinsight"></a>Scenario: Fout in Java-heap-ruimte bij het openen van Apache Spark geschiedenis server in azure HDInsight

In dit artikel worden probleemoplossings stappen en mogelijke oplossingen voor problemen beschreven bij het gebruik van Apache Spark-onderdelen in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

U ontvangt de volgende fout melding bij het openen van gebeurtenissen in de Spark-geschiedenis server:

```
scala.MatchError: java.lang.OutOfMemoryError: Java heap space (of class java.lang.OutOfMemoryError)
```

## <a name="cause"></a>Oorzaak

Dit probleem wordt vaak veroorzaakt door een gebrek aan resources bij het openen van grote bestanden met Spark-gebeurtenissen. De grootte van de Spark-heap is standaard ingesteld op 1 GB, maar grote Spark-gebeurtenis bestanden kunnen meer dan dit vereisen.

Als u de grootte van de bestanden die u wilt laden wilt controleren, kunt u de volgende opdrachten uitvoeren:

```bash
hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0274_1/
**576.5 M**  wasb:///hdp/spark2-events/application_1503957839788_0274_1

hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0264_1/
**2.1 G**  wasb:///hdp/spark2-events/application_1503957839788_0264_1
```

## <a name="resolution"></a>Oplossing

U kunt het Spark-geschiedenis server geheugen verhogen door de `SPARK_DAEMON_MEMORY` eigenschap in de Spark-configuratie te bewerken en alle services opnieuw te starten.

U kunt dit doen vanuit de gebruikers interface van de Ambari-browser door de sectie Spark2/config/Advanced Spark2-env te selecteren.

![Sectie Geavanceerde spark2-env](./media/apache-spark-ts-outofmemory-heap-space/image01.png)

Voeg de volgende eigenschap toe om het Spark-geschiedenis server geheugen van 1g te wijzigen `SPARK_DAEMON_MEMORY=4g`in 4G:.

![Spark-eigenschap](./media/apache-spark-ts-outofmemory-heap-space/image02.png)

Zorg ervoor dat u alle betrokken services opnieuw opstart vanuit Ambari.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
