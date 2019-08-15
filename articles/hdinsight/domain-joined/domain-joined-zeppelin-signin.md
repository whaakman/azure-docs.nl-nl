---
title: Kan niet aanmelden bij Zeppelin in azure HDInsight
description: Kan niet aanmelden bij Zeppelin in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/12/2019
ms.openlocfilehash: 14f4cd7588405dbf351124b1e469fd96f9b38274
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977402"
---
# <a name="scenario-unable-to-sign-in-to-apache-zeppelin-in-azure-hdinsight"></a>Scenario: Kan niet aanmelden bij Apache Zeppelin in azure HDInsight

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen bij het werken met Azure HDInsight-clusters.

## <a name="issue"></a>Probleem

U kunt zich niet aanmelden bij Apache Zeppelin nadat u het wacht woord toevoegen in Active Directory hebt gewijzigd.

## <a name="cause"></a>Oorzaak

De gebruiker die in `activeDirectoryRealm.systemUsername` het `shiro_ini` bestand is vermeld, heeft het Active Directory-wacht woord gewijzigd.

## <a name="resolution"></a>Oplossing

1. Controleer of het gewijzigde wacht woord de hoofd oorzaak is door `activeDirectoryRealm.systemPassword = <new password>` opnemen in de `shiro_ini` Zeppelin-configuratie in Ambari. Verwijder de `activeDirectoryRealm.hadoopSecurityCredentialPath` instelling. Hieronder vindt u de `shiro ini`locatie van.

    ![Shiro](./media/domain-joined-zeppelin-signin/shiro.png)

1. Als gebruikers zich nu na stap 1 kunnen aanmelden bij Zeppelin, maakt u een `jceks` nieuw bestand met het nieuwe wacht woord en `activeDirectoryRealm.hadoopSecurityCredentialPath` vervangt u het door het nieuwe bestand.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring. Verbinding maken met de Azure-community met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees [hoe u een ondersteunings aanvraag voor Azure kunt maken](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)voor meer informatie. De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
