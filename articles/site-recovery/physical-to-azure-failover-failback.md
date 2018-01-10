---
title: Failover en mislukken back fysieke servers die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het fysieke servers naar Azure failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 08/01/2018
ms.author: raynew
ms.openlocfilehash: 2fbad3b53319c8dc8be3480162a4fa67bab94306
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Failover en mislukken back fysieke servers die zijn gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe failover van een fysieke server naar Azure. Nadat u een failover is hebt, schakelt u de server terug naar uw on-premises site wanneer deze beschikbaar is. 

## <a name="preparing-for-failover-and-failback"></a>Failover en failback voorbereiden

Fysieke servers die zijn gerepliceerd naar Azure met Site Recovery kunnen alleen mislukken als virtuele VMware-machines. U moet een VMware-infrastructuur om een failback uit. 

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: failover machines van de lokale site naar Azure.
2. **Beveiligt virtuele Azure-machines**: beveiligt de Azure VM's, zodat deze beginnen te repliceren naar de lokale virtuele VMware-machines.
3. **Failover naar on-premises**: uitvoeren van een failover een failback van Azure.
4. **Beveiligt lokale VMs**: nadat de gegevens terug is mislukt, beveiligt u de lokale virtuele VMware-machines die u niet terug naar, zodat deze beginnen te repliceren naar Azure.

## <a name="verify-server-properties"></a>Controleer de servereigenschappen van

Controleer de servereigenschappen van de en zorg ervoor dat het voldoet aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) voor Azure Virtual machines.

1. In **beveiligde Items**, klikt u op **gerepliceerde Items**, en selecteer de machine.

2. In de **gerepliceerde item** deelvenster er is een overzicht van machine informatie gezondheidsstatus, en de meest recente beschikbare herstelpunten. Klik op **eigenschappen** om meer details te bekijken.
3. In **berekening en netwerk**, kunt u de naam van Azure, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en [beheerde instellingen voor de schijf](#managed-disk-considerations)
4. U kunt weergeven en wijzigen van de netwerkinstellingen, met inbegrip van de netwerk-en het subnet waarin de virtuele machine in Azure worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.
5. In **schijven**, vindt u informatie over het besturingssysteem van de machine en gegevensschijven.

## <a name="run-a-failover-to-azure"></a>Een failover uitvoeren naar Azure

1. In **instellingen** > **gerepliceerde items** Klik op de machine > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover naar. U kunt een van de volgende opties gebruiken:
   - **Meest recente** (standaard): deze optie verwerkt eerst alle gegevens die worden verzonden naar Site Recovery. Het biedt de laagste RPO (beoogd herstelpunt) omdat de virtuele machine in Azure gemaakt nadat de failover is de gegevens die is gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
   - **Meest recente verwerkte**: deze optie is overgenomen van de machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
   - **Meest recente app-consistente**: deze optie is overgenomen van de machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery.
   - **Aangepaste**: Geef een herstelpunt.

3. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor de bronmachine afsluiten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
4. Als u verbinding maken met de Azure VM voorbereid, verbinding maken met na de failover te valideren.
5. Nadat u hebt gecontroleerd, **doorvoeren** de failover. Hiermee verwijdert u de beschikbare herstelpunten.

> [!WARNING]
> Niet een failover uitgevoerd niet annuleren. Voordat de failover is gestart, stopt de replicatie van de machine. Als u de failover annuleert, wordt gestopt, maar de computer opnieuw won't repliceren.
> Voor fysieke servers kan extra failover verwerking ongeveer acht tot tien minuten duren om te voltooien. 

## <a name="create-a-process-server-in-azure"></a>Maken van een processerver in Azure

De processerver ontvangt gegevens van de virtuele machine van Azure en verzendt het naar de lokale site. Een traag netwerk is vereist tussen de processerver en de beveiligde machine.

- Voor testdoeleinden, hebt u een Azure ExpressRoute-verbinding, kunt u de lokale processerver die automatisch wordt geïnstalleerd op de configuratieserver.
- Als u een VPN-verbinding hebt of als u failback in een productieomgeving uitvoert, moet u een Azure VM instellen als een op basis van een Azure-processerver voor failback.
- Volg de instructies in [in dit artikel](site-recovery-vmware-setup-azure-ps-resource-manager.md) voor het instellen van een processerver in Azure.

## <a name="configure-the-master-target-server"></a>De hoofddoelserver configureren

De hoofddoelserver ontvangt standaard failback gegevens. Deze wordt uitgevoerd op de lokale configuratie-server.

- Als de VMware VM waaraan u een failback uit op een ESXi-host die wordt beheerd door de VMware vCenter-Server is, moet de hoofddoelserver toegang tot het gegevensarchief van de virtuele machine (VMDK) gerepliceerde gegevens schrijven naar de VM-schijven hebben. Zorg ervoor dat de VM gegevensopslag is gekoppeld op het hoofddoel host, met lees-/ schrijftoegang.
- Als de ESXi-host die niet wordt beheerd door een vCenter-server, Site Recovery-service een nieuwe virtuele machine tijdens beveiligingspoging wordt gemaakt. De virtuele machine wordt gemaakt op de ESX-host waarop u het hoofddoel VM maakt. De harde schijf van de virtuele machine moet zich in een gegevensopslag die toegankelijk is voor de de host waarop de hoofddoelserver wordt uitgevoerd.
- Voor fysieke machines die u failback, moet u detectie van de host waarop de hoofddoelserver wordt uitgevoerd, voordat u opnieuw met de machine beveiligen kunt uitvoeren.
- Een andere optie wordt als de lokale virtuele machine al voor failback bestaat, deze verwijderen voordat u een failback. Failback maakt vervolgens een nieuwe virtuele machine op dezelfde host als de hoofddoelserver ESX-host. Wanneer u een failover naar een alternatieve locatie, wordt de gegevens naar het hetzelfde gegevensarchief en de dezelfde ESX-host die de on-premises hoofddoelserver hersteld.
- U kunt Storage vMotion niet gebruiken op de hoofddoelserver. Als u dit doet, werkt failback niet, omdat de schijven zijn niet beschikbaar voor deze. Het hoofddoel-servers uit de lijst met vMotion uitsluiten.

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

