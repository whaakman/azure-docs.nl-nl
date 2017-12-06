---
title: Hyper-V-Capaciteitsplanner voor Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de Capaciteitsplanner voor Hyper-V uitvoert voor Azure Site Recovery
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: e2a69f240068d3155c2fdd52c118dc037ccbcdcb
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
Nieuwe uitgebreide versie van [Azure Site Recovery implementatie planner voor Hyper-V naar Azure](site-recovery-hyper-v-deployment-planner.md) is nu beschikbaar en vervang oude hulpprogramma. Het nieuwe hulpprogramma gebruiken voor het plannen van uw implementatie. Het hulpprogramma geeft volgende richtlijn: 
* VM in aanmerking komt, op basis van beoordeling aantal schijven, schijfgrootte IOP's, verloop en enkele VM-kenmerken.
* Netwerkbandbreedte moet versus RPO assessment.
* Vereisten voor Azure-infrastructuur.
* On-premises infrastructuurvereisten.
* Initiële replicatie batchverwerking richtlijnen.
* Geschatte totaalkosten DR naar Azure.


# <a name="hyper-v-capacity-planner-tool-for-site-recovery"></a>Hyper-V-Capaciteitsplanner voor Site Recovery

Als onderdeel van uw Azure Site Recovery-implementatie moet u de replicatie en de vereiste bandbreedte te achterhalen. De Capaciteitsplanner voor Hyper-V voor Site Recovery kunt u hiervoor voor Hyper-V-replicatie voor virtuele machines.

Dit artikel wordt beschreven hoe u de Capaciteitsplanner voor Hyper-V uitvoert. Dit hulpprogramma moet worden gebruikt in combinatie met de informatie in [capaciteitsplanning voor siteherstel](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Voordat u begint
U kunt het hulpprogramma uitvoeren op een Hyper-V server of cluster-knooppunt in de primaire site. Voer het hulpprogramma de Hyper-V-hostservers moet:

* Besturingssysteem: WindowsServer 2012 of 2012 R2
* Geheugen: 20 MB (minimum)
* CPU: 5 procent overhead (minimum)
* Schijfruimte: 5 MB (minimum)

Voordat u het hulpprogramma uitvoert, moet u de primaire site voorbereiden. Als u repliceert tussen twee on-premises sites en u wilt controleren, bandbreedte, moet u een replica-server voorbereiden.

## <a name="step-1-prepare-the-primary-site"></a>Stap 1: Bereid de primaire site

1. Controleer op de primaire site een lijst met alle van de Hyper-V virtuele machines die u wilt repliceren en de Hyper-V-hosts/clusters waarop ze zich bevinden. Het hulpprogramma kan worden uitgevoerd voor meerdere zelfstandige hosts of voor een cluster, maar niet allebei tegelijk. Het moet ook afzonderlijk uitvoeren voor elk besturingssysteem, dus u gegevens over Hyper-V-servers als volgt verzamelen moet:

   * Windows Server 2012 zelfstandige servers
   * Windows Server 2012-clusters
   * Zelfstandige servers voor Windows Server 2012 R2
   * Windows Server 2012 R2-clusters
2. Inschakelen van externe toegang tot WMI op de Hyper-V-hosts en clusters. Deze opdracht uitvoeren op elke server/cluster om ervoor te zorgen dat firewall-regels en de gebruikersmachtigingen zijn ingesteld:

        netsh firewall set service RemoteAdmin enable
3. Schakel de bewaking van toepassingsprestaties op servers en clusters, als volgt:

   * Open de Windows Firewall met de **geavanceerde beveiliging** module en klikt u vervolgens schakelt u het volgende binnenkomende regels: **COM +-netwerktoegang (DCOM-IN)** en alle regels in de **externe gebeurtenislogboek Beheergroep**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Stap 2: Een replica-server (on-premises naar lokale replicatie) voorbereiden
U hoeft niet te doen als u naar Azure repliceert.

Wij raden aan dat u één Hyper-V-host instellen als een herstelserver, zodat een dummy-virtuele machine kan worden gerepliceerd naar deze om te controleren van de bandbreedte.  U kunt dit overslaan, maar het niet mogelijk bandbreedte meten, tenzij u dit doen.

1. Als u gebruiken van een clusterknooppunt wilt als de replica Hyper-V Replica broker configureren:

   * In **Serverbeheer**Open **Failoverclusterbeheer**.
   * Verbinding maken met het cluster, markeer de naam van het cluster en klikt u op **acties** > **functie configureren** om de wizard hoge beschikbaarheid te openen.
   * In **rol selecteren**, klikt u op **Hyper-V Replica Broker**. Geef in de wizard een **NetBIOS-naam** en de **IP-adres** moet worden gebruikt als het verbindingspunt met het cluster (een clienttoegangspunt genoemd). De **Hyper-V Replica Broker** wordt geconfigureerd, wat resulteert in een client access point-naam die u Houd er rekening mee.
   * Controleer of de functie Hyper-V Replica Broker online beschikbaar is, en tussen alle knooppunten van het cluster een failover. Om dit te doen, klik met de rechtermuisknop op de rol, wijs **verplaatsen**, en klik vervolgens op **knooppunt selecteren**. Selecteer een knooppunt > **OK**.
   * Als u verificatie op basis van certificaten gebruikt, controleert u of elk clusterknooppunt en de client toegangspunt dat alle certificaat geïnstalleerd hebben.
2. Een replica-server inschakelen:

   * Voor een cluster fout Cluster Manager openen, verbinding met het cluster en klikt u op **rollen** > Functieservices selecteren > **replicatie-instellingen** > **Maak van dit cluster als een Replica Server**. Als u een cluster als de replica gebruikt, moet u de Hyper-V Replica Broker-rol aanwezig zijn op het cluster in de primaire site ook.
   * Open Hyper-V-beheer voor een zelfstandige server. In de **acties** deelvenster, klikt u op **Hyper-V-instellingen** voor de server die u wilt inschakelen, en in **replicatieconfiguratie** klikt u op **deze computer inschakelen als een Replica-server**.
3. Verificatie instellen:

   * In **verificatie en poorten**, selecteer de manier waarop om de primaire server en de verificatie te verifiëren poorten. Als u een certificaat Klik **certificaat selecteren** om een te selecteren. Kerberos niet gebruiken als de primaire en herstelsites Hyper-V-hosts zich in hetzelfde domein of in vertrouwde domeinen. Gebruik van certificaten voor verschillende domeinen bevinden, of voor de implementatie van een werkgroep.
   * In **autorisatie en opslag**, toestaan **eventuele** geverifieerde (primaire) servers om replicatiegegevens te verzenden naar deze replicaserver.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Voer **netsh http show servicestate**, om te controleren of de listener wordt uitgevoerd voor de opgegeven protocol/poort:  
4. Firewalls instellen. Tijdens de installatie van Hyper-V, worden de firewallregels voor verkeer op de standaard-poorten (HTTPS op 443, Kerberos op 80) gemaakt. Deze regels als volgt inschakelen:
  - Certificaatverificatie op cluster (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Kerberos-verificatie op cluster (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Certificaatverificatie op zelfstandige server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Kerberos-verificatie op de zelfstandige server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planner-tool"></a>Stap 3: Voer het hulpprogramma capacity planner
Nadat u uw primaire site hebt voorbereid, en een herstelserver hebt ingesteld, kunt u het hulpprogramma uitvoeren.

1. [Download](https://www.microsoft.com/download/details.aspx?id=39057) het hulpprogramma uit het Microsoft Download Center.
2. Het hulpprogramma uitvoeren vanaf een van de primaire servers (of een van de knooppunten van het primaire cluster). Met de rechtermuisknop op het .exe-bestand en kies vervolgens **als administrator uitvoeren**.
3. In **voordat u begint met**, opgeven hoe lang u gegevens wilt verzamelen. U wordt aangeraden dat het hulpprogramma wordt uitgevoerd tijdens de productie-uren om ervoor te zorgen dat er gegevens vertegenwoordiger. Als u alleen netwerkconnectiviteit controleren probeert, kunt u alleen een minuut verzamelen.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. In **details van de primaire site**, de servernaam of de FQDN-naam opgeven voor een zelfstandige host of voor een cluster, geef de FQDN-naam van de client punt accepteren, cluster-naam of een willekeurig knooppunt in het cluster en klik vervolgens op **volgende**. Het hulpprogramma detecteert automatisch de naam van de server die wordt uitgevoerd. Het hulpprogramma neemt over virtuele machines die kunnen worden gecontroleerd voor de gespecificeerde servers.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. In **Replica sitegegevens**, als u naar Azure repliceert of als u naar een secundair datacenter repliceert en een replica-server hebt ingesteld selecteert **overslaan tests met betrekking tot de replica-site**. Als u naar een secundair datacenter repliceert en u een ReplicaType hebt ingesteld, voert u de FQDN van de zelfstandige server of de client access point voor het cluster in **Server name (of) Hyper-V Replica Broker CAP**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. In **uitgebreide Replica Details**, inschakelen **overslaan van de tests met betrekking tot de site van de uitgebreide Replica**. Deze tests worden niet ondersteund door Site Recovery.
7. In **kiezen virtuele machines repliceren**, de hulpprogramma's maakt verbinding met de server of het cluster en virtuele machines worden weergegeven en schijven op de primaire server, volgens de instellingen u hebt opgegeven op de **primaire Site Details** pagina. Virtuele machines die al zijn ingeschakeld voor replicatie of die niet zijn uitgevoerd, worden niet weergegeven. Selecteer de virtuele machines waarvoor u wenst te verzamelen van metrische gegevens. Automatisch selecteren van de VHD's worden gegevens verzameld voor de virtuele machines te.
8. Als u een replica-server of cluster hebt geconfigureerd in **netwerkgegevens**, geef de geschatte WAN-bandbreedte die u denkt dat wordt gebruikt tussen de primaire en replica-sites en selecteer de certificaten als u hebt geconfigureerd. certificaatverificatie.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. In **samenvatting**, Controleer de instellingen en klik op **volgende** om te beginnen met het verzamelen van meetgegevens. Hulpprogramma voortgang en status wordt weergegeven op de **berekenen capaciteit** pagina. Wanneer u het hulpprogramma is voltooid, klikt u op **rapport weergeven** om de uitvoer weer te geven. Logboeken en rapporten worden standaard opgeslagen in **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Stap 4: Interpreteer de resultaten

Hier volgen de belangrijke metrische gegevens. Metrische gegevens die hier niet worden weergegeven, kunt u negeren. Ze zijn niet relevant voor de Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>On-premises naar lokale replicatie

* Gevolgen van replicatie op de primaire host compute-, geheugen
* Gevolgen van de replicatie op de primaire schijf-opslagruimte met herstel-hosts IOP 's
* Totale bandbreedte die vereist zijn voor replicatie van verschillen (Mbps)
* Waargenomen netwerkbandbreedte tussen de primaire host en de herstelhost (Mbps)
* Voorstel voor de ideale aantal actieve parallelle overdrachten tussen de twee hosts/clusters

### <a name="on-premises-to-azure-replication"></a>On-premises naar Azure replicatie

* Gevolgen van replicatie op de primaire host compute-, geheugen
* Gevolgen van replicatie op de primaire host schijf opslagruimte IOP 's
* Totale bandbreedte die vereist zijn voor replicatie van verschillen (Mbps)

## <a name="more-resources"></a>Meer bronnen
* Voor gedetailleerde informatie over het hulpprogramma voor het document die wordt meegestuurd met het downloaden van het hulpprogramma te lezen.
* Bekijk een overzicht van het programma op van Keith Mayer [blog van TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [De resultaten](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) van onze prestatietests voor on-premises naar lokale Hyper-V-replicatie

## <a name="next-steps"></a>Volgende stappen

Nadat u klaar bent met het plannen van capaciteit kunt u beginnen met het implementeren van Site Recovery:

* [Hyper-V-machines in VMM-clouds repliceren naar Azure](site-recovery-vmm-to-azure.md)
* [Hyper-V-machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V-machines repliceren tussen VMM-sites](site-recovery-vmm-to-vmm.md)
