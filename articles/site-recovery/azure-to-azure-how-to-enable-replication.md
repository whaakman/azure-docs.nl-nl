---
title: Configureren van replicatie voor virtuele Azure-machines in Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u replicatie voor Azure-VM's van een Azure-regio naar de andere met behulp van Site Recovery configureren.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: c8b5cf840b8cb5eec2a993cfe35c8a8a7ac904fb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58079772"
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Virtuele Azure-machines repliceren naar een andere Azure-regio



In dit artikel wordt beschreven hoe u de replicatie van Azure-VM's van een Azure-regio naar een andere inschakelt.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt al ingesteld Site Recovery voor dit scenario, zoals beschreven in de [Azure naar Azure-zelfstudie](azure-to-azure-tutorial-enable-replication.md). Zorg ervoor dat u hebt voorbereid, de vereisten en de Recovery Services-kluis gemaakt.



## <a name="enable-replication"></a>Replicatie inschakelen

Replicatie inschakelen. Deze procedure wordt ervan uitgegaan dat de primaire Azure-regio Oost-Azië is en de secundaire regio Zuidoost-Azië.

1. Klik in de kluis op **+ repliceren**.
2. Houd rekening met de volgende velden:
   - **Bron**: Het punt van herkomst van de virtuele machines, die in dit geval **Azure**.
   - **Bronlocatie**: De Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de Oost-Azië
   - **Implementatiemodel**: Azure-implementatiemodel van de bronmachines.
   - **Abonnement van bron**: Het abonnement waartoe de virtuele bronmachines behoren. Dit kan elk abonnement zijn binnen dezelfde Azure Active Directory-tenant waar uw Recovery Services-kluis zich bevindt.
   - **Resourcegroep**: De resourcegroep waartoe de virtuele bronmachines behoren. Alle virtuele machines onder de geselecteerde resourcegroep worden weergegeven voor beveiliging in de volgende stap.

     ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. In **virtuele Machines > virtuele machines selecteren**op en selecteer elke VM die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.
    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. In **instellingen**, kunt u eventueel target-site-instellingen configureren:

   - **Doellocatie**: De locatie waar de brongegevens van de virtuele machine worden gerepliceerd. Afhankelijk van de locatie van uw geselecteerde machines, biedt Site Recovery u de lijst met geschikte doelregio's. U wordt aangeraden dat u de doellocatie hetzelfde als de locatie voor de Recovery Services-kluis.
   - **Doelabonnement**: Het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Standaard is het doelabonnement niet hetzelfde als het bronabonnement.
   - **Doelresourcegroep**: De resourcegroep waarin alle uw gerepliceerde virtuele machines behoren. Azure Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met de naam met achtervoegsel 'asr'. In het geval resourcegroep hebt gemaakt met Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt. U kunt ook aanpassen zoals wordt weergegeven in de onderstaande sectie. De locatie van de doelresourcegroep kan zijn van andere Azure-regio's met uitzondering van de regio waarin de virtuele bronmachines worden gehost.
   - **Target Virtual Network**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio met de naam met achtervoegsel 'asr'. Dit is toegewezen aan de bron-netwerk en voor eventuele toekomstige beveiliging gebruikt. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
   - **Doelopslagaccounts (als de bron-VM maakt geen gebruik van schijven beheerde)**: Site Recovery maakt standaard een nieuw doel-opslagaccount mimicking de opslagconfiguratie van de bron-VM. In het geval er bestaat al een storage-account, wordt dit opnieuw gebruikt.
   - **Beheerde replicaschijven (als de bron-VM gebruikmaakt van beheerde schijven)**: Site Recovery maakt nieuwe beheerde replicaschijven in de doelregio voor het spiegelen van beheerde schijven van de bron-VM met hetzelfde opslagtype (Standard of premium) als de bron-VM van schijf beheerde.
   - **Storage-accounts in de cache**: Site Recovery moet extra opslagaccount met de naam van cacheopslag in de bronregio. Alle wijzigingen die plaatsvinden op de bron-VM's worden bijgehouden en verzonden naar de cache-opslagaccount voor het repliceren van die naar de doellocatie.
   - **Doelbeschikbaarheidssets**: In Azure Site Recovery wordt standaard een nieuwe beschikbaarheidsset gemaakt in de doelregio, met een naam die eindigt op 'asr' voor het VM-gedeelte van een beschikbaarheidsset in de bronregio. Als de beschikbaarheidsset die wordt gemaakt met Azure Site Recovery, al bestaat, wordt deze opnieuw gebruikt.
   - **Doelbeschikbaarheidszones**: In Site Recovery wordt standaard hetzelfde zonenummer toegewezen als de bronregio in de doelregio, als de doelregio ondersteuning biedt voor beschikbaarheidszones.

     Als de doelregio geen ondersteuning biedt voor beschikbaarheidszones, worden de doel-VM's standaard geconfigureerd als enkele exemplaren. Indien vereist, kunt u dergelijke VM's configureren als onderdeel van beschikbaarheidssets in de doelregio door op Aanpassen te klikken.

     >[!NOTE]
     >Nadat u replicatie hebt ingeschakeld, kunt u het type beschikbaarheid - enkel exemplaar, beschikbaarheidsset of beschikbaarheidszone - niet meer wijzigen. Als u het type beschikbaarheid wilt wijzigen, moet u replicatie uitschakelen en opnieuw inschakelen.
     >
    
   - **Beleid voor wachtwoordreplicatie**: Hiermee worden de instellingen voor recovery point bewaren geschiedenis en de app de momentopnamefrequentie gedefinieerd. Azure Site Recovery maakt standaard een nieuw replicatiebeleid met de standaardinstellingen van 24 uur voor de bewaarperiode voor herstelpunten en ' 60 minuten voor de frequentie voor app-consistente momentopname te maken.

     ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
  
## <a name="customize-target-resources"></a>Doelresources aanpassen

U kunt de standaardinstellingen van de doel-gebruikt door Site Recovery kunt wijzigen.

1. Klik op **aanpassen:** naast 'Doelabonnement' om te wijzigen van het doelabonnement standaard. Selecteer het abonnement uit de lijst met alle abonnementen die beschikbaar zijn in dezelfde Azure Active Directory (AAD)-tenant.

2. Klik op **aanpassen:** standaardinstellingen wijzigen:
    - In **doelresourcegroep**, selecteer de resourcegroep in de lijst met alle resourcegroepen in de doellocatie van het abonnement.
    - In **virtueel doelnetwerk**, het netwerk selecteren uit een lijst van het virtuele netwerk op de doellocatie.
    - In **beschikbaarheidsset**, u beschikbaarheid instellen op de virtuele machine, kunt toevoegen als ze deel uitmaken van een beschikbaarheidsset in de bronregio.
    - In **Doelopslag accounts**, selecteert u het account dat u wilt gebruiken.

        ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
1. Klik op **aanpassen:** replicatie-instellingen wijzigen.
   - In **Multi-VM-consistentie**, selecteert u de virtuele machines die u wilt repliceren samen 
   - Alle machines in een replicatiegroep hebben gedeelde crash-consistente en app-consistente herstelpunten bij een failover. Het inschakelen van multi-VM-consistentie is CPU-intensief en kan de prestaties van de werklast beïnvloeden. Deze functie moet alleen worden gebruikt als de computers dezelfde werklast hebben en u consistentie tussen meerdere computers nodig hebt. Bijvoorbeeld, als een toepassing 2 virtuele machines van de sql- en 2 webservers heeft vervolgens u moet alleen sql virtuele machines toevoegen als onderdeel van de replicatiegroep.
   - Een replicatiegroep kan maximaal 16 virtuele machines bevatten.
   - Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd. Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.
![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
2. Klik op **maken doelresource** > **inschakelen replicatie**.
3. Nadat de virtuele machines zijn ingeschakeld voor replicatie, kunt u de status van de status van de virtuele machine onder controleren **gerepliceerde items**

>[!NOTE]
>Tijdens de eerste replicatie kan de status even om te vernieuwen, zonder dat wordt uitgevoerd. Klik op de **vernieuwen** knop, de meest recente status ophalen.
>

# <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
