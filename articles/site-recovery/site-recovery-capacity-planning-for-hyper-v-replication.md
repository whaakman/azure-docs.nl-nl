---
title: Hyper-V capaciteitsplanning hulpprogramma voor Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de Hyper-V hulpprogramma voor capaciteitsplanning voor Azure Site Recovery uitvoert
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
ms.openlocfilehash: 80c3dcb65bd74bcfa3acc5f12dd5c45cd1c06455
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="hyper-v-capacity-planning-tool-for-site-recovery"></a>Hyper-V capaciteitsplanning hulpprogramma voor Site Recovery

Een nieuwe uitgebreide versie van [Azure Site Recovery implementatie Planner voor Hyper-V naar Azure-implementatie](site-recovery-hyper-v-deployment-planner.md) is nu beschikbaar. Het oude hulpprogramma vervangt. Het nieuwe hulpprogramma gebruiken voor het plannen van uw implementatie. Het hulpprogramma biedt de volgende richtlijnen:

* Evaluatie van VM in aanmerking komt, op basis van het aantal schijven, schijfgrootte IOP's, verloop en enkele VM-kenmerken
* Netwerkbandbreedte moet versus RPO assessment
* Vereisten voor Azure-infrastructuur
* On-premises infrastructuurvereisten
* Initiële replicatie batchverwerking richtlijnen
* Geschatte kosten van de totale disaster recovery naar Azure

Als onderdeel van uw Azure Site Recovery-implementatie moet u de replicatie en de vereiste bandbreedte te achterhalen. Het hulpprogramma van Hyper-V capaciteitsplanning voor Site Recovery kunt u deze vereisten voor het repliceren van Hyper-V virtuele machines bepalen.

Dit artikel wordt beschreven hoe u het hulpprogramma voor capaciteitsplanning van Hyper-V uitvoert. U kunt dit hulpprogramma samen met de informatie in gebruiken [planning van capaciteit voor de Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Voordat u begint
U kunt het hulpprogramma uitvoeren op een Hyper-V server of cluster-knooppunt in de primaire site. Het hulpprogramma nodig, de Hyper-V-hostservers:

* Besturingssysteem: WindowsServer 2012 of 2012 R2
* Geheugen: 20 MB (minimum)
* CPU: 5 procent overhead (minimum)
* Schijfruimte: 5 MB (minimum)

Voordat u het hulpprogramma uitvoert, moet u de primaire site voorbereiden. Als u tussen twee lokale sites repliceert en u wilt controleren, bandbreedte, moet u een replica-server voorbereiden.

## <a name="step-1-prepare-the-primary-site"></a>Stap 1: Bereid de primaire site

1. Maak een lijst van alle Hyper-V-machines die u wilt repliceren op de primaire site. Lijst van de Hyper-V-hosts of clusters waar ze zich bevinden. Het hulpprogramma kan worden uitgevoerd voor meerdere zelfstandige hosts of voor een cluster, maar niet allebei tegelijk. Het moet ook afzonderlijk uitvoeren voor elk besturingssysteem. Verzamel de volgende informatie over Hyper-V-servers:

   * Windows Server 2012 zelfstandige servers
   * Windows Server 2012-clusters
   * Zelfstandige servers voor Windows Server 2012 R2
   * Windows Server 2012 R2-clusters

2. Inschakelen van externe toegang tot Windows Management Instrumentation op de Hyper-V-hosts en clusters. Deze opdracht uitvoeren op elke server of cluster Zorg ervoor dat firewall-regels en de gebruikersmachtigingen zijn ingesteld:

        netsh firewall set service RemoteAdmin enable
3. Schakel de bewaking van toepassingsprestaties op servers en clusters als volgt:

   a. Open Windows Firewall met de **geavanceerde beveiliging** -module. 
   
   b. Selecteer de regel binnenkomende **COM +-netwerktoegang (DCOM-IN)**. Selecteer alle regels in de **Extern logboekbeheer** groep.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Stap 2: Een replica-server (on-premises naar lokale replicatie) voorbereiden
Als u naar Azure repliceert, hoeft u niet te doen in deze stap.

Het is raadzaam dat u ingesteld één Hyper-V-host als een herstelserver zodat een dummy-virtuele machine kan worden gerepliceerd naar deze om te controleren van de bandbreedte. U kunt deze stap overslaan, maar u kunt geen bandbreedte meten, tenzij u dit doen.

1. Als u gebruiken van een clusterknooppunt als de replica wilt, configureert u de Hyper-V Replica Broker:

   a. In **Serverbeheer**Open **Failoverclusterbeheer**.

   b. Verbinding maken met het cluster en markeert u de naam van het cluster. Selecteer **acties** > **functie configureren** om de wizard hoge beschikbaarheid te openen.

   c. In **rol selecteren**, selecteer **Hyper-V Replica Broker**. Voer een naam voor in de wizard **NetBIOS-naam**. Geef een adres voor **IP-adres** moet worden gebruikt als het verbindingspunt met het cluster (een clienttoegangspunt genoemd). De **Hyper-V Replica Broker** is geconfigureerd, wat resulteert in een client access point-naam die u Houd er rekening mee.

   d. Controleren of de functie Hyper-V Replica Broker online beschikbaar is en naar alle knooppunten van het cluster een failover. Met de rechtermuisknop op de rol en selecteer **verplaatsen** > **knooppunt selecteren**. Selecteer een knooppunt en selecteer vervolgens **OK**.

   e. Als u verificatie op basis van certificaten gebruikt, controleert u of elk clusterknooppunt en de client toegangspunt dat alle certificaat geïnstalleerd hebben.

2. Om een replica-server, de volgende stappen uitvoeren:

   a. Voor een cluster, opent u **fout Clusterbeheer** en maak verbinding met het cluster. Selecteer **rollen**, en selecteer vervolgens een rol. Selecteer **replicatie-instellingen** > **Maak van dit cluster een replicaserver**. Als u een cluster als de replica gebruikt, moet u de Hyper-V Replica Broker-rol aanwezig zijn op het cluster in de primaire site ook.

   b. Voor een zelfstandige server, opent u **Hyper-V-beheer**. In de **acties** deelvenster **Hyper-V-instellingen** voor de server die u wilt inschakelen. In **replicatieconfiguratie**, selecteer **deze computer inschakelen als een Replica-server**.

3. Als u verificatie instelt, moet u deze stappen uitvoeren:

   a. Onder **verificatie en poorten**, selecteer de manier waarop om de primaire server en de verificatie te verifiëren poorten. Als u een certificaat gebruikt, selecteert u **certificaat selecteren** om een te selecteren. Kerberos niet gebruiken als de primaire en herstelsites Hyper-V-hosts zich in hetzelfde domein of in vertrouwde domeinen. Gebruik van certificaten voor verschillende domeinen of voor de implementatie van een werkgroep.

   b. Onder **autorisatie en opslag**, toestaan **eventuele** geverifieerde (primaire) servers om replicatiegegevens te verzenden naar deze replicaserver.

   ![Replicatieconfiguratie](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

   c. Als u wilt controleren of de listener wordt uitgevoerd voor het protocol of de opgegeven poort, voer **netsh http show servicestate**. 

4. Firewalls instellen. Tijdens de installatie van Hyper-V worden de firewallregels voor verkeer op de standaard-poorten (HTTPS op 443) en Kerberos op 80 gemaakt. Deze regels als volgt inschakelen:

    - Certificaatverificatie op cluster (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
    - Kerberos-verificatie op cluster (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
    - Certificaatverificatie op zelfstandige server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
    - Kerberos-verificatie op de zelfstandige server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planning-tool"></a>Stap 3: Voer het hulpprogramma voor capaciteitsplanning
Na het voorbereiden van uw primaire site en een herstelserver hebt ingesteld, kunt u het hulpprogramma uitvoeren.

1. [Download](https://www.microsoft.com/download/details.aspx?id=39057) het hulpprogramma uit het Microsoft Download Center.

2. Het hulpprogramma uitvoeren vanaf een van de primaire servers of een van de knooppunten van het primaire cluster. Met de rechtermuisknop op het .exe-bestand en kies vervolgens **als administrator uitvoeren**.

3. In **voordat u begint**, opgeven hoe lang u gegevens wilt verzamelen. Het is raadzaam dat u het hulpprogramma tijdens de productie-uren uitvoeren om ervoor te zorgen dat er gegevens vertegenwoordiger. Als u valideren alleen verbinding met het netwerk wilt, kunt u alleen een minuut verzamelen.

    ![Voordat u begint](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. In **primaire Site Details**, de servernaam of de FQDN-naam opgeven voor een zelfstandige host. Geef de FQDN-naam van de client access point, clusternaam of een willekeurig knooppunt in het cluster voor een cluster. Selecteer **Volgende**. Het hulpprogramma detecteert automatisch de naam van de server die wordt uitgevoerd. Het hulpprogramma neemt over virtuele machines die kunnen worden gecontroleerd voor de gespecificeerde servers.

    ![Details van de primaire Site](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. In **Replica sitegegevens**, als u naar Azure repliceren of als u naar een secundair datacenter repliceert en een replica-server hebt ingesteld selecteert **overslaan tests met betrekking tot de replica-site**. Als u naar een secundair datacenter repliceert en u een ReplicaType instellen, voert u de FQDN-naam van de zelfstandige server of de client access point voor het cluster in **Server name (of) Hyper-V Replica Broker CAP**.

    ![Details van de replica-Site](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. In **uitgebreide Replica Details**, selecteer **overslaan van de tests met betrekking tot de site van de uitgebreide Replica**. Deze tests worden niet ondersteund door Site Recovery.

7. In **kiezen virtuele machines repliceren**, het hulpprogramma maakt verbinding met de server of het cluster. Virtuele machines wordt weergegeven en schijven op de primaire server, op basis van de instellingen die u hebt opgegeven op de **primaire Site Details** pagina. Virtuele machines die al zijn ingeschakeld voor replicatie zijn of die niet zijn uitgevoerd worden niet weergegeven. Selecteer de virtuele machines waarvoor u wenst te verzamelen van metrische gegevens. Automatisch selecteren van de VHD's worden gegevens verzameld voor de virtuele machines, te.

8. Als u een replica-server of cluster geconfigureerd in **netwerkgegevens**, geef de geschatte WAN-bandbreedte tussen de primaire en replica-sites worden gebruikt. Als u certificaatauthenticatie hebt geconfigureerd, selecteert u de certificaten.

    ![Informatie over het netwerk](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

9. In **samenvatting**, Controleer de instellingen. Selecteer **volgende** om te beginnen met het verzamelen van meetgegevens. Hulpprogramma voortgang en status worden weergegeven op de **berekenen capaciteit** pagina. Wanneer u het hulpprogramma is voltooid, selecteert u **rapport weergeven** om de uitvoer weer te geven. Logboeken en rapporten worden standaard opgeslagen in **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![Capaciteit wordt berekend](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Stap 4: Interpreteer de resultaten

Hier volgen de belangrijke metrische gegevens. Metrische gegevens die hier niet worden weergegeven, kunt u negeren. Ze zijn niet relevant voor de Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>On-premises naar lokale replicatie

* Gevolgen van replicatie op de primaire host berekenings- en geheugen
* Gevolgen van replicatie op de primaire host en de herstelhost schijfruimte voor opslag en IOP 's
* Totale bandbreedte die vereist zijn voor replicatie van verschillen (Mbps)
* Waargenomen netwerkbandbreedte tussen de primaire host en de herstelhost (Mbps)
* Voorstel voor de ideale aantal actieve parallelle overdrachten tussen de twee hosts of clusters

### <a name="on-premises-to-azure-replication"></a>On-premises naar Azure replicatie

* Gevolgen van replicatie op de primaire host berekenings- en geheugen
* Gevolgen van replicatie op de primaire host schijfruimte voor opslag en IOP 's
* Totale bandbreedte die vereist zijn voor replicatie van verschillen (Mbps)

## <a name="more-resources"></a>Meer bronnen

* Lees voor meer informatie over het hulpprogramma voor het document die wordt meegestuurd met het hulpprogramma downloaden.
* Bekijk een overzicht van het programma op van Keith Mayer [blog van TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [De resultaten](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) van prestatietests voor on-premises naar lokale Hyper-V-replicatie.

## <a name="next-steps"></a>Volgende stappen

Als u klaar bent met het plannen van capaciteit, kunt u Site Recovery kunt implementeren:
* [Hyper-V-machines in VMM-clouds repliceren naar Azure](site-recovery-vmm-to-azure.md)
* [Hyper-V-machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V-machines repliceren tussen VMM-sites](site-recovery-vmm-to-vmm.md)
