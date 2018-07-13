---
title: Failover- en failback-overschakeling uitvoeren van met Site Recovery naar Azure gerepliceerde VMware-VM’s en fysieke servers | Microsoft Docs
description: Leer een failover-overschakeling uitvoeren van VMware-VM’s en fysieke servers naar Azure, en een failback naar de on-premises site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 06/13/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 57be12e3d203ba88cf7900598a09abdd206e1495
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37115072"
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Failover- en failback-overschakeling uitvoeren van naar Azure gerepliceerde VMware-VM’s en fysieke servers

Deze zelfstudie beschrijft het uitvoeren van een failover-overschakeling van een VMware-VM naar Azure. Nadat u een failover hebt uitgevoerd, valt u terug op uw on-premises site wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de VMware-VM-eigenschappen in overeenstemming zijn met de Azure-vereisten
> * Een failover naar Azure uitvoeren
> * Een processerver en hoofddoelserver maken voor failback
> * Azure-VM’s opnieuw beveiligen naar de on-premises site
> * Failover-overschakeling uitvoeren van Azure naar on-premises
> * On-premises VM’s opnieuw beveiligen om weer te beginnen met replicatie naar Azure

Dit is de vijfde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](vmware-azure-tutorial-prepare-on-premises.md)
3. [Herstel na noodgeval instellen](vmware-azure-tutorial.md)
4. [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Voorbereiden voor failover en failback

Zorg ervoor dat er geen momentopnamen zijn op de VM. De on-premises VM wordt uitgeschakeld tijdens het opnieuw beveiligen.
Dit helpt ervoor te zorgen dat gegevens tijdens de replicatie consistent blijven. Schakel de VM niet in nadat het opnieuw beveiligen is voltooid. Gebruik de dezelfde hoofddoelserver om een replicatiegroep opnieuw te beveiligen. Als u een andere hoofddoelserver gebruikt om een replicatiegroep opnieuw te beveiligen, kan de server geen gemeenschappelijk tijdstip bieden.

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: failover-overschakeling uitvoeren van computers van de on-premises site naar Azure.
2. **Azure-VM’s opnieuw beveiligen**: beveilig de Azure VM's opnieuw, zodat ze weer worden gerepliceerd naar de on-premises VMware-VM’s.
3. **Failover naar on-premises**: voer een failover-overschakeling uit om een failback uit te voeren van Azure.
4. **On-premises VM’s opnieuw beveiligen**: nadat een failback van gegevens is uitgevoerd, beschermt u de on-premises VM’s waarop u bent teruggevallen, zodat ze gaan repliceren naar Azure.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Verifieer de VM-eigenschappen en zorg ervoor dat de VM voldoet aan de [Azure-vereisten](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Klik in **Beveiligde items** op **Gerepliceerde items** > VM.

2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.

3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en [beheerde-schijfinstellingen](#managed-disk-considerations) wijzigen

4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.

5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.

2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   - **Laatste** (standaard): met deze optie worden eerst alle gegevens naar Site Recovery verzonden gegevens verwerkt. Dit biedt het laagste RPO (Recovery Point Objective), omdat de na de failover gemaakte Azure-VM alle gegevens heeft die naar Site Recovery is gerepliceerd toen de failover werd geactiveerd.
   - **Laatst verwerkte punt**: met deze optie wordt een failover-overschakeling van de VM uitgevoerd naar het laatste door Site Recovery verwerkte herstelpunt. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover-overschakeling uitgevoerd van de VM naar het laatste door Site Recovery verwerkte toepassingsconsistente herstelpunt.
   - **Aangepast**: geef een herstelpunt op.

3. Selecteer **Sluit de computer af voordat de failover wordt gestart** om te proberen virtuele bronmachines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.

4. Als u de verbinding met de Azure VM hebt voorbereid, maakt u na de failover verbinding om deze te valideren.

5. Na het verifiëren kunt u de failover **Doorvoeren**. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt.
> Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor fysieke servers, VMware Linux-computers, VMware VM's waarop de DHCP-service niet is ingeschakeld, en VMware VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="create-a-process-server-in-azure"></a>Een processerver maken in Azure

De processerver ontvangt gegevens van de Azure VM en verzendt deze naar de on-premises site. Er is een netwerk met lage latentie vereist tussen de processerver en de beveiligde VM.

- Als u een Azure ExpressRoute-verbinding hebt, kunt u voor testdoeleinden de on-premises processerver gebruiken die automatisch wordt geïnstalleerd op de configuratieserver.
- Als u een VPN-verbinding hebt of als u failback in een productieomgeving uitvoert, moet u een Azure VM instellen als een Azure-processerver voor failback.
- Volg de instructies in [dit artikel](vmware-azure-set-up-process-server-azure.md) om een processerver in te stellen in Azure.

## <a name="configure-the-master-target-server"></a>De hoofddoelserver configureren

De master target-server wordt standaard uitgevoegd op de on-premises configuratieserver. In deze zelfstudie gebruiken we de standaardserver. De hoofddoelserver ontvangt failbackgegevens.

Als de VM op een ESXi host staat die door een vCenter-server wordt beheerd, moet de hoofddoelserver toegang hebben tot de gegevensopslag van de VM (VMDK) om gerepliceerde gegevens naar de VM-schijven te schrijven. Zorg ervoor dat de VM-gegevensopslag is gekoppeld op de host van het hoofddoel, met lees-/schrijftoegang.

Als de VM op een ESXi staat die niet door een vCenter-server wordt beheerd, creëert maakt Recovery een nieuwe VM tijdens het opnieuw beveiligen. De VM wordt gemaakt op de ESX-host waarop u het hoofddoel maakt.
De harde schijf van de VM moet zich in een gegevensopslag bevinden die toegankelijk is voor de host waarop de hoofddoelserver draait.

Als de VM geen gebruik maakt van vCenter, moet u de detectie van de host waarop de hoofddoelserver draait voltooien voordat u de computer opnieuw kunt beveiligen. Dit geldt ook voor failback van fysieke servers. Een andere optie, als de on-premises VM bestaat, is om deze te verwijderen voordat u een failback uitvoert. Failback maakt vervolgens een nieuwe VM op dezelfde host als de ESX-host van de hoofddoelserver. Wanneer u een failback uitvoert naar een alternatieve locatie, worden de gegevens hersteld naar dezelfde gegevensopslag en dezelfde ESX-host als die welke gebruikt wordt door de on-premises hoofddoelserver.

U kunt Storage vMotion niet gebruiken op de hoofddoelserver. Als u dat doet, werkt failback niet, omdat de schijven er niet beschikbaar voor zijn. Sluit de hoofddoelservers uit van uw vMotion-lijst.

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
