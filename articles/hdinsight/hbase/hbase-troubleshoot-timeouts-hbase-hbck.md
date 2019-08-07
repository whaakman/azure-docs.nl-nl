---
title: Time-outs met de opdracht ' hbase hbck ' in azure HDInsight
description: Er is een time-outprobleem opgetreden met de opdracht ' hbase hbck ' bij het herstellen van regio toewijzingen
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 4f3f1c22fa1dc05a66a8b6bf0179903a44cef9b6
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737925"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Scenario: Time-outs met de opdracht ' hbase hbck ' in azure HDInsight

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen bij het werken met Azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Er worden time- `hbase hbck` outs gevonden met de opdracht bij het herstellen van regio toewijzingen.

## <a name="cause"></a>Oorzaak

De mogelijke oorzaak kan zijn dat er voor een lange periode meerdere regio's onder de status ' in transitie ' zijn. Deze regio's kunnen worden gezien als offline vanuit Apache HBase Master-gebruikers interface. Als gevolg van een groot aantal regio's dat probeert over te gaan, kan HBase Master een time-out krijgen en kunnen deze regio's niet weer online worden gezet.

## <a name="resolution"></a>Oplossing

1. Meld u aan bij HDInsight HBase-cluster met behulp van SSH.

1. Voer `hbase zkcli` de opdracht uit om verbinding te maken met de Zookeeper-shell.

1. Uitvoeren `rmr /hbase/regions-in-transition` of`rmr /hbase-unsecure/regions-in-transition` opdracht.

1. Sluit de `hbase zkcli` shell af met `exit` behulp van de opdracht.

1. Open de Ambari-gebruikers interface en start de service Active HBase Master opnieuw vanaf Ambari.

1. Bewaak de HBase Master gebruikers interface ' regio in overgang ' die sectie om ervoor te zorgen dat er geen regio achterblijft.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
