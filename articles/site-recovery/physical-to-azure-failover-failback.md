---
title: Failover en mislukken back fysieke servers die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het fysieke servers naar Azure failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/22/2018
ms.author: raynew
ms.openlocfilehash: bbad2a0ea1a58834eaf32e0d3286f6e8a794d364
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Failover en mislukken back fysieke servers die zijn gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe failover van een fysieke server naar Azure. Nadat u een failover is hebt, schakelt u de server terug naar uw on-premises site wanneer deze beschikbaar is. 

## <a name="preparing-for-failover-and-failback"></a>Voorbereiden voor failover en failback

Fysieke servers die zijn gerepliceerd naar Azure met Site Recovery kunnen alleen mislukken als virtuele VMware-machines. U moet een VMware-infrastructuur om een failback uit. 

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: failover-overschakeling uitvoeren van computers van de on-premises site naar Azure.
2. **Beveiligt virtuele Azure-machines**: beveiligt de Azure VM's, zodat deze beginnen te repliceren naar de lokale virtuele VMware-machines.
3. **Failover naar on-premises**: voer een failover-overschakeling uit om een failback uit te voeren van Azure.
4. **Beveiligt lokale VMs**: nadat de gegevens terug is mislukt, beveiligt u de lokale virtuele VMware-machines die u niet terug naar, zodat deze beginnen te repliceren naar Azure.

## <a name="verify-server-properties"></a>Controleer de servereigenschappen van

Controleer de servereigenschappen van de en zorg ervoor dat het voldoet aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) voor Azure Virtual machines.

1. In **beveiligde Items**, klikt u op **gerepliceerde Items**, en selecteer de machine.

2. In de **gerepliceerde item** deelvenster er is een overzicht van machine informatie gezondheidsstatus, en de meest recente beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en [beheerde-schijfinstellingen](#managed-disk-considerations) wijzigen
4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.
5. In **schijven**, vindt u informatie over het besturingssysteem van de machine en gegevensschijven.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. In **instellingen** > **gerepliceerde items** Klik op de machine > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   - **Laatste** (standaard): met deze optie worden eerst alle gegevens naar Site Recovery verzonden gegevens verwerkt. Dit biedt het laagste RPO (Recovery Point Objective), omdat de na de failover gemaakte Azure-VM alle gegevens heeft die naar Site Recovery is gerepliceerd toen de failover werd geactiveerd.
   - **Meest recente verwerkte**: deze optie is overgenomen van de machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
   - **Meest recente app-consistente**: deze optie is overgenomen van de machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery.
   - **Aangepast**: geef een herstelpunt op.

3. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor de bronmachine afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Als u de verbinding met de Azure VM hebt voorbereid, maakt u na de failover verbinding om deze te valideren.
5. Na het verifiëren kunt u de failover **Doorvoeren**. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> Niet een failover uitgevoerd niet annuleren. Voordat de failover is gestart, stopt de replicatie van de machine. Als u de failover annuleert, wordt gestopt, maar de computer opnieuw won't repliceren.
> Voor fysieke servers kan extra failover verwerking ongeveer acht tot tien minuten duren om te voltooien. 

## <a name="create-a-process-server-in-azure"></a>Een processerver maken in Azure

De processerver ontvangt gegevens van de Azure VM en verzendt deze naar de on-premises site. Een traag netwerk is vereist tussen de processerver en de beveiligde machine.

- Als u een Azure ExpressRoute-verbinding hebt, kunt u voor testdoeleinden de on-premises processerver gebruiken die automatisch wordt geïnstalleerd op de configuratieserver.
- Als u een VPN-verbinding hebt of als u failback in een productieomgeving uitvoert, moet u een Azure VM instellen als een Azure-processerver voor failback.
- Volg de instructies in [in dit artikel](site-recovery-vmware-setup-azure-ps-resource-manager.md) voor het instellen van een processerver in Azure.

## <a name="configure-the-master-target-server"></a>De hoofddoelserver configureren

De hoofddoelserver ontvangt standaard failback gegevens. Deze wordt uitgevoerd op de lokale configuratie-server.

- Als de VMware VM waaraan u een failback uit op een ESXi-host die wordt beheerd door de VMware vCenter-Server is, moet de hoofddoelserver toegang tot het gegevensarchief van de virtuele machine (VMDK) gerepliceerde gegevens schrijven naar de VM-schijven hebben. Zorg ervoor dat de VM-gegevensopslag is gekoppeld op de host van het hoofddoel, met lees-/schrijftoegang.
- Als de ESXi-host die niet wordt beheerd door een vCenter-server, Site Recovery-service een nieuwe virtuele machine tijdens beveiligingspoging wordt gemaakt. De virtuele machine wordt gemaakt op de ESX-host waarop u het hoofddoel VM maakt. De harde schijf van de VM moet zich in een gegevensopslag bevinden die toegankelijk is voor de host waarop de hoofddoelserver draait.
- Voor fysieke machines die u failback, moet u detectie van de host waarop de hoofddoelserver wordt uitgevoerd, voordat u opnieuw met de machine beveiligen kunt uitvoeren.
- Een andere optie wordt als de lokale virtuele machine al voor failback bestaat, deze verwijderen voordat u een failback. Failback maakt vervolgens een nieuwe VM op dezelfde host als de ESX-host van de hoofddoelserver. Wanneer u een failback uitvoert naar een alternatieve locatie, worden de gegevens hersteld naar dezelfde gegevensopslag en dezelfde ESX-host als die welke gebruikt wordt door de on-premises hoofddoelserver.
- U kunt Storage vMotion niet gebruiken op de hoofddoelserver. Als u dat doet, werkt failback niet, omdat de schijven er niet beschikbaar voor zijn. Sluit de hoofddoelservers uit van uw vMotion-lijst.

## <a name="reprotect-azure-vms"></a>Azure VM’s opnieuw beveiligen

Bij deze procedure wordt ervan uitgegaan dat de on-premises VM niet beschikbaar is en dat u opnieuw beschermt naar een alternatieve locatie.

1. Klik in **Instellingen** > **Gerepliceerde items** met de rechtermuisknop op de VM waarvoor een failover-overschakeling is uitgevoerd > **Opnieuw beveiligen**.
2. Controleer in **Opnieuw beveiligen** of **Azure naar on-premises** is geselecteerd.
3. Geef de on-premises hoofddoelserver en de processerver op.

4. Selecteer in **Gegevensopslag** de gegevensopslag van het hoofddoel waarnaar u de on-premises schijven wilt herstellen. Gebruik deze optie wanneer de on-premises VM is verwijderd en u nieuwe schijven moet maken. Deze instellingen worden genegeerd als de schijven al bestaan, maar u moet wel een waarde opgeven.
5. Selecteer het bewaarstation van het hoofddoel. Het failback-beleid wordt automatisch geselecteerd.
6. Klik op **OK** om te beginnen met opnieuw beveiligen. Er wordt een taak gestart voor het repliceren van de VM van Azure naar de on-premises site. U kunt de voortgang volgen op het tabblad **Taken**.

> [!NOTE]
> Als u de Azure-VM wilt herstellen naar een bestaande on-premises VM, koppelt u de gegevensopslag van de virtuele machine met lees-/schrijf-toegang aan de ESXi-host van de hoofddoelserver.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Een failover van Azure naar on-premises uitvoeren

Voor opnieuw repliceren naar on-premises wordt een failbackbeleid gebruikt. Dit beleid wordt automatisch gemaakt wanneer u een replicatiebeleid voor replicatie naar Azure hebt gemaakt:

- Het beleid wordt automatisch gekoppeld aan de configuratieserver.
- Het beleid kan niet worden gewijzigd.
- De beleidswaarden zijn:
    - RPO-drempelwaarde = 15 minuten
    - Bewaarperiode van het herstelpunt = 24 uur
    - Frequentie van de app-consistente momentopname = 60 minuten

Voer de failover als volgt uit:

1. Klik op de pagina **Gerepliceerde items** met de rechtermuisknop op de computer > **Niet-geplande failover**.
2. Controleer in **Failover bevestigen** of de failoverrichting van Azure is.

3. Selecteer het herstelpunt dat u wilt gebruiken voor de failover. Een app-consistent herstelpunt treedt op voor het meest recente tijdstip, en dit veroorzaakt enig gegevensverlies. Wanneer failover wordt uitgevoerd, schakelt Site Recovery de Azure VM's uit en start de on-premises VM op. Er zal wat downtime zijn, dus kies een geschikte tijd.
4. Klik met de rechtermuisknop op de computer en klik op **Doorvoeren**. Dit activeert een taak waardoor de Azure VM's worden verwijderd.
5. Controleer of de Azure VM's zijn afgesloten zoals verwacht.


## <a name="reprotect-on-premises-machines-to-azure"></a>On-premises computers opnieuw beveiligen naar Azure

De gegevens moeten nu weer terug zijn op uw on-premises site, maar ze worden niet gerepliceerd naar Azure. U kunt als volgt weer beginnen met repliceren naar Azure:

1. Selecteer in de kluis > **Instellingen** >**Gerepliceerde items** de failback-VM's waarvoor een failback is uitgevoerd en klik op **Opnieuw beveiligen**.
2. Selecteer de processerver die wordt gebruikt om de gerepliceerde gegevens naar Azure te verzenden en klik op **OK**.

Nadat het opnieuw beveiligen is voltooid, repliceert de VM weer naar Azure en kunt u zo nodig een failover uitvoeren.

