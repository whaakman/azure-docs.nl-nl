---
title: Een Azure-VM repliceren naar een andere Azure-regio (preview)
description: Deze snelstartgids bevat de stappen die nodig zijn voor het repliceren van een Azure-VM van de ene Azure-regio naar een andere regio.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 02/07/2018
ms.author: raynew
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 213b218a50506ace2298aa3f77721db1b81e2efe
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="replicate-an-azure-vm-to-another-azure-region-preview"></a>Een Azure-VM repliceren naar een andere Azure-regio (preview)

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps beschikbaar blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

In deze snelstartgids wordt het repliceren van een Azure-VM naar een andere Azure-regio beschreven.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="enable-replication-for-the-azure-vm"></a>Replicatie inschakelen voor de Azure-VM

1. Klik in Azure Portal op **Virtuele machines** en selecteer de VM die u wilt repliceren.

2. Klik in **Instellingen** op **Herstel na noodgeval (preview)**.
3. Selecteer in **Noodherstel configureren** > **Doelregio** de doelregio waarnaar u wilt repliceren.
4. Accepteer voor deze snelstartgids de overige standaardinstellingen.
5. Klik op **Replicatie inschakelen**. Hierdoor wordt een taak gestart voor het inschakelen van replicatie voor de VM.

    ![replicatie inschakelen](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Instellingen controleren

Nadat de replicatietaak is voltooid, kunt u de replicatiestatus controleren, replicatie-instellingen wijzigen en de implementatie testen.

1. Klik in het VM-menu op **Herstel na noodgeval (preview)**.
2. U kunt de replicatiestatus, gemaakte herstelpunten en bron- en doelregio's op de kaart controleren.

   ![Replicatiestatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Resources opschonen

De VM in de primaire regio stopt met repliceren wanneer u replicatie uitschakelt:

- De bronreplicatie-instellingen worden automatisch opgeschoond.
- Site Recovery-facturering voor de VM wordt ook stopgezet.

Stop de replicatie als volgt:

1. Selecteer de VM.
2. Klik in **Herstel na noodgeval (preview)**op **Meer**.
3. Klik op **Replicatie uitschakelen**.

   ![Replicatie uitschakelen](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u één VM gerepliceerd naar een secundaire regio.

> [!div class="nextstepaction"]
> [Herstel na noodgeval voor Azure-VM’s configureren](azure-to-azure-tutorial-enable-replication.md)
