---
title: Failover en mislukken back-VMware-machines en fysieke servers die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het VMware-machines en fysieke servers naar Azure failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 28a14a9b28dfe9c2014add9b9f691bce6ba91a4c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Failover en mislukken back-VMware-machines en fysieke servers die zijn gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe een VMware VM naar Azure failover. Nadat u hebt failover, schakelt u terug naar uw on-premises site wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de VMware-VM-eigenschappen om te controleren in overeenstemming met de Azure-vereisten
> * Een failover uitvoeren naar Azure
> * Maak een processerver en de hoofddoelserver voor failback
> * Beveiligt virtuele Azure-machines naar de lokale site
> * Failover van Azure naar on-premises
> * Lokale virtuele machines, om te repliceren naar Azure opnieuw beveiligt

Dit is de vijfde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)
3. [Herstel na noodgeval instellen](tutorial-vmware-to-azure.md)
4. [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Failover en failback voorbereiden

Zorg ervoor dat er zijn geen momentopnamen op de virtuele machine. De lokale virtuele machine is uitgeschakeld tijdens de beveiligingspoging.
Dit helpt ervoor te zorgen dat gegevens tijdens de replicatie. Schakel niet op de virtuele machine nadat beveiligingspoging is voltooid. Gebruik de dezelfde hoofddoelserver aan een replicatiegroep beveiligt. Als u een andere hoofddoelserver aan een replicatiegroep beveiligt, opgeven niet de server een gemeenschappelijk punt in tijd.

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: failover machines van de lokale site naar Azure.
2. **Beveiligt virtuele Azure-machines**: beveiligt de Azure VM's, zodat ze weer worden gerepliceerd naar de lokale VMware-machines starten.
3. **Failover naar on-premises**: uitvoeren van een failover een failback van Azure.
4. **Beveiligt lokale VMs**: nadat de gegevens terug is mislukt, beveiligt u de lokale virtuele machines die u niet terug naar, zodat deze beginnen te repliceren naar Azure.

## <a name="verify-vm-properties"></a>Controleer de eigenschappen van de virtuele machine

Controleer of de VM-eigenschappen en zorg ervoor dat de virtuele machine voldoet aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. In **beveiligde Items**, klikt u op **gerepliceerde Items** > VM.

2. In de **gerepliceerde item** deelvenster er is een overzicht van VM-informatie, gezondheidsstatus, en de meest recente beschikbare herstelpunten. Klik op **eigenschappen** om meer details te bekijken.

3. In **berekening en netwerk**, kunt u de naam van Azure, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en [beheerde instellingen voor de schijf](#managed-disk-considerations)

4. U kunt weergeven en wijzigen van de netwerkinstellingen, met inbegrip van de netwerk-en het subnet waarin de virtuele machine in Azure worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.

5. In **schijven**, vindt u informatie over het besturingssysteem en gegevensschijven op de virtuele machine.

## <a name="run-a-failover-to-azure"></a>Een failover uitvoeren naar Azure

1. In **instellingen** > **gerepliceerde items** klikt u op de virtuele machine > **Failover**.

2. In **Failover** Selecteer een **herstelpunt** failover naar. U kunt een van de volgende opties gebruiken:
   - **Meest recente** (standaard): deze optie verwerkt eerst alle gegevens die worden verzonden naar Site Recovery. Het biedt de laagste RPO (beoogd herstelpunt) omdat de virtuele machine in Azure gemaakt nadat de failover is de gegevens die is gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
   - **Meest recente verwerkte**: deze optie is overgenomen van de virtuele machine naar het recentste herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
   - **Meest recente app-consistente**: deze optie is overgenomen van de virtuele machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery.
   - **Aangepaste**: Geef een herstelpunt.

3. Selecteer **machine afsluiten voordat u begint met failover** probeert te doen virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.

4. Als u verbinding maken met de Azure VM voorbereid, verbinding maken met na de failover te valideren.

5. Nadat u hebt gecontroleerd, **doorvoeren** de failover. Hiermee verwijdert u de beschikbare herstelpunten.

> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: voordat failover wordt gestart, VM-replicatie is gestopt.
> Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.

In sommige scenario's moet failover extra verwerking die het duurt ongeveer 8 tot tien minuten is voltooid. Ziet u mogelijk meer failovertijden voor de fysieke servers, VMware Linux-machines, virtuele VMware-machines waarvoor geen DHCP-service waarmee en VMware-machines die niet de volgende opstartstuurprogramma's testen: storvsc, vmbus, storflt, intelide, atapi.

## <a name="create-a-process-server-in-azure"></a>Maken van een processerver in Azure

De processerver ontvangt gegevens van de virtuele machine van Azure en verzendt het naar de lokale site. Een traag netwerk is vereist tussen de processerver en de beveiligde virtuele machine.

- Voor testdoeleinden, hebt u een Azure ExpressRoute-verbinding, kunt u de lokale processerver die automatisch wordt geïnstalleerd op de configuratieserver.
- Als u een VPN-verbinding hebt of als u failback in een productieomgeving uitvoert, moet u een Azure VM instellen als een op basis van een Azure-processerver voor failback.
- Als u een processerver in Azure instelt, volg de instructies in [in dit artikel](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>De hoofddoelserver configureren

De hoofddoelserver wordt standaard uitgevoerd op de lokale configuratie-server. We gebruiken de standaard-master voor het doel van deze zelfstudie. De hoofddoelserver ontvangt gegevens van failback.

Als de virtuele machine zich op een ESXi-host die wordt beheerd door een vCenter-server, moet de hoofddoelserver toegang tot het gegevensarchief van de virtuele machine (VMDK) gerepliceerde gegevens schrijven naar de VM-schijven hebben. Zorg ervoor dat de VM gegevensopslag is gekoppeld op het hoofddoel host, met lees-/ schrijftoegang.

Als de virtuele machine zich op een ESXi die niet wordt beheerd door een vCenter-server, maakt Site Recovery-service een nieuwe virtuele machine tijdens de beveiligingspoging. De virtuele machine wordt gemaakt op de ESX-host waarop u het hoofddoel maakt.
De harde schijf van de virtuele machine moet zich in een gegevensopslag die toegankelijk is voor de de host waarop de hoofddoelserver wordt uitgevoerd.

Als de virtuele machine geen vCenter gebruikt, moet u detectie van de host waarop de hoofddoelserver wordt uitgevoerd, voordat u opnieuw met de machine beveiligen kunt uitvoeren. Dit geldt voor terug fysieke servers te mislukken. Een andere optie wordt als de lokale virtuele machine bestaat, deze verwijderen voordat u een failback. Failback maakt vervolgens een nieuwe virtuele machine op dezelfde host als de hoofddoelserver ESX-host. Wanneer u een failover naar een alternatieve locatie, wordt de gegevens naar het hetzelfde gegevensarchief en de dezelfde ESX-host die de on-premises hoofddoelserver hersteld.

U kunt Storage vMotion niet gebruiken op de hoofddoelserver. Als u dit doet, werkt failback niet, omdat de schijven zijn niet beschikbaar voor deze. Het hoofddoel-servers uit de lijst met vMotion uitsluiten.

## <a name="reprotect-azure-vms"></a>Beveiligt virtuele machines in Azure

Deze procedure wordt ervan uitgegaan dat de lokale virtuele machine is niet beschikbaar en u opnieuw te op een andere locatie beveiligen bent.

1. In **instellingen** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die is mislukt via > **opnieuw beveiligen**.
2. In **opnieuw beveiligen**, Controleer **Azure met On-premises**, is geselecteerd.
3. Geef de on-premises hoofddoelserver en de processerver.

4. In **Datastore**, selecteert u het hoofddoel gegevensarchief waarnaar u herstellen van de schijven wilt on-premises. Gebruik deze optie wanneer de lokale virtuele machine is verwijderd en moet u nieuwe schijven te maken. Deze instelling wordt genegeerd als de schijven is al aanwezig, maar u hoeft een waarde op te geven.
5. Selecteer het bewaarstation van het hoofddoel. Het failbackbeleid voor wordt automatisch geselecteerd.
6. Klik op **OK** om te beginnen met de beveiligingspoging. Een taak begint met het repliceren van de virtuele machine van Azure naar de lokale site. U kunt de voortgang volgen op de **taken** tabblad.

> [!NOTE]
> Als u wilt herstellen de Azure VM aan een bestaand lokaal VM, gegevensopslag van de lokale virtuele machine met lees-/ schrijftoegang koppelen op de hoofddoelserver van ESXi-host.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Een failover uitvoeren van Azure met on-premises

Als u wilt repliceren naar on-premises, wordt een failbackbeleid gebruikt. Dit beleid wordt automatisch gemaakt wanneer u een beleid voor wachtwoordreplicatie voor replicatie naar Azure hebt gemaakt:

- Het beleid is automatisch gekoppeld aan de configuratieserver.
- Het beleid kan niet worden gewijzigd.
- De beleidswaarden zijn:
    - RPO drempelwaarde = 15 minuten
    - Herstel bewaarperiode = 24 uur
    - App-consistente momentopname frequentie = 60 minuten

De failover als volgt uitvoeren:

1. Op de **gerepliceerde Items** pagina, met de rechtermuisknop op de machine > **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting van de van Azure.

3. Selecteer het herstelpunt dat u wilt gebruiken voor de failover. Een app-consistente herstelpunt vindt plaats voordat het meest recente punt in tijd en hierdoor kunnen gegevens verloren gaan. Wanneer failover wordt uitgevoerd, Site Recovery de Azure VM's wordt afgesloten en wordt de lokale virtuele machine wordt opgestart. Er zijn enige uitvaltijd, dus kiest u een bepaalde tijd.
4. Met de rechtermuisknop op de machine en klik op **doorvoeren**. Dit activeert een taak die door de Azure VM's zijn verwijderd.
5. Controleer of u hebt Azure VM's zijn afgesloten zoals verwacht.


## <a name="reprotect-on-premises-machines-to-azure"></a>Lokale-machines in Azure beveiligt

Gegevens worden nu terug op de site van uw on-premises, maar deze wordt niet gerepliceerd naar Azure. U kunt beginnen met het repliceren naar Azure opnieuw als volgt:

1. In de kluis > **instellingen** > **gerepliceerde Items**, selecteert u de mislukte vorige virtuele machines die zijn mislukt terug en klik op **opnieuw beveiligen**.
2. Selecteer de processerver die wordt gebruikt voor de gerepliceerde gegevens verzenden naar Azure, en klik op **OK**.

Nadat de beveiligingspoging is voltooid, de virtuele machine repliceert naar Azure en u kunt een failover uitvoeren zoals vereist.
