---
title: Azure Site Recovery bewaken | Microsoft Docs
description: Bewaken en problemen oplossen van problemen met Azure Site Recovery-replicatie en bewerkingen met behulp van de portal
author: bsiva
manager: abhemra
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: bsiva
ms.openlocfilehash: 48e53bcc542b9a0e00a544f80ec796082fa71f7b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210417"
---
# <a name="monitor-and-troubleshoot-site-recovery"></a>Bewaken en problemen oplossen van Site Recovery

In dit artikel leert u over het gebruik van Azure Site Recovery in de ingebouwde controlefuncties voor controle en probleemoplossing. 

## <a name="use-the-dashboard"></a>Het dashboard gebruiken

1. Klik in de kluis op **overzicht** om de Site Recovery-dashboard te openen. Er zijn dashboard-pagina's voor zowel de Site Recovery en de back-up en u kunt schakelen tussen deze.

    ![Site Recovery-dashboard](./media/site-recovery-monitor-and-troubleshoot/dashboard.png)

2.  Het dashboard worden samengevoegd voor alle controlegegevens voor de kluis op één locatie. Vanuit het dashboard, kunt u inzoomen op verschillende gebieden. 

    ![Site Recovery-dashboard](./media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png).

3. Op **gerepliceerde items**, klikt u op **Alles weergeven** om te zien van alle servers in de kluis.
4. Inzoomen door te klikken op de details van de status in elke sectie. In **infrastructuurweergave**, kunt u bewakingsgegevens sorteren op het type machines u repliceert.

## <a name="monitor-replicated-items"></a>Gerepliceerde items controleren

In de sectie gerepliceerde items ziet de status van alle machines waarvoor replicatie is ingeschakeld in de kluis.

**status** | **Details**
--- | ---
In orde | Replicatie wordt normaal gesproken voortgezet. Er is geen fout of waarschuwing symptomen worden gedetecteerd.
Waarschuwing | Een of meer waarschuwing problemen die mogelijk van invloed zijn op replicatie worden gedetecteerd.
Kritiek | Een of meer kritieke replicatie fout problemen zijn gedetecteerd.<br/><br/> Deze fout-problemen zijn doorgaans indicatoren dat replicatie vastgelopen of niet zo snel mogelijk de gegevenswijzigingssnelheid wordt uitgevoerd.
Niet van toepassing | Servers die momenteel worden niet verwacht worden gerepliceerd. Dit kan dus ook machines die een failover zijn.

## <a name="monitor-test-failovers"></a>Testfailovers van monitor

U kunt de status van de failovertest voor machines weergeven in de kluis.

- Het is raadzaam dat u een testfailover uitvoeren voor gerepliceerde machines ten minste eenmaal na zes maanden. Het is een manier om te controleren dat failover werkt zoals verwacht zonder te onderbreken van uw productieomgeving. 
- Een testfailover wordt beschouwd als geslaagd pas nadat het opruimen van de failover- en na een failover zijn voltooid.

**status** | **Details**
--- | ---
Test aanbevolen | Machines die nog niet een test-failover was omdat de beveiliging is ingeschakeld.
Succesvol uitgevoerd | Testfailovers machines met of meer successen te behalen.
Niet van toepassing | Computers die momenteel geschikt zijn voor een testfailover niet. Machines waarvoor een failover, is hebben bijvoorbeeld initiële replicatie/failover/testfailover wordt uitgevoerd.

## <a name="monitor-configuration-issues"></a>Problemen met de configuratie van monitor

De **configuratieproblemen** sectie ziet u een lijst met problemen die mogelijk van invloed op uw mogelijkheid is failover.

- Problemen met configuratie (met uitzondering van software-update-beschikbaarheid), worden gedetecteerd door een bewerking periodieke validator die standaard wordt elke 12 uur wordt uitgevoerd. U kunt afdwingen dat de bewerking validatie van het direct uitvoeren door te klikken op het pictogram naast de **configuratieproblemen** sectiekop.
- Klik op de koppelingen voor meer informatie. Voor problemen die invloed hebben op specifieke computers, klikt u op de **aandacht** in de **configuraties als doel** kolom. De details bevatten herstelaanbevelingen.

**status** | **Details**
--- | ---
Ontbrekende configuraties | Er is een vereiste instelling ontbreekt, zoals een herstelnetwerk of een resourcegroep.
Ontbrekende resources | Een opgegeven resource kan niet worden gevonden of is niet beschikbaar in het abonnement. Bijvoorbeeld: de resource is verwijderd of gemigreerd. Bewaakte resources opgenomen de doelresourcegroep, doel-VNet/subnet, logboek-/ doelopslagaccount, doelbeschikbaarheidsset, doel-IP-adres.
Het abonnementsquotum |  De beschikbare abonnementen quotum resourceverdeling vergeleken met het saldo die nodig is voor failover van alle machines in de kluis.<br/><br/> Als er niet voldoende resources, wordt een saldo Onvoldoende quotum gerapporteerd.<br/><br/> Quota bewaakt voor het aantal VM-kernen, VM-familie kerngeheugens, netwerkinterfaces (NIC)-kaart.
Software-updates | De beschikbaarheid van nieuwe software-updates en informatie over het verlopen softwareversies.


## <a name="monitoring-errors"></a>Bewaking van fouten 
De **Foutensamenvatting** sectie ziet u momenteel actieve fout problemen die mogelijk van invloed zijn op servers in de kluis en het aantal betrokken machines repliceren.

- Aan het begin van de sectie worden fouten die invloed hebben op de on-premises infrastructuuronderdelen weergegeven. Bijvoorbeeld, niet-ontvangst van een heartbeat van de Azure Site Recovery Provider die wordt uitgevoerd op de on-premises configuratieserver, de VMM-server of de Hyper-V-host.
- Volgende replicatie fout symptomen die invloed hebben op gerepliceerde servers worden weergegeven.
- Items in de tabel worden gesorteerd op aflopende volgorde van de ernst van fout, en vervolgens op het aflopende volgorde van de telling van de betrokken computers.
- Het aantal betrokken server is een handige manier om te begrijpen of één onderliggende probleem mogelijk van invloed zijn op meerdere machines. Bijvoorbeeld, een netwerkfout opgetreden kan mogelijk gevolgen hebben voor alle machines repliceren naar Azure. 
- Er kunnen meerdere replicatiefouten optreden op één server. In dit geval, telt elke fout symptoom die server in de lijst van de betrokken servers. Nadat het probleem is opgelost, replicatie parameters verbeteren en de fout van de machine is uitgeschakeld.

## <a name="monitor-the-infrastructure"></a>Controleer de infrastructuur.

De **infrastructuurweergave** ziet u de infrastructuuronderdelen die betrokken zijn in de replicatie en de status van de connectiviteit tussen servers en de Azure-services.

- Een groene lijn geeft aan of de verbinding in orde is.
- Een rode lijn met het pictogram verlaid fout geeft aan dat het bestaan van een of meer fout symptomen die invloed op de connectiviteit.
-  Beweeg de muisaanwijzer op het foutpictogram van de om de fout en het aantal betrokken entiteiten weer te geven. Klik op het pictogram voor een gefilterde lijst met betrokken entiteiten.

    ![Weergave van de infrastructuur voor site Recovery (kluis)](./media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

## <a name="tips-for-monitoring-the-infrastructure"></a>Tips voor het bewaken van de infrastructuur

- Zorg ervoor dat de on-premises infrastructuur-onderdelen (configuratieserver, processervers, VMM-servers, Hyper-V-hosts, VMware-machines) de meest recente versies van de Site Recovery Provider en/of agents worden uitgevoerd.
- Voor het gebruik van alle functies in de infrastructuurweergave, u moet worden uitgevoerd [Update rollup 22](https://support.microsoft.com/help/4072852) voor deze onderdelen.
- Voor het gebruik van de infrastructuurweergave, moet u de juiste replicatiescenario selecteren in uw omgeving. U kunt inzoomen in de weergave voor meer informatie. De volgende tabel ziet u welke scenario's worden weergegeven.

    **Scenario** | **status**  | **Weergave beschikbaar?**
    --- |--- | ---
    **Replicatie tussen on-premises sites** | Alle statussen | Nee 
    **Azure VM-replicatie tussen Azure-regio 's**  | Replicatie ingeschakeld/initiële replicatie wordt uitgevoerd | Ja
    **Azure VM-replicatie tussen Azure-regio 's** | Failover / failback | Nee   
    **VMware-replicatie naar Azure** | Replicatie ingeschakeld/initiële replicatie wordt uitgevoerd | Ja     
    **VMware-replicatie naar Azure** | Kan niet terug via/is mislukt | Nee      
    **Hyper-V-replicatie naar Azure** | Kan niet terug via/is mislukt | Nee

- Voor de infrastructuurweergave voor een enkele replicerende machine, in het kluismenu, klikt u op **gerepliceerde items**, en selecteer een server.  

### <a name="common-questions"></a>Veelgestelde vragen


**Waarom wordt het aantal virtuele machines in de kluis infrastructuurweergave verschilt van het totale aantal weergegeven in de gerepliceerde items?**

De weergave kluis infrastructuur is ingedeeld per replicatiescenario's. Alleen computers in de momenteel geselecteerde replicatiescenario worden opgenomen in het aantal voor de weergave. Bovendien tellen we alleen virtuele machines die zijn geconfigureerd om te repliceren naar Azure. Failover van machines of machines die worden gerepliceerd naar een on-premises site in de weergave niet meegerekend.

**Waarom is het aantal gerepliceerde items worden weergegeven in de sectie Essentials verschilt van het totale aantal gerepliceerde items op het dashboard?**

Alleen machines voor waarvoor de initiële replicatie is voltooid, zijn opgenomen in het aantal die wordt weergegeven in de sectie Essentials. Op de gerepliceerde items het totaal bevat alle machines in de kluis, met inbegrip van die voor waarvoor de initiële replicatie wordt momenteel uitgevoerd.


## <a name="monitor-recovery-plans"></a>Plannen voor herstel controleren

In de **herstelplannen sectie** Controleer de waarde van plannen, nieuwe abonnementen maken en bestaande wijzigen.  

## <a name="monitor-jobs"></a>Taken controleren

De **taken** gedeelte worden de status van Site Recovery-bewerkingen.

- De meeste bewerkingen in Azure Site Recovery wordt asynchroon uitgevoerd met een tracering-taak wordt gemaakt en gebruikt voor het bijhouden van de voortgang van de bewerking. 
- Het taakobject heeft alle informatie die u nodig hebt om de status en de voortgang van de bewerking te houden. 

Als volgt te werk om taken te controleren:

1. In het dashboard > **taken** sectie ziet u een overzicht van de taken die zijn voltooid, wordt uitgevoerd, of wachten op invoer in de afgelopen 24 uur zijn. U kunt klikken op elke status voor meer informatie over de relevante taken.
2. Klik op **weergeven van alle** om te zien van alle taken in de afgelopen 24 uur.

    > [!NOTE]
    > U kunt ook informatie over taak in het kluismenu openen > **Site Recovery-taken**. 

2. In de **Site Recovery-taken** lijst, een lijst met taken wordt weergegeven. Foutdetails voor een specifieke taken, filter op het bovenste menu krijgt u de takenlijst met op basis van specifieke criteria en geselecteerde taakdetails naar Excel exporteren.
3. U kunt inzoomen op een taak door erop te klikken. 

## <a name="monitor-virtual-machines"></a>Virtuele machines bewaken

In het dashboard toevoegen, kunt u machines op de pagina virtuele machines bewaken. 

1. Klik in de kluis op **gerepliceerde items** voor een lijst van gerepliceerde machines.  U kunt ook aan een gefilterde lijst van de beveiligde items ophalen door te klikken op een van de scoped snelkoppelingen op de dashboardpagina.

    ![Site Recovery gerepliceerd weergave van de lijst met items](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

2. Op de **gerepliceerde items** pagina die u kunt weergeven en filteren van gegevens. U kunt acties uitvoeren voor een bepaalde machine, met inbegrip van een testfailover uitvoeren of het weergeven van specifieke fouten in het actiemenu aan de bovenkant.
3.  Klik op **kolommen** om extra kolommen, bijvoorbeeld om weer te geven RPO, weer te geven als doel problemen met configuratie en replicatiefouten.
4. Klik op **Filter** om informatie op basis van specifieke parameters, zoals de replicatiestatus van of een bepaalde replicatiebeleid weer te geven.
5. Met de rechtermuisknop op een machine bewerkingen zoals het test-failover voor het starten of om details van de specifieke fout die is gekoppeld aan deze weer te geven.
6. Klik op een machine om te zoomen op meer informatie voor het. Details omvatten:
      - **Replicatiegegevens**: Huidige status en integriteit van de machine.
      - **RPO** (recovery point objective): Huidige RPO voor de virtuele machine en de tijd waarbinnen de RPO voor het laatst is berekend.
      - **Herstelpunten**: Meest recente beschikbare herstelpunten voor de machine.
      - **Failover-gereedheid**: Geeft aan of een test-failover is uitgevoerd voor de virtuele machine, de agent-versie die wordt uitgevoerd op de computer (voor machines waarop de Mobility-service wordt uitgevoerd) en eventuele configuratieproblemen.
      - **Fouten**: Lijst met replicatie fout symptomen momenteel waargenomen op de machine en de mogelijke oorzaken en/of acties.
      - **Gebeurtenissen**: Een chronologische lijst met recente gebeurtenissen die invloed hebben op de machine. Details van deze fout ziet u de symptomen momenteel waarneembare fout terwijl gebeurtenissen is een historisch overzicht van de problemen die mogelijk invloed hebben de machine.
      - **Infrastructuurweergave**: Toont de status van de infrastructuur voor het scenario dat bij het repliceren van machines naar Azure.

    ![Site Recovery gerepliceerd item details/overzicht](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)


### <a name="common-questions"></a>Veelgestelde vragen

**Hoe verschilt RPO van de meest recente beschikbare herstelpunt?**


- Site Recovery gebruikt een asynchroon proces met meerdere stappen om de machines repliceren naar Azure.
- Recente wijzigingen op de computer, samen met metagegevens, worden in de voorlaatste stap van de replicatie in een logboek/cache-opslagaccount gekopieerd.
- Deze wijzigingen, samen met de code voor het identificeren van een punt hersteld, worden geschreven naar het opslagaccount in de doelregio.
-  Site Recovery kan nu een herstelbare punt voor de virtuele machine worden gegenereerd.
- Het RPO heeft op dit moment is voldaan om de wijzigingen die zijn geüpload naar het opslagaccount dat tot nu toe. Met andere woorden, de machine RPO op dit moment is gelijk aan hoeveel tijd verstrijkt tussen de tijdstempel die overeenkomt met het herstelbare punt.
- Nu Site Recovery kiest de geüploade gegevens uit het opslagaccount en toegepast op de replicaschijven die zijn gemaakt voor de machine.
- Site Recovery een herstelpunt genereert, en is nu beschikbaar voor herstel na een failover. Dus de meest recente beschikbare herstelpunt geeft aan dat de tijdstempel die overeenkomt met de meest recente herstelpunt dat al is verwerkt en toegepast op de replicaschijven.

> [!NOTE]
> Een onjuiste systeemtijd op de replicerende bronmachine of op on-premises infrastructuurservers wordt de berekende RPO-waarde een verkeerd. Voor nauwkeurige RPO-rapportage, zorg ervoor dat de systeemklok nauwkeurig op alle servers en virtuele machines. 

## <a name="subscribe-to-email-notifications"></a>Abonneren op e-mailmeldingen

U kunt zich abonneren voor het ontvangen van e-mailmeldingen voor deze kritieke gebeurtenissen:
 
- Kritieke status voor de gerepliceerde machine.
- Er is geen connectiviteit tussen de on-premises infrastructuuronderdelen en de Site Recovery-service. Connectiviteit tussen de Site Recovery en on-premises servers die zijn geregistreerd in een kluis is gedetecteerd met een heartbeat-mechanisme.
- Failover-fouten.

Abonneer u als volgt:

In de kluis > **bewaking en rapporten** sectie, klikt u op **Site Recovery-gebeurtenissen**.
2. Klik op **e-mailmeldingen**.
3. In **e-mailmelding**, meldingen inschakelen en opgeven aan wie de om naar te verzenden. U kunt verzenden naar alle abonnementsbeheerders worden verzonden, meldingen en desgewenst specifieke e-mailadressen.

    ![E-mailmeldingen](./media/site-recovery-monitor-and-troubleshoot/email.png)
