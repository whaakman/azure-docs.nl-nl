---
title: Configureren van replicatie voor virtuele Azure-machines in Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u replicatie voor Azure-VM's van een Azure-regio naar de andere met behulp van Site Recovery configureren.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: asgang
ms.openlocfilehash: 19a1e13815f1d83f13392892dd9c87b84a40a1c1
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551694"
---
# <a name="replicate-azure-vms-to-another-azure-region"></a>Virtuele Azure-machines repliceren naar een andere Azure-regio


In dit artikel wordt beschreven hoe u de replicatie van Azure-VM's van een Azure-regio naar een andere inschakelt.

## <a name="before-you-start"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt voorbereid voor implementatie van Site Recovery, zoals beschreven in de [zelfstudie voor herstel na noodgevallen van Azure naar Azure](azure-to-azure-tutorial-enable-replication.md).

Vereisten moeten worden voldaan, en u een Recovery Services-kluis moet hebt gemaakt.


## <a name="enable-replication"></a>Replicatie inschakelen

Replicatie inschakelen. Deze procedure wordt ervan uitgegaan dat de primaire Azure-regio Oost-Azië is en de secundaire regio Zuidoost-Azië.

1. Klik in de kluis op **+ repliceren**.
2. Houd rekening met de volgende velden:
   - **Bron**: Het punt van herkomst van de virtuele machines, die in dit geval **Azure**.
   - **Bronlocatie**: De Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de Oost-Azië
   - **Implementatiemodel**: Azure-implementatiemodel van de bronmachines.
   - **Abonnement van bron**: Het abonnement waartoe de bron-VM's behoren. Dit kan elk abonnement zijn binnen dezelfde Azure Active Directory-tenant waar uw Recovery Services-kluis zich bevindt.
   - **Resourcegroep**: De resourcegroep waartoe de virtuele bronmachines behoren. Alle virtuele machines onder de geselecteerde resourcegroep worden weergegeven voor beveiliging in de volgende stap.

     ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. In **virtuele Machines > virtuele machines selecteren**op en selecteer elke VM die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.
    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. In **instellingen**, kunt u eventueel target-site-instellingen configureren:

   - **Doellocatie**: De locatie waar de brongegevens van de virtuele machine worden gerepliceerd. Afhankelijk van de locatie van uw geselecteerde machines, biedt Site Recovery u de lijst met geschikte doelregio's. U wordt aangeraden dat u de doellocatie hetzelfde als de locatie voor de Recovery Services-kluis.
   - **Doelabonnement**: Het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Standaard is het doelabonnement niet hetzelfde als het bronabonnement.
   - **Doelresourcegroep**: De resourcegroep waarin alle uw gerepliceerde virtuele machines behoren.
       - Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met het achtervoegsel 'asr' in de naam.
       - Als de resourcegroep die is gemaakt door Site Recovery al bestaat, wordt dit opnieuw gebruikt.
       - U kunt de instellingen voor de resource.
       - De locatie van de doelresourcegroep is een Azure-regio, met uitzondering van de regio waarin de bron-VM's worden gehost.
   - **Virtueel doelnetwerk**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio met het achtervoegsel 'asr' in de naam. Dit is toegewezen aan de bron-netwerk en voor eventuele toekomstige beveiliging gebruikt. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
   - **Doelopslagaccounts (bron-VM geen beheerde schijven gebruiken)** : Site Recovery maakt standaard een nieuw doel-opslagaccount mimicking de opslagconfiguratie van de bron-VM. In het geval er bestaat al een storage-account, wordt dit opnieuw gebruikt.
   - **Replica-beheerde schijven (bron-VM maakt gebruik van beheerde schijven)** : Site Recovery maakt een nieuwe replica-beheerde schijven in de doelregio voor het spiegelen van beheerde schijven van de bron-VM met hetzelfde opslagtype (Standard of premium) als de bron-VM van schijf beheerde.
   - **Storage-accounts in de cache**: Site Recovery moet extra opslagaccount met de naam van cacheopslag in de bronregio. Alle wijzigingen die plaatsvinden op de bron-VM's worden bijgehouden en verzonden naar de cache-opslagaccount voor het repliceren van die naar de doellocatie. Dit opslagaccount moet standaard.
   - **Doelbeschikbaarheidssets**: Site Recovery maakt standaard een nieuwe beschikbaarheidsset in de doelregio met het achtervoegsel 'asr' in de naam, voor virtuele machines die deel van een beschikbaarheidsset in de bronregio uitmaken. Als de beschikbaarheidsset gemaakt door Site Recovery al bestaat, wordt dit opnieuw gebruikt.
   - **Doelbeschikbaarheidszones**: In Site Recovery wordt standaard hetzelfde zonenummer toegewezen als de bronregio in de doelregio, als de doelregio ondersteuning biedt voor beschikbaarheidszones.

     Als de doelregio geen ondersteuning biedt voor beschikbaarheidszones, worden de doel-VM's standaard geconfigureerd als enkele exemplaren. Indien vereist, kunt u dergelijke VM's configureren als onderdeel van beschikbaarheidssets in de doelregio door op Aanpassen te klikken.

     >[!NOTE]
     >Nadat u replicatie hebt ingeschakeld, kunt u het type beschikbaarheid - enkel exemplaar, beschikbaarheidsset of beschikbaarheidszone - niet meer wijzigen. Als u het type beschikbaarheid wilt wijzigen, moet u replicatie uitschakelen en opnieuw inschakelen.
     >
    
   - **Beleid voor wachtwoordreplicatie**: Hiermee worden de instellingen voor recovery point bewaren geschiedenis en de app de momentopnamefrequentie gedefinieerd. Standaard Azure Site Recovery maakt een nieuw replicatiebeleid met de standaardinstellingen van 24 uur voor de bewaarperiode voor herstelpunten en frequentie van 4 uur voor app-consistente momentopname.

     ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

### <a name="enable-replication-for-added-disks"></a>Replicatie inschakelen voor toegevoegde schijven

Als u schijven aan een virtuele Azure-machine waarop replicatie is ingeschakeld toevoegt, gebeurt het volgende:
-   Een waarschuwing weergegeven, ziet u de replicatiestatus voor de virtuele machine en een opmerking wordt geïnformeerd aangeeft dat een of meer schijven zijn beschikbaar voor beveiliging.
-   Als u beveiliging voor de toegevoegde schijven inschakelt, wordt de waarschuwing verdwijnt na de initiële replicatie van de schijf.
-   Als u ervoor kiest geen replicatie in te schakelen voor de schijf, kunt u de waarschuwing negeren.

    
    ![Nieuwe schijf toegevoegd](./media/azure-to-azure-how-to-enable-replication/newdisk.png)

U schakelt replicatie voor een extra schijf, het volgende doen:

1.  In de kluis > **gerepliceerde Items**, klikt u op de virtuele machine waaraan u de schijf toegevoegd.
2.  Klik op **schijven**, en selecteer vervolgens de gegevensschijf waarvan u wilt replicatie in te schakelen (deze schijven hebben een **niet beveiligd** status).
3.  In **Schijfdetails**, klikt u op **inschakelen replicatie**.

    ![Replicatie inschakelen voor toegevoegde schijf](./media/azure-to-azure-how-to-enable-replication/enabled-added.png)

Nadat de inschakelen replicatie taak wordt uitgevoerd, en de initiële replicatie is voltooid, wordt de replicatie: waarschuwing voor het probleem schijf verwijderd.


  
## <a name="customize-target-resources"></a>Doelresources aanpassen

U kunt de standaardinstellingen van de doel-gebruikt door Site Recovery kunt wijzigen.

1. Klik op **aanpassen:** naast 'Doelabonnement' om te wijzigen van het doelabonnement standaard. Selecteer het abonnement uit de lijst met alle abonnementen die beschikbaar zijn in dezelfde Azure Active Directory (AAD)-tenant.

2. Klik op **aanpassen:** standaardinstellingen wijzigen:
    - In **doelresourcegroep**, selecteer de resourcegroep in de lijst met alle resourcegroepen in de doellocatie van het abonnement.
    - In **virtueel doelnetwerk**, het netwerk selecteren uit een lijst van het virtuele netwerk op de doellocatie.
    - In **beschikbaarheidsset**, u beschikbaarheid instellen op de virtuele machine, kunt toevoegen als ze deel uitmaken van een beschikbaarheidsset in de bronregio.
    - In **Doelopslag accounts**, selecteert u het account dat u wilt gebruiken.

        ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
3. Klik op **aanpassen:** replicatie-instellingen wijzigen.
4. In **Multi-VM-consistentie**, selecteert u de virtuele machines die u wilt repliceren samen.
    - Alle machines in een replicatiegroep hebben gedeelde crash-consistente en app-consistente herstelpunten bij een failover.
    - Inschakelen van multi-VM-consistentie kan beïnvloeden werkbelasting (zoals het CPU-intensieve is). Dit moet alleen worden ingeschakeld als machines dezelfde werkbelasting worden uitgevoerd, en u consistentie tussen meerdere computers.
    - Bijvoorbeeld, als een toepassing 2 virtuele machines met SQL Server en twee webservers heeft, moet klikt u vervolgens u toevoegen alleen de SQL Server-VM's aan een replicatiegroep.
    - U kunt maximaal 16 virtuele machines in een replicatiegroep hebben.
    - Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004.
    - Zorg dat er geen firewall-apparaat wordt van de interne communicatie tussen de virtuele machines via poort 20004.
    - Als u wilt dat Linux VM's deel uitmaken van een replicatiegroep, zorg ervoor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen voor de specifieke Linux-versie.
![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
5. Klik op **maken doelresource** > **inschakelen replicatie**.
6. Nadat de virtuele machines zijn ingeschakeld voor replicatie, kunt u de status van de status van de virtuele machine onder controleren **gerepliceerde items**

>[!NOTE]
>Tijdens de eerste replicatie kan de status even om te vernieuwen, zonder dat wordt uitgevoerd. Klik op de **vernieuwen** knop, de meest recente status ophalen.
>

# <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
