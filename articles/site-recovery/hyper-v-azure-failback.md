---
title: Een failback uitvoeren naar een on-premises-site voor Hyper-v-machines | Microsoft Docs
description: Azure Site Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers. Meer informatie over de failback vanuit Azure naar on-premises datacenter.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: fd171251ef465a28e4844901a529e0a3eaaf8f9d
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920869"
---
# <a name="run-a-failback-for-hyper-v-vms"></a>Een failback uitvoeren voor Hyper-V-machines

Dit artikel wordt beschreven hoe u een failover van de vorige Hyper-V virtuele machines beveiligd door Site Recovery.

## <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat u hebt gelezen, de details over de [verschillende typen failback](concepts-types-of-failback.md) en de bijbehorende beperkingen.
1. Zorg ervoor dat de primaire site VMM-server of Hyper-V-host-server is verbonden met Azure.
2. U moet hebben uitgevoerd **doorvoeren** op de virtuele machine.

## <a name="perform-failback"></a>Failback uitvoeren
Na een failover van de primaire naar secundaire locatie, gerepliceerde virtuele machines niet zijn beveiligd door Site Recovery en de secundaire locatie nu fungeert als de actieve locatie. Mislukken back-VM's in een herstelplan, voer een geplande failover van de secundaire site naar de primaire als volgt uit. 
1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover** > **geplande Failover**.
2. Op de **geplande Failover bevestigen** pagina, kiest u de bron- en doellocaties. Houd er rekening mee de failoverrichting. Als de failover van primaire gewerkt als verwachten en alle virtuele machines zijn op de secundaire locatie als die dit alleen ter informatie is.
3. Als u bent werkt het uitvoeren van Azure selecteert u instellingen in **gegevenssynchronisatie**:
    - **Synchroniseren van gegevens voordat de failover (Synchronize alleen op de deltawijzigingen)**— deze optie wordt de uitvaltijd voor virtuele machines geminimaliseerd omdat het synchroniseert zonder dat deze wordt afgesloten. Het biedt de volgende stappen uit:
        - Fase 1: Momentopname van de virtuele machine in Azure en vervolgens gekopieerd naar de on-premises Hyper-V-host. De machine wordt uitgevoerd in Azure.
        - Fase 2: Sluit de virtuele machine in Azure, zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set delta wijzigingen worden overgebracht naar de on-premises server en de on-premises virtuele machine is gestart.

    - **Synchroniseren van gegevens tijdens de failover alleen (volledige download)**— deze optie is veel sneller.
        - Deze optie is sneller, omdat we verwachten dat de meeste van de schijf is gewijzigd en we niet wilt dat de tijd in de berekening van de controlesom. Een download van de schijf worden uitgevoerd. Het is ook handig wanneer de on-premises virtuele machine is verwijderd.
        - We raden u deze optie als u hebt uitgevoerd Azure een tijdje (per maand of langer) of de on-premises virtuele machine is verwijderd. Deze optie niet controlesom berekeningen uit te voeren.


4. Als gegevensversleuteling is ingeschakeld voor de cloud in **versleutelingssleutel** selecteert u het certificaat dat is uitgegeven wanneer u versleuteling van gegevens tijdens de installatie van de Provider op de VMM-server ingeschakeld.
5. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
6. Als u ervoor hebt gekozen om de gegevens voordat de failover te synchroniseren, zodra de initiële synchronisatie voltooid is en u kunt nu de virtuele machines in Azure afsluiten, klikt u op **taken** geplande failover taaknaam **Failover voltooien**. Dit de Azure-machine wordt afgesloten, verzendt de meest recente wijzigingen aan de on-premises virtuele machine en start de VM on-premises.
7. U kunt nu Meld u aan bij de virtuele machine om te valideren beschikbaar is als verwacht.
8. De virtuele machine is in behandeling staat. Klik op **doorvoeren** om door te voeren van de failover.
9. Nu om te voltooien van de klik failback **omgekeerde repliceren** om te beginnen met het beveiligen van de virtuele machine in de primaire site.


Volg deze procedures een failback uitvoeren naar de oorspronkelijke primaire site. Deze procedure wordt beschreven hoe u een geplande failover voor een herstelplan uitvoeren. U kunt ook de failover voor een enkele virtuele machine uitvoeren op de **virtuele Machines** tabblad.


## <a name="failback-to-an-alternate-location-in-hyper-v-environment"></a>Failback naar een alternatieve locatie in Hyper-V-omgeving
Als u beveiliging tussen hebt geïmplementeerd een [Hyper-V-site en Azure](site-recovery-hyper-v-site-to-azure.md) hebt tot de mogelijkheid voor failback van Azure naar een andere on-premises locatie. Dit is handig als u nodig hebt voor het instellen van nieuwe on-premises hardware. Hier ziet u hoe u dit doet.

1. Als het instellen van nieuwe hardware installeert u Windows Server 2012 R2 en de Hyper-V-rol op de server.
2. Maak een virtuele netwerkswitch met dezelfde naam die u op de oorspronkelijke server hebt.
3. Selecteer **beveiligde Items** -> **beveiligingsgroep**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> u wilt een failback uitvoeren, en selecteer **gepland Failover**.
4. In **geplande Failover bevestigen** Selecteer **maken on-premises virtuele machine indien deze niet bestaat**.
5. In de hostnaam, ** selecteert u de nieuwe Hyper-V-host-server waarop u wilt plaatsen van de virtuele machine.
6. Synchronisatie van gegevens, raden we u de optie **synchroniseert de gegevens voordat de failover**. Dit minimaliseert downtime voor virtuele machines als het synchroniseert zonder dat deze wordt afgesloten. Dit doet het volgende:

    - Fase 1: Momentopname van de virtuele machine in Azure en vervolgens gekopieerd naar de on-premises Hyper-V-host. De machine wordt uitgevoerd in Azure.
    - Fase 2: Sluit de virtuele machine in Azure, zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set wijzigingen worden overgebracht naar de on-premises server en de on-premises virtuele machine is gestart.
    
7. Klik op het vinkje om te beginnen met de failover (failback).
8. Nadat de initiële synchronisatie is voltooid en u klaar bent om de virtuele machine in Azure, klikt u op **taken** > <planned failover job> > **volledige Failover**. Deze machine van Azure wordt afgesloten, verzendt de meest recente wijzigingen aan de on-premises virtuele machine en wordt gestart, krijgt.
9. U kunt zich aanmelden bij de on-premises virtuele machine om te controleren of dat alles werkt zoals verwacht. Klik vervolgens op **doorvoeren** de failover te voltooien. Doorvoeren wordt verwijderd van de virtuele machine van Azure en de schijven en bereidt de virtuele machine moet opnieuw worden beveiligd.
10. Klik op **omgekeerde repliceren** om te beginnen met het beveiligen van de on-premises virtuele machine.

    > [!NOTE]
    > Als u de failbacktaak annuleert terwijl het in de stap voor het synchroniseren van gegevens, worden de on-premises VM beschadigd is. Dit is omdat het synchroniseren van gegevens de meest recente gegevens opgehaald uit Azure VM-schijven op de schijven van de gegevens on-premises en totdat de synchronisatie is voltooid, de schijfgegevens mogelijk niet in een consistente status. Als de On-premises virtuele machine wordt opgestart nadat het synchroniseren van gegevens is geannuleerd, kan deze niet opstarten. Ook opnieuw activeren failover voor het voltooien van de synchronisatie van gegevens.


## <a name="why-is-there-no-button-called-failback"></a>Waarom is er geen knop met de naam van de failback?
Op de portal en er is geen expliciete gebaar van de failback genoemd. Failback is een stap waarin u keert u terug naar de primaire site. Failback is per definitie, wanneer u een failover een back-virtuele machines van herstel naar primaire.

Wanneer u een failover initiëren, de blade u wordt geïnformeerd over de richting waarin de virtuele machines wordt verplaatst, als de richting van Azure naar On-premises, is het een failback.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Waarom is er alleen een geplande failover beweging failback?
Azure is een maximaal beschikbare omgeving en uw virtuele machines altijd beschikbaar zijn. Failback is een geplande activiteit waarop u besluit om een kleine uitvaltijd te zodat de werkbelastingen beginnen met het kunnen on-premises nogmaals uit te voeren. Dit wordt verwacht dat zonder verlies van gegevens. Daarom alleen een gebaar van de geplande failover beschikbaar is, wordt die de virtuele machines in Azure uitschakelen, de recentste wijzigingen gedownload en zorg dat er geen gegevens verloren gaan.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Moet ik een processerver in Azure om een failback uitvoeren naar Hyper-v?
Een processerver is niet het geval is, vereist alleen wanneer u virtuele VMware-machines beveiligt. Er zijn geen extra onderdelen zijn vereist voor bij het beveiligen van/failback van Hyper-v virtuele machines worden geïmplementeerd.


## <a name="time-taken-to-failback"></a>Gebruikte tijd voor failback
De gebruikte tijd voor het voltooien van de synchronisatie van gegevens en start de virtuele machine is afhankelijk van diverse factoren. Om een beter inzicht in de tijd die nodig, uitleggen we wat er gebeurt tijdens het synchroniseren van gegevens.

Synchronisatie van gegevens een momentopname van de schijven van de virtuele machine en wordt gestart door blok controleren en berekent de bijbehorende controlesom. Dit berekende controlesom wordt verzonden naar on-premises moeten worden vergeleken met de on-premises-controlesom van hetzelfde blok. In het geval de controlesommen overeenkomen, wordt het gegevensblok niet overgedragen. Als deze niet overeenkomt, wordt het gegevensblok overgedragen naar on-premises. Deze overdrachtstijd is afhankelijk van de bandbreedte die beschikbaar is. De snelheid van de controlesom is een paar GB's per minuut. 

Als u het downloaden van gegevens sneller, wilt kunt u de MARS-agent voor het gebruik van meer threads voor het downloaden van het parallel configureren. Raadpleeg de [hier](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) over het wijzigen van de download-threads in de agent.


## <a name="next-steps"></a>Volgende stappen

Na **doorvoeren**, kunt u starten de *omgekeerde repliceren*. Hiermee start u de virtuele machine beveiligen van on-premises naar Azure. Dit wordt alleen de wijzigingen gerepliceerd omdat de virtuele machine is uitgeschakeld in Azure en kan daarom alleen differentiële wijzigingen verzendt.
