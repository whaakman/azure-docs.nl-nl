---
title: Controleren en problemen oplossen van Azure Site Recovery | Microsoft Docs
description: Controleren en problemen oplossen van problemen met Azure Site Recovery replicatie en bewerkingen met behulp van de portal
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/06/2017
ms.author: bsiva
ms.openlocfilehash: 6dca032c8611ac4f7e66eb6f7e22e53f49209143
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>Bewaking en probleemoplossing van Azure Site Recovery

In dit artikel leert u hoe u Azure Site Recovery in ingebouwde controlemogelijkheden voor bewaking en probleemoplossing. Leer hoe u het volgende doet:
> [!div class="checklist"]
> - Gebruik het dashboard van Azure Site Recovery (overzichtspagina kluis)
> - Controleren en problemen oplossen van problemen met replicatie
> - Monitor voor Azure Site Recovery-taken/bewerkingen
> - Abonneren op e-mailmeldingen

## <a name="using-the-azure-site-recovery-dashboard"></a>Met behulp van de Azure Site Recovery-dashboard

Het Azure Site Recovery-dashboard op de overzichtspagina kluis consolideert alle controlegegevens voor de kluis op één locatie. Start op het kluisdashboard en dive diepere door te navigeren door de delen van het dashboard voor meer informatie. De belangrijke onderdelen van het dashboard van Azure Site Recovery zijn als volgt:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Schakelen tussen Azure Backup en Azure Site Recovery dashboards

De schakeloptie boven aan de overzichtspagina kunt u schakelen tussen de dashboardpagina's voor herstel van sites en back-up. Deze selectie, eenmaal is gemaakt, onthouden en standaard ingesteld op de volgende keer openen van de overzichtspagina voor de kluis. Selecteer de optie Site Recovery om te zien van de Site Recovery-dashboard. 

Elke 10 minuten vernieuwd de verschillende onderdelen van de dashboardpagina van Azure Site Recovery automatisch zodat het dashboard wordt aangegeven dat de meest recente informatie.

![Bewaking van functies in de overzichtspagina van Azure Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Gerepliceerde Items

De sectie gerepliceerde items van het dashboard geeft een overzicht van de replicatiestatus van de beveiligde servers in de kluis. 

<table>
<tr>
    <td>In orde</td>
    <td>Replicatie wordt normaal uitgevoerd voor deze servers en er geen fout of waarschuwing problemen zijn gedetecteerd.</td>
</tr>
<tr>
    <td>Waarschuwing </td>
    <td>Een of meer waarschuwing problemen die mogelijk invloed op replicatie of aangeven dat de replicatie is niet normaal gesproken enige gedetecteerd voor deze servers.</td>
</tr>
<tr>
    <td>Kritiek</td>
    <td>Een of meer kritieke replicatie symptomen fout gedetecteerd voor deze servers. Deze fout symptomen zijn meestal indicatoren replicatie ofwel is vastgelopen of verloopt niet snel de gegevens worden gewijzigd voor deze servers toe te passen.</td>
</tr>
<tr>
    <td>Niet van toepassing</td>
    <td>Servers die momenteel worden niet repliceren verwacht, zoals servers die zijn overgenomen.</td>
</tr>
</table>

Klik op de beschrijving van de replicatie health naast de ring een overzicht van de beveiligde servers die zijn gefilterd door de status van replicatie. De weergave die alle in de buurt van de sectietitel koppelen is een snelkoppeling naar de pagina gerepliceerde items voor de kluis. Gebruik de weergave dat alle koppelen aan de lijst met alle servers in de kluis.

### <a name="3-failover-test-success"></a>3. Failover-test geslaagd

De failover-test geslaagd-sectie van het dashboard geeft een splitsing van virtuele machines in de kluis op basis van test failover-status. 

<table>
<tr>
    <td>Aanbevolen</td>
    <td>Virtuele machines die nog niet is een geslaagde testfailover sinds het moment dat ze een beveiligde status bereikt.</td>
</tr>
<tr>
    <td>Geslaagd</td>
    <td>Virtuele machines die een of meer geslaagde testfailovers hebben gehad.</td>
</tr>
<tr>
    <td>Niet van toepassing</td>
    <td>Virtuele machines die niet momenteel in aanmerking komen voor een testfailover zijn. Voorbeelden zijn: servers voor welke initiële replicatie is mislukt voor servers, in voortgang, de servers waarvoor een failover uitgevoerd wordt, de servers waarvoor een testfailover al uitgevoerd wordt.</td>
</tr>
</table>

Klik op de status van de test failover naast de ring, dus overzicht van de beveiligde servers op basis van hun test failover-status.
 
> [!IMPORTANT]
> Als een best practice is het raadzaam dat u een testfailover uitvoeren op de beveiligde servers ten minste één keer om de zes maanden. Een testfailover uitvoeren is een niet-verstoren manier voor het testen van failover van uw servers en toepassingen naar een geïsoleerde omgeving en kunt u uw zakelijke continuïteit voorbereid evalueren.

 Een testfailover op een server of een herstelplan wordt beschouwd als geslaagd pas nadat de testfailover zowel de testfailover van opschonen zijn voltooid.

### <a name="4-configuration-issues"></a>4. Problemen met configuratie

De configuratiesectie problemen bevat een lijst met problemen die mogelijk van invloed op uw mogelijkheid tot succes failover virtuele machines. De klassen van problemen die worden vermeld in deze sectie zijn:
 - **Configuraties ontbreekt:** beveiligde servers nodig configuraties, zoals een netwerk voor herstel of een resourcegroep recovery ontbreekt.
 - **Ontbrekende resources:** geconfigureerd doel/herstel resources niet gevonden of is niet beschikbaar in het abonnement. Bijvoorbeeld, de bron is verwijderd of is gemigreerd naar een ander abonnement of resourcegroep. De volgende configuraties voor doel/herstel worden gecontroleerd op beschikbaarheid: doelresourcegroep, doel-virtuele netwerk en subnet, doel-logboek storage-account, doel beschikbaarheidsset, doel-IP-adres.
 - **Abonnementenquotum:** quotum resourceverdeling van de beschikbare abonnementen wordt vergeleken met het saldo failover mogelijk moeten alle virtuele machines in de kluis. Als de voorraad onvoldoende wordt gevonden, wordt er onvoldoende quota saldo gerapporteerd. Quota's voor de volgende Azure-resources worden bewaakt: aantal virtuele machines-core, familie core-telling van virtuele machine, aantal netwerkkaarten (NIC)-interface.
 - **Software-updates:** de beschikbaarheid van nieuwe software-updates, softwareversies verloopt.

Problemen met de configuratie (met uitzondering van beschikbaarheid van software-updates), worden gedetecteerd door een periodieke validator bewerking die standaard wordt elke 12 uur wordt uitgevoerd. U kunt afdwingen dat de bewerking validator onmiddellijk uitvoeren door te klikken op het pictogram naast de *configuratieproblemen* sectiekop.

Klik op de koppelingen voor meer informatie over de vermelde problemen en virtuele machines die zijn beïnvloed door deze. Voor problemen die invloed hebben op specifieke virtuele machines, kunt u meer informatie krijgen door te klikken op de **aandacht** koppeling onder de doelkolom configuraties voor de virtuele machine. De volgende gegevens bevatten aanbevelingen voor hoe u de gedetecteerde problemen kunt herstellen.

### <a name="5-error-summary"></a>5. Samenvatting van fouten

De samenvatting van de fout ziet u de momenteel actieve replicatie fout problemen die mogelijk van invloed op de replicatie van servers in de kluis, samen met het aantal betrokken entiteiten als gevolg van elke fout optreedt.

De replicatie fout symptomen voor servers in een status Kritiek of waarschuwing replicatie in de samenvattingstabel fout weergegeven. 

- Fouten die van invloed op de on-premises infrastructuuronderdelen zoals de niet-ontvangst van een heartbeat van de Azure Site Recovery Provider uitgevoerd op de lokale configuratie-Server, VMM-server of Hyper-V-host worden weergegeven aan het begin van de samenvatting fout sectie
- Replicatie fout symptomen die invloed hebben op beveiligde servers naast vermeld. De fout samenvattingstabel vermeldingen worden gesorteerd op aflopende volgorde van de ernst van de fout en vervolgens op aflopende volgorde van het aantal betrokken servers.
 

> [!NOTE]
> 
>  Meerdere replicatie fout symptomen kunnen op één server worden waargenomen. Als er meerdere fout symptomen op één server geteld elke fout symptoom die server in de lijst met de betrokken servers. Als het onderliggende probleem, wat resulteert in een fout-probleem is opgelost, replicatie parameters verbeteren en de fout van de virtuele machine is uitgeschakeld.
>
> > [!TIP]
> Het aantal betrokken servers is een handige manier om te begrijpen als één onderliggende probleem mogelijk van invloed op meerdere servers. Bijvoorbeeld, een storing netwerk kan mogelijk gevolgen hebben voor alle servers van een on-premises site te repliceren naar Azure. Deze weergave wordt snel dat is hersteld of een onderliggende probleem replicatie voor meerdere servers wordt opgelost.
>

### <a name="6-infrastructure-view"></a>6. Weergave van de infrastructuur

De infrastructuur-weergave bevat een scenario verstandig visuele representatie van de betrokken bij de replicatie infrastructurele onderdelen. Ook visueel ziet u de status van de verbinding tussen de verschillende servers en tussen de servers en de Azure-services die zijn betrokken bij de replicatie. 

Een groene lijn geeft aan dat connection orde is, terwijl een rode lijn met het pictogram overlappende fout geeft aan dat het bestaan van een of meer fout problemen die van invloed op de connectiviteit tussen de onderdelen die zijn betrokken. De muisaanwijzer muiswijzer op het pictogram van de fout op de regel bevat de fout en het aantal betrokken entiteiten. 

Op het foutpictogram ziet u een gefilterde lijst met betrokken entiteiten voor de fouten.

![Weergave van site Recovery-infrastructuur (kluis)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Zorg ervoor dat de meest recente versie van Azure Site Recovery-software worden uitgevoerd door de onderdelen van de lokale infrastructuur (configuratieserver, aanvullende processen servers repliceren van virtuele VMware-machines, Hyper-V-hosts en VMM-servers). Als u de functies van de weergave van de infrastructuur te gebruiken, moet u actief [updatepakket 22](https://support.microsoft.com/help/4072852) of hoger voor Azure Site Recovery

Voor het gebruik van de infrastructuur-weergave, selecteer het juiste replicatiescenario (virtuele machines in Azure, VMware-machines of fysieke server of Hyper-V), afhankelijk van uw bronomgeving. De weergave van de infrastructuur die zijn gepresenteerd in de overzichtspagina van kluis is een samengevoegde weergave voor de kluis. U kunt inzoomen verder naar de afzonderlijke onderdelen door te klikken op de vakken.

Een weergave met een infrastructuur binnen het bereik van de context van een enkele replicerende machine is beschikbaar op de overzichtspagina van gerepliceerde item. Ga naar de overzichtspagina voor een replicerende server gaat u naar de gerepliceerde items in het kluismenu en selecteer de server kunt u de details voor.

### <a name="infrastructure-view---faq"></a>Weergave van de infrastructuur - Veelgestelde vragen

**V:** Waarom krijg ik de weergave van de infrastructuur voor mijn VM niet zien? </br>
**A:** De infrastructuur weergave-functie is alleen beschikbaar voor virtuele machines die zijn gerepliceerd naar Azure. De functie is momenteel niet beschikbaar voor virtuele machines die worden gerepliceerd tussen on-premises sites.

**V:** Waarom is het aantal virtuele machines in de kluis infrastructuur weergave die verschilt van het totale aantal weergegeven in de gerepliceerde items ring?</br>
**A:** De weergave van de infrastructuur kluis is afgestemd op replicatiescenario's. Alleen virtuele machines die deel uitmaken van het momenteel geselecteerde replicatiescenario zijn opgenomen in het aantal virtuele machines in de weergave van de infrastructuur wordt weergegeven. Bovendien alleen virtuele machines die momenteel zijn geconfigureerd voor het repliceren naar Azure voor het geselecteerde scenario zijn opgenomen in het aantal virtuele machines in de weergave van de infrastructuur wordt weergegeven (voor bijvoorbeeld: failover van virtuele machines, virtuele machines repliceren terug naar de site van een on-premises worden niet opgenomen in de weergave van de infrastructuur.)

**V:** Waarom is het aantal gerepliceerde items weergegeven in de sectie essentials op de overzichtspagina verschilt van het totale aantal gerepliceerde items worden weergegeven in het diagram ring in het dashboard?</br>
**A:** Alleen de virtuele machines voor welke initiële replicatie is voltooid zijn opgenomen in de telling die wordt weergegeven in de lade essentials. De gerepliceerde items ring totaal bevat alle virtuele machines in de kluis en servers voor welke initiële replicatie momenteel wordt uitgevoerd.

**V:** Welke replicatiescenario's is de infrastructuur-weergave beschikbaar voor? </br>
**A:**
>[!div class="mx-tdBreakAll"]
>|Replicatiescenario  | Status virtuele machine  | Infrastructuur weergave beschikbaar  |
>|---------|---------|---------|
>|Virtuele machines repliceren tussen twee lokale sites     | -        | Nee      |
>|Alle     | Failover         |  Nee       |
>|Virtuele machines repliceren tussen twee Azure-regio 's     | Initiële replicatie uitgevoerd of beveiligd         | Ja         |
>|VMware-virtuele machines repliceren naar Azure     | Initiële replicatie uitgevoerd of beveiligd        | Ja        |
>|VMware-virtuele machines repliceren naar Azure     | Failover van virtuele machines worden gerepliceerd naar een on-premises VMware-Site         | Nee        |
>|Hyper-V virtuele machines repliceren naar Azure     | Initiële replicatie uitgevoerd of beveiligd        | Ja       |
>|Hyper-V virtuele machines repliceren naar Azure     | Failover / Failback wordt uitgevoerd        |  Nee       |


### <a name="7-recovery-plans"></a>7. Plannen voor herstel

De sectie herstel plannen toont het aantal plannen voor herstel in de kluis. Klikt u op het overzicht van de herstelplannen, nieuwe herstelplannen maken of bestaande relaties te bewerken. 

### <a name="8-jobs"></a>8. Taken

Azure Site Recovery-taken volgen de status van Azure Site Recovery-bewerkingen. De meeste bewerkingen in de Azure Site Recovery worden asynchroon uitgevoerd met een bijhouden taak die wordt gebruikt voor het bijhouden van de voortgang van de bewerking.  Zie voor meer informatie over het bewaken van de status van een bewerking, de [Monitor Azure Site Recovery-taken/Operations](#monitor-azure-site-recovery-jobsoperations) sectie.

Deze sectie van de taken van het dashboard bevat de volgende informatie:

<table>
<tr>
    <td>Mislukt</td>
    <td>Kan Azure Site Recovery-taken in de afgelopen 24 uur</td>
</tr>
<tr>
    <td>Wordt uitgevoerd</td>
    <td>Azure Site Recovery-taken die momenteel uitgevoerd worden</td>
</tr>
<tr>
    <td>Wacht op invoer</td>
    <td>Azure Site Recovery-taken die momenteel is onderbroken wachten op invoer van de gebruiker.</td>
</tr>
</table>

De weergave alle koppeling naast de sectiekop is een snelkoppeling naar de pagina van de lijst met taken.

## <a name="monitor-and-troubleshoot-replication-issues"></a>Controleren en problemen oplossen van problemen met replicatie

Naast de informatie die beschikbaar zijn in de kluis dashboardpagina krijgt u extra details en informatie over probleemoplossing in de pagina van de lijst met virtuele machines en de detailpagina voor de virtuele machine. U kunt de lijst met beveiligde virtuele machines in de kluis weergeven door de **gerepliceerde items** optie in het kluismenu. U kunt ook een gefilterde lijst met beveiligde items ophalen door te klikken op een van de scoped snelkoppelingen die beschikbaar zijn op de dashboardpagina kluis.

![Site Recovery gerepliceerde items lijstweergave](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

De filteroptie op de pagina van de lijst met gerepliceerde items kunt u verschillende filters zoals replicatiestatus en beleid voor wachtwoordreplicatie toepassen. 

De kolom selector-optie kunt u aanvullende kolommen moet worden weergegeven zoals RPO, problemen met de doel-configuratie en replicatiefouten opgeven. U kunt bewerkingen op een virtuele machine of fouten weergeven die invloed hebben op de virtuele machine door met de rechtermuisknop op een bepaalde rij van de lijst met computers op te starten.

Voor verdere detailanalyse, selecteert u een virtuele machine door erop te klikken. Hiermee opent u de pagina met details van de virtuele machine. De overzichtspagina onder de details van de virtuele machine bevat een dashboard vindt u aanvullende informatie met betrekking tot de machine. 

Op de overzichtspagina voor de replicerende machine vindt u het:
- RPO (beoogd herstelpunt): huidige RPO voor de virtuele machine en de tijd waarop de RPO voor het laatst is berekend.
- Meest recente beschikbare herstelpunten voor de machine
- Problemen met de configuratie als een die mogelijk van invloed op de gereedheid van de failover van de machine. Klik op de koppeling voor meer informatie.
- Details van fout: de lijst met replicatie fout symptomen momenteel waargenomen op de machine samen met mogelijke oorzaken en aanbevolen herstelbewerkingen
- Gebeurtenissen: Een chronologische lijst met recente gebeurtenissen die invloed hebben op de machine. Details van fouten toont de symptomen momenteel waarneembare fout op de machine, gebeurtenissen is een historisch overzicht van de diverse gebeurtenissen die mogelijk hebben invloed op de machine met inbegrip van de fout symptomen die eerder is wellicht de voor de machine.
- Weergave van de infrastructuur voor machines repliceren naar Azure

![Site Recovery gerepliceerd item details/overzicht](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Het actiemenu boven aan de pagina biedt opties voor verschillende bewerkingen uitvoeren zoals failover op de virtuele machine testen. De knop fout details in het actiemenu kunt u alle actieve fouten waaronder replicatiefouten, problemen met de configuratie en configuratie best practices op basis van waarschuwingen voor de virtuele machine bekijken.

> [!TIP]
> Hoe wordt RPO of recovery point objective verschilt van de meest recente beschikbare herstelpunt?
> 
>Azure Site Recovery maakt gebruik van een asynchrone proces van meerdere virtuele machines repliceren naar Azure. In de voorlaatste stap van de replicatie, worden recente wijzigingen op de virtuele machine samen met metagegevens gekopieerd naar een logboekbestand/cache storage-account. Zodra deze wijzigingen samen met het label voor het identificeren van een herstelbare punt is geschreven naar het opslagaccount in de doelregio, heeft Azure Site Recovery de informatie die nodig zijn voor het genereren van een herstelbare punt voor de virtuele machine. De RPO is op dit punt wordt voldaan om de wijzigingen die tot nu toe naar het opslagaccount geüpload. Met andere woorden, de RPO voor de virtuele machine op dit moment uitgedrukt in tijd, is gelijk aan verstreken tijd van de tijdstempel die overeenkomt met het herstelbare punt.
>
>De Azure Site Recovery-service uitgevoerd op de achtergrond, neemt over van de geüploade gegevens van het opslagaccount en past deze naar de replica-schijven voor de virtuele machine gemaakt. Vervolgens genereert een herstelpunt wordt gemaakt en wordt deze gemaakt bij een failover beschikbaar voor herstel. De meest recente beschikbare herstelpunt geeft aan dat de tijdstempel die overeenkomt met de meest recente herstelpunt dat al is verwerkt en toegepast op de replica-schijven.
>> [!WARNING]
> Een vertekende klok of de systeemprestatiemeteritems tijd op de replicerende bronmachine of de lokale infrastructuurservers wordt een verkeerd berekende RPO-waarde. Om ervoor te zorgen nauwkeurige rapportage van RPO waarden ervoor te zorgen dat de klok op de servers die zijn betrokken bij de replicatie correct is. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>Monitor voor Azure Site Recovery-taken/bewerkingen

Azure Site Recovery wordt de bewerkingen die u asynchroon opgeeft uitgevoerd. Voorbeelden van bewerkingen die u kunt uitvoeren zijn replicatie inschakelen, herstelplan maken, testen van failover, replicatie-instellingen enz bijwerken. Om deze bewerking heeft een bijbehorende taak die is gemaakt voor het volgen en controleren van de bewerking. Het taakobject heeft de benodigde informatie vereist is voor u om de status en de voortgang van de bewerking te houden. U kunt de status van de verschillende Site Recovery-bewerkingen voor de kluis volgen op de pagina taken. 

Voor een overzicht van Site Recovery-taken voor de kluis gaat de **bewaking en rapporten** sectie van de kluismenu en selecteer taken > Site Recovery-taken. Selecteer een taak in de lijst met taken op de pagina door te klikken op de opgegeven taak voor meer informatie. Als een taak is nog niet voltooid of fouten bevat, kunt u meer informatie bekijken op het herstel van de fout en mogelijke door te klikken op de knop fout details boven aan de pagina met details taak (ook toegankelijk is vanaf de pagina van de lijst met taken door met de rechtermuisknop op de mislukte taak). U kunt de filteroptie in het actiemenu boven aan de pagina van de lijst met taken gebruiken voor het filteren van de lijst op basis van een bepaald criterium voldoen en gebruik de knop exporteren om details van de geselecteerde taken naar excel te exporteren. U kunt ook toegang tot de weergave van de lijst met taken vanuit de snelkoppeling beschikbaar op de dashboardpagina van Site Recovery. 

 Voor bewerkingen die u vanuit de Azure-portal uitvoert, kunnen de gemaakte taak en de huidige status ook worden gevolgd in de sectie meldingen (het belpictogram rechts bovenin) van de Azure portal.

## <a name="subscribe-to-email-notifications"></a>Abonneren op e-mailmeldingen

De ingebouwde e-mailmeldingsfunctie kunt u abonneren om te ontvangen van e-mailmeldingen voor kritieke gebeurtenissen. Als u zich hebt geabonneerd, worden e-mailmeldingen verzonden naar de volgende gebeurtenissen:
- De status van de replicatie van een replicatiepartner machine beïnvloeden in kritiek.
- Er is geen netwerkverbinding tussen de on-premises infrastructuuronderdelen en de Azure Site Recovery-service. De verbinding met de Site Recovery-service van de on-premises infrastructuuronderdelen zoals de configuratieserver (VMware) of System Center Virtual Machine Manager(Hyper-V) geregistreerd in de kluis is gedetecteerd met een heartbeat-mechanisme.
- Failover-bewerkingsfouten indien van toepassing.

Voor het ontvangen van e-mailmeldingen voor Azure Site Recovery abonneren, gaat de **bewaking en rapporten** gedeelte van het kluismenu en:
1. Selecteer waarschuwingen en gebeurtenissen > Site Recovery-gebeurtenissen.
2. Selecteer 'E-mailmeldingen' in het menu boven op de pagina voor gebeurtenissen die wordt geopend.
3. Gebruik de wizard e-mailmeldingen inschakelen of uitschakelen van e-mailmeldingen en selecteer ontvangers voor de meldingen. U kunt opgeven dat alle abonnementsbeheerders meldingen worden verzonden en/of geef een lijst met e-mailadressen u meldingen wilt verzenden. 