---
title: Prestaties van Apache Spark-workloads met Azure HDInsight i/o-Cache (Preview)
description: Meer informatie over Azure HDInsight i/o-Cache en het gebruik van het Apache Spark-prestaties te verbeteren.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 10/15/2018
ms.openlocfilehash: 724e6c57f10fb85b4b91c2236d17a64899953d67
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581932"
---
# <a name="improve-performance-of-apache-spark-workloads-using-azure-hdinsight-io-cache-preview"></a>Prestaties van Apache Spark-workloads met Azure HDInsight i/o-Cache (Preview)

I/o-Cache is een service voor Azure HDInsight, waardoor de prestaties van Apache Spark-taken voor het opslaan in cache van gegevens. I/o-Cache werkt ook met [Apache TEZ](https://tez.apache.org/) en [Apache Hive](https://hive.apache.org/) werkbelastingen, die kunnen worden uitgevoerd op [Apache Spark](https://spark.apache.org/) clusters. I/o-Cache maakt gebruik van een open-source-component opslaan in cache RubiX genoemd. RubiX is een lokale schijf-cache voor gebruik met big data analytics-engines met toegang gegevens van cloudopslagsystemen tot. RubiX is uniek zijn in de cache-systemen, omdat hierbij Solid-State stations (SSD's) in plaats van de operationele geheugen reserveren voor cachedoeleinden. De i/o-Cache-service wordt gestart en RubiX metagegevens Servers op elke worker-knooppunt van het cluster beheert. Het ook configureert alle services van het cluster voor transparante gebruik van RubiX cache.

De meeste SSD's bieden meer dan 1 GB per seconde van de bandbreedte. Deze bandbreedte, aangevuld met de besturingssysteem-bestand in-memory-cache, biedt voldoende bandbreedte voor het laden van big compute verwerking gegevensengines, zoals Apache Spark. Het operationele geheugen is beschikbaar voor Apache Spark voor het verwerken van veel geheugen-afhankelijke taken, zoals shuffles van links. Exclusief gebruik van het besturingssysteem van geheugen met kunt Apache Spark voor optimaal Resourcegebruik.  

>[!Note]
>I/o-Cache gebruikt momenteel RubiX als onderdeel van een cache, maar dit kan veranderen in toekomstige versies van de service. Gebruik van i/o-Cache-interfaces en eventuele afhankelijkheden rechtstreeks op de implementatie RubiX niet uitvoeren.

## <a name="benefits-of-azure-hdinsight-io-cache"></a>Voordelen van Azure HDInsight i/o-Cache

Gebruik van i/o-Cache, biedt een toename van de prestaties voor taken die gegevens uit Azure Blob Storage lezen.

U hebt geen wijzigingen aanbrengen in uw Spark-taken om te zien van prestaties bij het gebruik van i/o-Cache. Als i/o-Cache is uitgeschakeld, deze Spark-code zou lezen van gegevens op afstand van Azure Blob Storage: `spark.read.load('wasbs:///myfolder/data.parquet').count()`. Als i/o-Cache is geactiveerd, wordt de dezelfde regel code in de cache lezen via de i/o-Cache. Op de volgende leest de gegevens lokaal gelezen uit SSD. Worker-knooppunten op HDInsight-cluster zijn uitgerust met lokaal aangesloten, toegewezen SSD-stations. HDInsight i/o-Cache gebruikt deze lokale SSD's voor de cache, biedt de laagste niveau van de latentie en maximaliseert de bandbreedte.

## <a name="getting-started"></a>Aan de slag

Azure HDInsight i/o-Cache is standaard ingeschakeld in de Preview-versie gedeactiveerd. I/o-Cache is beschikbaar op Azure HDInsight 3.6 + Spark-clusters, welke Apache Spark 2.3 worden uitgevoerd.  Voor het activeren van i/o-Cache, het volgende doen:

1. Selecteer uw HDInsight-cluster in [de Azure-portal](https://portal.azure.com).

1. In de **overzicht** pagina (standaard geopend wanneer u het cluster selecteert) Selecteer **Ambari Home** onder **Clusterdashboards**.

1. Selecteer de **i/o-Cache** service aan de linkerkant.

1. Selecteer **acties** en **activeren**.

    ![Het inschakelen van de i/o-Cache-service in Ambari](./media/apache-spark-improve-performance-iocache/ambariui-enable-iocache.png "inschakelen van de i/o-Cache-service in Ambari")

1. Controleer of de betrokken services op het cluster opnieuw gestart.

>[!NOTE] 
> Hoewel de voortgangsbalk geactiveerd wordt, wordt niet daadwerkelijk i/o-Cache ingeschakeld totdat u de betrokken services opnieuw starten.

## <a name="troubleshooting"></a>Problemen oplossen
  
U krijgt mogelijk ruimte schijffouten Spark taken uitgevoerd na het inschakelen van i/o-Cache. Deze fouten optreden met Spark lokale schijfopslag wordt ook wordt gebruikt voor het opslaan van gegevens tijdens de volgorde wijzigen van bewerkingen. Spark kan SSD-ruimte te weinig als i/o-Cache is ingeschakeld en de ruimte voor Spark-opslag wordt verminderd. De hoeveelheid ruimte die wordt gebruikt door de i/o-Cache standaard ingesteld op de helft van de totale SSD-ruimte. Het gebruik van schijfruimte voor i/o-Cache kan worden geconfigureerd in Ambari. Als er schijffouten ruimte, de SSD-ruimte die wordt gebruikt voor i/o-Cache te verminderen en de service opnieuw. Als u wilt wijzigen van de ruimte die is ingesteld voor i/o-Cache, voer de volgende stappen uit:

1. Selecteer in de Apache Ambari, de **HDFS** service aan de linkerkant.

1. Selecteer de **Peeringconfiguraties** en **Geavanceerd** tabbladen.

    ![Geavanceerde configuratie van HDFS bewerken](./media/apache-spark-improve-performance-iocache/ambariui-hdfs-service-configs-advanced.png "HDFS geavanceerde configuratie bewerken")

1. Schuif naar beneden en vouw de **aangepaste core-site** gebied.

1. Zoek de eigenschap **hadoop.cache.data.fullness.percentage**.

1. Wijzig de waarde in het vak.

    ![I/o-Cache volheid Percentage bewerken](./media/apache-spark-improve-performance-iocache/ambariui-cache-data-fullness-percentage-property.png "i/o-Cache volheid Percentage bewerken")

1. Selecteer **opslaan** in de rechterbovenhoek.

1. Selecteer **opnieuw** > **opnieuw alle betrokken**.

    ![Opnieuw opstarten alle betrokken](./media/apache-spark-improve-performance-iocache/ambariui-restart-all-affected.png "alle betrokken opnieuw opstarten")

1. Selecteer **opnieuw starten bevestigen alle**.

Als dit niet werkt, schakelt u i/o-Cache.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over i/o-Cache, met inbegrip van prestatiestandaarden in dit blogbericht: [Apache Spark-taken toegang tot 9 x versnellen met HDInsight i/o-Cache](https://azure.microsoft.com/en-us/blog/apache-spark-speedup-with-hdinsight-io-cache/)
