---
title: Failback in de Azure Site Recovery voor Hyper-v virtuele machines | Microsoft Docs
description: "Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines en fysieke servers. Meer informatie over failback vanuit Azure naar on-premises datacentrum."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: fafaf3f55f07741d438a06e58713d57d465b1137
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>Failback in Site Recovery voor Hyper-V virtuele machines

Dit artikel wordt beschreven hoe u failback van virtuele machines die beveiligd door Site Recovery.

## <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat de primaire VMM-server/Hyper-V-server is verbonden.
2. U moet hebben uitgevoerd **doorvoeren** op de virtuele machine.

## <a name="why-is-there-no-button-called-failback"></a>Waarom is er geen knop failback aangeroepen?
In de portal is er geen expliciete gebaar van de failback wordt aangeroepen. Failback is een stap waar u keert u terug naar de primaire site. Failover is wanneer u failover van de virtuele machines van primary(on-premises) Recovery (Azure site) en failback is wanneer u failover de virtuele machines van herstel terug naar de primaire per definitie.

Wanneer u een failover initieert, wordt u door de blade informeert over de richting van de taak. Als de richting van Azure met On-premises, is een failback.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Waarom ligt er alleen een geplande failover gebaar failback?
Azure is een maximaal beschikbare omgeving en uw virtuele machines altijd beschikbaar zijn. Failback is een geplande activiteit waar u besluiten tot een kleine uitvaltijd zodat de werkbelastingen kunnen starten lokale nogmaals uit te voeren. Dit verwacht geen gegevens verloren gaan. Daarom alleen een gebaar van de geplande failover beschikbaar is, wordt die de virtuele machines in Azure uitschakelen, download de meest recente wijzigingen en zorg dat er geen gegevens verloren gaan.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Moet ik een processerver in Azure voor failback naar Hyper-v?
Nee, processerver alleen is vereist wanneer u virtuele VMware-machines beveiligt. Er zijn geen extra onderdelen zijn vereist wanneer beveiligen/failback van Hyper-v virtuele machines worden geïmplementeerd.

## <a name="initiate-failback"></a>Failback initiëren
Na een failover van de primaire naar secundaire locatie gerepliceerde virtuele machines niet worden beveiligd door Site Recovery en de secundaire locatie nu fungeert als de actieve locatie. Volg deze procedures mislukken terug naar de oorspronkelijke primaire site. Deze procedure wordt beschreven hoe u een geplande failover voor een herstelplan uitvoeren. U kunt ook de failover voor een enkele virtuele machine uitvoeren op de **virtuele Machines** tabblad.

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover** > **geplande Failover**.
2. Op de ** geplande Failover bevestigen ** pagina, kiest u de bron- en doellocaties. Noteer de failoverrichting van de. Als de failover van primaire gewerkt als verwacht en alle virtuele machines zich in de secundaire locatie als die dit alleen ter informatie is.
3. Als u failback vanuit Azure mislukken bent instellingen in selecteren **gegevenssynchronisatie**:

   * **Synchroniseren van gegevens voordat failover wordt uitgevoerd (alleen synchroniseren deltawijzigingen)**— deze optie uitvaltijd voor virtuele machines wordt geminimaliseerd omdat het synchroniseert zonder dat deze wordt afgesloten. Dit biedt de volgende stappen uit:
     * Fase 1: Maakt momentopname van de virtuele machine in Azure en vervolgens gekopieerd naar de lokale Hyper-V-host. De machine blijft worden uitgevoerd in Azure.
     * Fase 2: De virtuele machine in Azure afgesloten zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set van delta-wijzigingen worden overgebracht naar de on-premises server en de on-premises virtuele machine is gestart.

    - **Synchroniseren van gegevens tijdens de failover alleen (volledige download)**: Gebruik deze optie als u gedurende een lange periode hebt is uitgevoerd op Azure. Deze optie is sneller, omdat we verwachten dat de meeste van de schijf is gewijzigd en we willen geen tijd in de berekening van de controlesom besteden aan. Een download van de schijf worden uitgevoerd. Het is ook handig wanneer de on-premises virtuele machine is verwijderd.

    >[!NOTE]
    >Het is raadzaam het gebruik van deze optie als u hebt al actief Azure een tijdje (een maand of langer) of de on-premises virtuele machine is verwijderd. Deze optie niet controlesom berekeningen uit te voeren.


4. Als gegevensversleuteling is ingeschakeld voor de cloud in **versleutelingssleutel** selecteert u het certificaat dat was uitgegeven wanneer u de versleuteling van gegevens tijdens de installatie van de Provider op de VMM-server ingeschakeld.
5. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
6. Als u de optie voor het synchroniseren van de gegevens voordat de failover hebt geselecteerd, als de initiële synchronisatie voltooid is en u klaar bent om de virtuele machines in Azure afsluiten, klikt u op **taken** geplande failover taaknaam **Failover voltooien**. De Azure-machine is afgesloten, de verzendt de meest recente wijzigingen aan de on-premises virtuele machine en de VM on-premises wordt gestart.
7. U kunt nu zich aanmeldt bij de virtuele machine te valideren beschikbaar is zoals verwacht.
8. De virtuele machine is in behandeling status. Klik op **doorvoeren** doorvoeren van de failover.
9. Nu om te kunnen voltooien van de failback Klik **omgekeerde replicatie** om te beginnen met het beveiligen van de virtuele machine in de primaire site.

## <a name="failback-to-an-alternate-location"></a>Failback naar een alternatieve locatie
Als u de beveiliging tussen hebt geïmplementeerd een [Hyper-V-site en Azure](site-recovery-hyper-v-site-to-azure.md) hebt tot de mogelijkheid om een failback van Azure te een alternatieve on-premises locatie. Dit is handig als u voor het instellen van een nieuwe lokale hardware nodig. Hier ziet u hoe u dit doet.

1. Als u nieuwe hardware instelt Windows Server 2012 R2 en de Hyper-V-rol op de server installeren.
2. Maak een virtuele netwerkswitch met dezelfde naam die u op de oorspronkelijke server had.
3. Selecteer **beveiligde Items** -> **beveiligingsgroep**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> u wilt een failback uit en selecteer **gepland Failover**.
4. In **geplande Failover bevestigen** Selecteer **maken lokale virtuele machine als deze niet bestaat**.
5. In de hostnaam, ** selecteert u de nieuwe Hyper-V-hostserver waarop u wilt plaats de virtuele machine.
6. In de synchronisatie van gegevens, raden wij u de optie **synchroniseert de gegevens voordat de failover**. Hierdoor minimaliseert uitvaltijd voor virtuele machines als het synchroniseert zonder dat deze wordt afgesloten. Dit doet het volgende:

   * Fase 1: Maakt momentopname van de virtuele machine in Azure en vervolgens gekopieerd naar de lokale Hyper-V-host. De machine blijft worden uitgevoerd in Azure.
   * Fase 2: De virtuele machine in Azure afgesloten zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set wijzigingen worden overgebracht naar de on-premises server en de on-premises virtuele machine is opgestart.
7. Klik op het vinkje om te beginnen met de failover (failback).
8. Nadat de initiële synchronisatie is voltooid en u klaar bent om de virtuele machine in Azure afsluiten, klikt u op **taken** > <planned failover job> > **volledige Failover**. De Azure-machine is afgesloten, de verzendt de meest recente wijzigingen aan de on-premises virtuele machine en deze gestart.
9. U kunt zich aanmelden bij de on-premises virtuele machine om te controleren of dat alles werkt zoals verwacht. Klik vervolgens op **doorvoeren** de failover voltooien.
10. Klik op **omgekeerde replicatie** om te beginnen met het beveiligen van de on-premises virtuele machine.

    > [!NOTE]
    > Als u de failbacktaak annuleert terwijl het gegevenssynchronisatie stap, wordt de lokale virtuele machine worden beschadigd is. Dit is omdat de synchronisatie van de meest recente gegevens opgehaald uit Azure VM-schijven aan de on-premises gegevensschijven en totdat de synchronisatie is voltooid, de schijfgegevens mogelijk niet in een consistente status. Als de On-premises virtuele machine wordt opgestart nadat de synchronisatie van gegevens is geannuleerd, kan deze niet opgestart. Opnieuw activeren failover voor het voltooien van de synchronisatie van gegevens.

## <a name="time-taken-to-failback"></a>Tijd van failback
De gegevenssynchronisatie voltooid en start de virtuele machine is de tijd is afhankelijk van verschillende factoren. Als u een beter inzicht in de tijd geven, kunnen worden uitgelegd wat er gebeurt tijdens het synchroniseren van gegevens.

Synchronisatie van gegevens een momentopname van de virtuele machine-schijven en controleert bloksgewijze en berekent de controlesom. Dit berekende controlesom wordt verzonden naar on-premises moeten worden vergeleken met de lokale controlesom van hetzelfde blok. Als de controlesommen overeenkomen, wordt het gegevensblok niet overgedragen. Als deze niet overeenkomt, wordt het gegevensblok overgedragen met on-premises. Deze overdrachtstijd is afhankelijk van de beschikbare bandbreedte. De snelheid van de controlesom, is enkele GB's per minuut. 

Als u wilt versnellen het downloaden van gegevens, kunt u uw MARS-agent voor het gebruik van meer threads voor het downloaden van de parallelize configureren. Raadpleeg de [document hier](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) over het wijzigen van de download-threads in de agent.


## <a name="next-steps"></a>Volgende stappen

Als u klaar bent met de failbacktaak **doorvoeren** de virtuele machine. Commit Hiermee verwijdert u de virtuele machine van Azure en de schijven en voorbereiden van de virtuele machine moet opnieuw worden beveiligd.

Na **doorvoeren**, kunt u initiëren de *omgekeerde replicatie*. Hiermee start u de virtuele machine beveiligt on-premises naar Azure. Dit wordt alleen de wijzigingen gerepliceerd omdat de virtuele machine in Azure is uitgeschakeld en daarom alleen differentiële wijzigingen verzendt.
