---
title: Apache Spark streaming-taken langer duren dan gebruikelijk voor het verwerken van Azure HDInsight
description: Apache Spark streaming-taken langer duren dan gebruikelijk voor het verwerken van Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 7c7baaaa5fd08a102b3b55c38fb2c4bf892480c5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679430"
---
# <a name="scenario-apache-spark-streaming-jobs-take-longer-than-usual-to-process-in-azure-hdinsight"></a>Scenario: Apache Spark streaming-taken langer duren dan gebruikelijk voor het verwerken van Azure HDInsight

In dit artikel worden probleemoplossings stappen en mogelijke oplossingen voor problemen beschreven bij het gebruik van Apache Spark-onderdelen in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

U ziet dat sommige van de Apache Spark streaming-taken traag zijn of langer duren dan normaal. Voor toepassingen met Spark-streaming komt elke batch berichten overeen met een taak die wordt verzonden naar Spark. Als een taak normaal gesp roken X seconden duurt, duurt het soms 2-3 minuten meer dan normaal.

## <a name="cause"></a>Oorzaak

Een mogelijke oorzaak is dat de Apache Kafka producer meer dan twee minuten nodig heeft om het schrijven naar het Kafka-cluster te volt ooien. Als u het probleem met Kafka verder wilt opsporen, kunt u een logboek registratie toevoegen aan de code die gebruikmaakt van een Kafka producer om berichten te verzenden en correleren met de logboeken van Kafka cluster.

Een andere mogelijke oorzaak is dat veelvuldige Lees-en schrijf bewerkingen naar WASB tot vertraging kunnen leiden. De WASB-implementatie `Filesystem.listStatus` van is erg traag vanwege een `O(n!)` algoritme voor het verwijderen van dubbele items. Er wordt te veel geheugen gebruikt als gevolg van de extra `BlobListItem` conversie `FileMetadata` van `FileStatus`naar naar. Het algoritme neemt bijvoorbeeld meer dan 30 minuten in beslag om 700.000 bestanden weer te geven. Als `ListBlobs` er dus een agressieve SparkSQL wordt aangeroepen door elke micro batch, zullen de volgende micro batches na verloop van vertraging worden veroorzaakt in wat u ondervindt als hoge plannings vertragingen. Met [deze patch](https://issues.apache.org/jira/browse/HADOOP-15547) wordt het probleem opgelost, maar als dit ontbreekt in uw omgeving `ListBlobs` , treedt er een hoge latentie op. Ook als u elk uur bestanden verwijdert, moet de vermelding in de back-end worden herhaald voor alle rijen (inclusief verwijderde), omdat het permanent verzamelen van het proces nog niet is voltooid. Hoewel de patch een deel van het probleem kan oplossen, kan het probleem van de garbagecollection mogelijk nog steeds vertraging veroorzaken bij het verwerken van batches in de stroom.

## <a name="resolution"></a>Oplossing

Pas de [HADOOP-15547-](https://issues.apache.org/jira/browse/HADOOP-15547) oplossing toe. Als dat niet mogelijk is, kunt u HDFS gebruiken als de locatie van het controle punt. Stel `checkpointDirectory` in op iets zoals `hdfs://mycluster/checkpoint`:.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
