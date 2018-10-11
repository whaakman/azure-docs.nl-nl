---
title: Een DR-herstelanalyse uitvoeren van Azure-VM’s naar een secundaire Azure-regio met Azure Site Recovery
description: Leer hoe u DR-herstelanalyse kunt uitvoeren van Azure-VM’s naar een secundaire Azure-regio met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 90d82abb6570028308b8d516a868fa981c75eb94
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066986"
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region"></a>Een DR-herstelanalyse uitvoeren van Azure-VM’s naar een secundaire Azure-regio

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps beschikbaar blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

Deze tutorial laat zien hoe u een DR-herstelanalyse kunt uitvoeren voor een Azure-VM, van de ene Azure-regio naar de andere, met een testfailover. Een analyse valideert uw replicatiestrategie zonder gegevensverlies of uitvaltijd, en is niet van invloed op uw productieomgeving. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten controleren
> * Een testfailover uitvoeren voor één VM



## <a name="prerequisites"></a>Vereisten

- Voordat u een failovertest uitvoert, wordt u aangeraden de VM-eigenschappen te controleren of alles is zoals verwacht.  Ga naar de VM-eigenschappen in **Gerepliceerde items**. In de **Essentials**-blade ziet u informatie over de instellingen en status van machines.
- Wij raden u aan om een apart VM-netwerk te gebruiken voor de failover, en niet het standaardnetwerk dat is opgezet toen u replicatie inschakelde.


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Klik in **Instellingen** > **Gerepliceerde items** op het pictogram **+Failover testen** van de VM.

2. Selecteer in **Failover testen** een herstelpunt om voor de failover te gebruiken:

   - **Laatst verwerkte**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM’s naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het virtuele Azure-doelnetwerk waarop Azure VM's in de secundaire regio na de failover worden aangesloten.

4. Klik op **OK** om de failover te starten. Klik op de VM om de eigenschappen ervan te openen als u de voortgang wilt volgen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Verwijder VM's die tijdens de testfailover zijn gemaakt door op het gerepliceerde item of het herstelplan op **Failovertest opschonen** te klikken. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een productie-failover uitvoeren](azure-to-azure-tutorial-failover-failback.md)
