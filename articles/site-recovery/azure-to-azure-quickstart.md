---
title: Herstel na noodgeval instellen voor een Azure IaaS-VM’s naar een secundaire Azure-regio
description: Deze quickstart biedt de stappen die zijn vereist voor herstel na noodgeval van een Azure IaaS-VM tussen Azure-regio’s, met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: f7adcb7d4516e9013bf87306fff77b566885e018
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855279"
---
# <a name="set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Herstel na noodgeval instellen naar een secundaire Azure-regio voor een Azure-VM        

De service [Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

In deze Quick Start wordt beschreven hoe u herstel na noodgevallen instellen voor een virtuele Azure-machine door naar een andere Azure-regio te repliceren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> In dit artikel is als een snel overzicht voor nieuwe gebruikers. Hierbij wordt het eenvoudigste pad, met standaard-opties en minimale aanpassingen.  Voor een uitgebreidere walkthrough beoordeling [onze zelfstudie](azure-to-azure-tutorial-enable-replication.md).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij Azure Portal op https://portal.azure.com.

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
2. U kunt de replicatiestatus, de gemaakte herstelpunten, en de bron- en doelregio's op de kaart controleren.

   ![Replicatiestatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Resources opschonen

De VM in de primaire regio stopt met repliceren wanneer u replicatie uitschakelt:

- De bronreplicatie-instellingen worden automatisch opgeschoond. De extensie voor Site Recovery geïnstalleerd op de virtuele machine als onderdeel van de replicatie wordt niet verwijderd en moet handmatig worden verwijderd. 
- Hiermee stopt u site Recovery-facturering voor de virtuele machine.

Stop de replicatie als volgt

1. Selecteer de VM.
2. Klik in **Herstel na noodgeval** op **Replicatie uitschakelen**.

   ![Replicatie uitschakelen](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u één VM gerepliceerd naar een secundaire regio. Probeer nu een meerdere Azure-VM's met behulp van een herstelplan te repliceren.

> [!div class="nextstepaction"]
> [Herstel na noodgeval voor Azure-VM’s configureren](azure-to-azure-tutorial-enable-replication.md)
