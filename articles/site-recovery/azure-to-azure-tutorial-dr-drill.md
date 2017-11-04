---
title: Een herstel na noodgevallen detailanalyse uitvoeren voor virtuele Azure-machines naar een secundair Azure-regio met Azure Site Recovery (Preview)
description: Informatie over het uitvoeren van een herstel na noodgevallen detailanalyse voor virtuele Azure-machines naar een secundair Azure-regio met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5bcd3d64714951508d984c17326e845ae4842670
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Een herstel na noodgevallen detailanalyse uitvoeren voor virtuele Azure-machines naar een secundair Azure-regio (Preview)

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door uw business-apps te houden beschikbaar tijdens geplande en ongeplande uitval. Site Recovery beheert en noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failovers en herstel ingedeeld.

Deze zelfstudie laat zien hoe u een herstel na noodgevallen detailanalyse voor een virtuele machine van Azure vanaf één Azure-regio naar de andere met een testfailover uitvoeren. Een detailanalyse valideert van uw replicatiestrategie voor zonder verlies van gegevens of uitvaltijd en niet van invloed op uw productieomgeving. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleer de vereisten
> * Een testfailover uitvoeren voor een enkele VM

## <a name="prerequisites"></a>Vereisten

- Voordat u een failovertest uitvoert, wordt u aangeraden de VM-eigenschappen om te controleren of alles naar verwachting te controleren.  Toegang tot de VM-eigenschappen in **gerepliceerde items**. De **Essentials** blade ziet u informatie over machines instellingen en status.
- Het is raadzaam om dat u een afzonderlijk Azure VM-netwerk gebruiken voor de testfailover en niet de standaardnetwerk dat is ingesteld wanneer u replicatie is ingeschakeld.


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. In **instellingen** > **gerepliceerde Items**, klikt u op de virtuele machine **+ Testfailover** pictogram.

2. In **Testfailover**, selecteer een herstelpunt voor de failover gebruiken:

   - **Meest recente verwerkte**: de virtuele machine wordt overgenomen door de meest recente herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie geen tijd besteed aan het verwerken van gegevens, zodat het biedt een laag RTO (beoogde hersteltijd)
   - **Meest recente app-consistente**: deze optie alle VM's naar de meest recente herstelpunt van app-consistente failover. Het tijdstempel wordt weergegeven.
   - **Aangepaste**: eender welk herstelpunt kiezen.

3. Selecteer de doel-Azure virtueel netwerk voor welke Azure VM's in de secundaire regio moet worden verbonden nadat de failover plaatsvindt.

4. Voor het starten van de failover, klikt u op **OK**. Om de voortgang volgen, klikt u op de virtuele machine om de eigenschappen te openen. U kunt ook op de **Testfailover** taak in de kluisnaam > **instellingen** > **taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, de replica virtuele machine in Azure wordt weergegeven in de Azure portal > **virtuele Machines**. Zorg ervoor dat de virtuele machine actief is, de juiste grootte en verbonden met het juiste netwerk.
6. Als u wilt verwijderen van de virtuele machines die zijn gemaakt tijdens de testfailover, klikt u op **opschonen testfailover** voor het gerepliceerde artikel of het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een productie-failover uitvoert](azure-to-azure-tutorial-failover-failback.md)
