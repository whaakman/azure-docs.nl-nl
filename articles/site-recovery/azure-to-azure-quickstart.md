---
title: Een Azure-VM repliceren naar een andere Azure-regio
description: Deze snelstartgids bevat de stappen die nodig zijn voor het repliceren van een Azure-VM van de ene Azure-regio naar een andere regio.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 07/06/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: de30085ae5ff95e7067684ff3af9ccb0d45357de
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990860"
---
# <a name="replicate-an-azure-vm-to-another-azure-region"></a>Een Azure-VM repliceren naar een andere Azure-regio

De service [Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

In deze snelstartgids wordt het repliceren van een Azure-VM naar een andere Azure-regio beschreven. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="enable-replication-for-the-azure-vm"></a>Replicatie inschakelen voor de Azure-VM

1. Klik in Azure Portal op **Virtuele machines** en selecteer de VM die u wilt repliceren.

2. Klik bij **Bewerkingen** op **Herstel na noodgeval**.
3. Selecteer in **Noodherstel configureren** > **Doelregio** de doelregio waarnaar u wilt repliceren.
4. Accepteer voor deze snelstartgids de overige standaardinstellingen.
5. Klik op **Replicatie inschakelen**. Hierdoor wordt een taak gestart voor het inschakelen van replicatie voor de VM.

    ![replicatie inschakelen](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Instellingen controleren

Nadat de replicatietaak is voltooid, kunt u de replicatiestatus controleren, de replicatie-instellingen wijzigen en de implementatie testen.

1. Klik in het VM-menu op **Herstel na noodgeval**.
2. U kunt de replicatiestatus, gemaakte herstelpunten en bron- en doelregio's op de kaart controleren.

   ![Replicatiestatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Resources opschonen

De VM in de primaire regio stopt met repliceren wanneer u replicatie uitschakelt:

- De bronreplicatie-instellingen worden automatisch opgeschoond.
- Site Recovery-facturering voor de VM wordt ook stopgezet.

Stop de replicatie als volgt:

1. Selecteer de VM.
2. Klik in **Herstel na noodgeval** op **Meer**.
3. Klik op **Replicatie uitschakelen**.

   ![Replicatie uitschakelen](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u één VM gerepliceerd naar een secundaire regio.

> [!div class="nextstepaction"]
> [Herstel na noodgeval voor Azure-VM’s configureren](azure-to-azure-tutorial-enable-replication.md)
