---
title: BindException-adres wordt al gebruikt in azure HDInsight
description: BindException-adres wordt al gebruikt in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/08/2019
ms.openlocfilehash: 8851a4dfb7deafab7ad77ef80619dd49ca46ed71
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947849"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Scenario: BindException-adres wordt al gebruikt in azure HDInsight

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen bij het werken met Azure HDInsight-clusters.

## <a name="issue"></a>Probleem

De bewerking voor opnieuw opstarten op een Apache HBase Region-server kan niet worden voltooid. Vanuit de `region-server.log` in `/var/log/hbase` -map op de worker-knoop punten waar het starten van de regio server mislukt, ziet u mogelijk een fout bericht als volgt:

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Oorzaak

De HBase-regio servers worden opnieuw gestart tijdens de zware activiteiten voor de werk belasting. Hieronder ziet u wat er gebeurt achter de schermen wanneer een gebruiker de bewerking voor het opnieuw starten van de HBase van de Ambari-UI initieert:

1. Er wordt door de Ambari-agent een Stop aanvraag verzonden naar de regio server.

1. De Ambari-agent wacht 30 seconden tot de regio server op een correcte manier wordt afgesloten.

1. Als de toepassing verbinding blijft maken met de regio server, wordt deze niet onmiddellijk afgesloten en wordt de time-out van 30 seconden eerder verlopen.

1. Na een periode van 30 seconden stuurt Ambari agent een geforceerde Kill (kill-9) naar de regio server.

1. Als gevolg van deze abrupte afsluiting, hoewel het Server proces van de regio is afgebroken, wordt de poort die is gekoppeld aan het proces mogelijk `AddressBindException`niet vrijgegeven, wat uiteindelijk leidt tot.

## <a name="resolution"></a>Oplossing

Verminder de belasting van de HBase-regio servers voordat u de computer opnieuw opstart.

U kunt regio servers ook hand matig opnieuw starten op de worker-knoop punten met behulp van de volgende opdrachten:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring. Verbinding maken met de Azure-community met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees [hoe u een ondersteunings aanvraag voor Azure kunt maken](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)voor meer informatie. De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
