---
title: Bewaken en problemen oplossen van Azure Site Recovery | Microsoft Docs
description: Bewaken en problemen oplossen van problemen met Azure Site Recovery-replicatie en bewerkingen met behulp van de portal
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 84b5bf3be09083a69216802fc7f557de1a7f0ee6
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917530"
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>Bewaking en probleemoplossing van Azure Site Recovery

In dit artikel leert u over het gebruik van Azure Site Recovery in de ingebouwde controlefuncties voor controle en probleemoplossing. Leer hoe u het volgende doet:
> [!div class="checklist"]
> - Gebruik het dashboard van Azure Site Recovery (overzichtspagina kluis)
> - Bewaken en problemen oplossen van problemen met replicatie
> - Monitor voor Azure Site Recovery-taken/bewerkingen
> - Abonneren op e-mailmeldingen

## <a name="using-the-azure-site-recovery-dashboard"></a>Met behulp van de Azure Site Recovery-dashboard

De Azure Site Recovery-dashboard op de overzichtspagina van de kluis worden samengevoegd voor alle controlegegevens voor de kluis op één locatie. Start op het dashboard van de kluis en duik dieper voor meer informatie door te navigeren door de onderdelen van het dashboard. De belangrijkste onderdelen van het dashboard van Azure Site Recovery zijn als volgt:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Schakelen tussen Azure Backup en Azure Site Recovery dashboards

De schakeloptie aan de bovenkant van de overzichtspagina kunt u schakelen tussen de dashboard-pagina's voor Site Recovery en back-up. Deze selectie, eenmaal is gemaakt, wordt onthouden en standaard ingesteld op de volgende keer dat u de overzichtspagina voor de kluis opent. Selecteer de optie voor Site Recovery om te zien van de Site Recovery-dashboard. 

Elke 10 minuten, vernieuwd de verschillende onderdelen van de dashboardpagina van Azure Site Recovery automatisch zodat het dashboard de meest recente informatie geeft.

![Controle van functies in de overzichtspagina van Azure Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Gerepliceerde Items

De sectie gerepliceerde items van het dashboard geeft een overzicht van de replicatiestatus van de beveiligde servers in de kluis. 

<table>
<tr>
    <td>In orde</td>
    <td>Replicatie normaal gesproken wordt voortgezet voor deze servers en er wordt geen fout of waarschuwing problemen zijn gedetecteerd.</td>
</tr>
<tr>
    <td>Waarschuwing </td>
    <td>Een of meer waarschuwing problemen die mogelijk van invloed zijn op replicatie of aangeven dat replicatie normaal gesproken wordt niet voortgezet is aangetroffen voor deze servers.</td>
</tr>
<tr>
    <td>Kritiek</td>
    <td>Een of meer kritieke replicatie fout problemen zijn gedetecteerd voor deze servers. Deze fout-problemen zijn doorgaans indicatoren dat replicatie ofwel is vastgelopen of verloopt niet zo snel mogelijk de gegevenswijzigingssnelheid voor deze servers.</td>
</tr>
<tr>
    <td>Niet van toepassing</td>
    <td>Servers die momenteel worden niet worden gerepliceerd verwacht, zoals servers die een failover zijn.</td>
</tr>
</table>

Een lijst van beveiligde servers die zijn gefilterd op de replicatiestatus wilt bekijken, klikt u op de beschrijving van de status van replicatie naast de ring. De weergave die alle in de buurt van de sectietitel koppelen is een snelkoppeling naar de pagina met gerepliceerde items voor de kluis. Gebruik de weergave dat alle koppelen als u wilt zien van de lijst met alle servers in de kluis.

### <a name="3-failover-test-success"></a>3. Failovertest geslaagd

De failover testen geslaagd-sectie van het dashboard geeft een break-up van virtuele machines in de kluis op basis van status van de failovertest. 

<table>
<tr>
    <td>Test aanbevolen</td>
    <td>Virtuele machines waarvoor nog geen succesvolle failovertest sinds het moment dat ze een beveiligde status bereikt.</td>
</tr>
<tr>
    <td>Succesvol uitgevoerd</td>
    <td>Virtuele machines die u een of meer geslaagde testfailovers hebt.</td>
</tr>
<tr>
    <td>Niet van toepassing</td>
    <td>Virtuele machines die niet momenteel geschikt zijn voor een testfailover zijn. Voorbeelden zijn: servers een failover, servers voor waarvoor de initiële replicatie is uitgevoerd, servers waarvoor een failover uitgevoerd wordt, servers waarvoor een testfailover al uitgevoerd wordt.</td>
</tr>
</table>

Klik op de status van de failovertest naast de ring, dus het overzicht van de beveiligde servers op basis van hun status van de failovertest.
 
> [!IMPORTANT]
> Als een best practice is het raadzaam dat u een testfailover uitvoeren op uw beveiligde servers ten minste eenmaal na zes maanden. Wanneer een testfailover is een niet-storende manier voor het testen van failover van uw servers en toepassingen in een geïsoleerde omgeving, en helpt u bij het evalueren van uw zakelijke continuïteit voorbereidende maatregelen.

 Een testfailover op een server of een herstelplan wordt beschouwd als geslaagd pas als zowel de testfailover als de testfailover opschonen met succes zijn voltooid.

### <a name="4-configuration-issues"></a>4. Problemen met configuratie

De configuratiesectie van de problemen ziet u een lijst met problemen die mogelijk van invloed op uw mogelijkheid is failover-virtuele machines. De klassen van problemen die worden vermeld in deze sectie zijn:
 - **Configuraties ontbreekt:** beveiligde servers ontbreekt de vereiste configuratie-instellingen, zoals een herstelnetwerk of een herstel-resourcegroep.
 - **Ontbrekende resources:** doel/herstel resources niet gevonden of niet beschikbaar in het abonnement geconfigureerd. Bijvoorbeeld, de resource is verwijderd of is gemigreerd naar een ander abonnement of resourcegroep. De volgende configuraties voor doel/herstel worden gecontroleerd op beschikbaarheid: doelresourcegroep, virtueel netwerk en subnet, doel-/ storage-account, doel doelbeschikbaarheidsset, doel-IP-adres.
 - **Het abonnementsquotum:** quotum resourceverdeling van de beschikbare abonnementen wordt vergeleken met het saldo failover mogelijk moeten alle virtuele machines in de kluis. Als het beschikbare saldo onvoldoende wordt gevonden, wordt er onvoldoende quotum saldo gerapporteerd. Quota's voor de volgende Azure-resources worden bewaakt: core-aantal virtuele machines, virtuele machine-familie kerngeheugens, netwerkinterfaces (NIC)-kaart.
 - **Software-updates:** de beschikbaarheid van nieuwe software-updates, verlopen softwareversies.

Problemen met configuratie (met uitzondering van beschikbaarheid van software-updates), worden gedetecteerd door een bewerking periodieke validator die standaard wordt elke 12 uur wordt uitgevoerd. U kunt afdwingen dat de bewerking validatie van het direct uitvoeren door te klikken op het pictogram naast de *configuratieproblemen* sectiekop.

Klik op de koppelingen voor meer informatie over de vermelde problemen en virtuele machines beïnvloed door deze. Voor problemen die invloed hebben op specifieke virtuele machines, kunt u meer informatie krijgen door te klikken op de **aandacht** koppeling onder de doelkolom configuraties voor de virtuele machine. De details bevatten aanbevelingen over hoe u de gedetecteerde problemen kunt herstellen.

### <a name="5-error-summary"></a>5. Samenvatting van fouten

De samenvatting van de fout ziet u de momenteel actieve replicatie fout problemen die mogelijk van invloed op de replicatie van servers in de kluis, samen met het aantal betrokken entiteiten vanwege een fout.

De symptomen van de fout replicatie voor servers in een status Kritiek of waarschuwing replicatie kunnen worden weergegeven in de samenvattingstabel fout. 

- Fouten die invloed hebben op de on-premises infrastructuuronderdelen, zoals de niet-ontvangst van een heartbeat van de Azure Site Recovery Provider die wordt uitgevoerd op de on-premises configuratieserver, de VMM-server of de Hyper-V-host worden weergegeven aan het begin van de foutsamenvatting sectie
- Replicatie fout symptomen die invloed hebben op beveiligde servers wordt vervolgens weergegeven. De fout samenvattingstabel vermeldingen worden gesorteerd op aflopende volgorde van de ernst van de fout en vervolgens op aflopende volgorde van het aantal betrokken servers.
 

> [!NOTE]
> 
>  Meerdere replicatie fout problemen kunnen worden waargenomen op één server. Als er meerdere fout symptomen op één server zijn zou elke fout symptoom telt de aanroep die server in de lijst van de betrokken servers. Als het onderliggende probleem leidt tot een fout-probleem is opgelost, replicatie parameters verbeteren en de fout van de virtuele machine is uitgeschakeld.
>
> > [!TIP]
> Het aantal betrokken servers is een handige manier om te begrijpen als één onderliggende probleem mogelijk van invloed zijn op meerdere servers. Bijvoorbeeld, een netwerkfout opgetreden kan mogelijk gevolgen hebben voor alle servers die worden gerepliceerd van een on-premises site naar Azure. In deze weergave wordt snel geeft die verhelpen dat één onderliggende probleem replicatie voor meerdere servers wordt opgelost.
>

### <a name="6-infrastructure-view"></a>6. Infrastructuurweergave

De infrastructuurweergave bevat een scenario verstandig visuele weergave van de infrastructurele componenten betrokken bij de replicatie. Ook visueel ziet u de status van de verbinding tussen de verschillende servers en tussen de servers en de Azure-services die betrokken zijn bij de replicatie. 

Een groene lijn geeft aan dat er verbinding in orde is, is terwijl een rode lijn met de overlappende foutpictogram geeft aan dat het bestaan van een of meer fout symptomen die invloed hebben op de verbinding tussen de onderdelen die betrokken zijn. De muisaanwijzer de muisaanwijzer boven het pictogram van de fout op de regel bevat de fout en het aantal betrokken entiteiten. 

Te klikken op het foutpictogram ziet u een gefilterde lijst met betrokken entiteiten voor de fouten.

![Weergave van de infrastructuur voor site Recovery (kluis)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Zorg ervoor dat de on-premises infrastructuuronderdelen (configuratieserver, aanvullende processenservers te repliceren virtuele VMware-machines, Hyper-V-hosts en VMM-servers) de meest recente versie van Azure Site Recovery-software worden uitgevoerd. Als u de functies van de infrastructuurweergave te gebruiken, moet u actief [Update rollup 22](https://support.microsoft.com/help/4072852) of hoger voor Azure Site Recovery

Voor het gebruik van de infrastructuurweergave, selecteert u het juiste replicatiescenario (virtuele machines van Azure, VMware-machines of fysieke server of Hyper-V), afhankelijk van uw bronomgeving. De infrastructuurweergave die zijn gepresenteerd in de overzichtspagina van de kluis is een samengevoegde weergave voor de kluis. U kunt inzoomen verder naar de afzonderlijke onderdelen door te klikken op de vakken.

Een infrastructuur weergeven binnen het bereik van de context van een replicerende machine is beschikbaar op de overzichtspagina van het gerepliceerde item. Om te gaan naar de overzichtspagina voor een replicerende server, gaat u naar gerepliceerde items in het kluismenu en selecteer de server om de details voor te bekijken.

### <a name="infrastructure-view---faq"></a>Infrastructuurweergave - Veelgestelde vragen

**V:** Waarom zie ik de infrastructuurweergave voor mijn VM? </br>
**A:** De infrastructuur weergave-functie is alleen beschikbaar voor virtuele machines die naar Azure repliceren. De functie is momenteel niet beschikbaar voor virtuele machines die worden gerepliceerd tussen on-premises sites.

**V:** Waarom wordt het aantal virtuele machines in de kluis infrastructuurweergave verschilt van het totale aantal weergegeven in de ring gerepliceerde items?</br>
**A:** De weergave kluis infrastructuur is ingedeeld per replicatiescenario's. Alleen virtuele machines die deel uitmaken van de momenteel geselecteerde replicatiescenario worden opgenomen in het aantal virtuele machines die worden weergegeven in de infrastructuurweergave. Bovendien alleen virtuele machines die momenteel zijn geconfigureerd als u wilt repliceren naar Azure voor het geselecteerde scenario zijn opgenomen in het aantal virtuele machines die worden weergegeven in de infrastructuurweergave (Fo voorbeeld: back-virtuele machines repliceren van virtuele machines een failover, naar een on-premises site zijn niet opgenomen in de infrastructuurweergave.)

**V:** Waarom is het aantal gerepliceerde items worden weergegeven in de sectie essentials op de overzichtspagina verschilt van het totale aantal gerepliceerde items worden weergegeven in het ringdiagram op het dashboard?</br>
**A:** Alleen de virtuele machines voor waarvoor de initiële replicatie is voltooid, zijn opgenomen in het aantal die wordt weergegeven in de sectie essentials. De totale gerepliceerde items-ring bevat alle virtuele machines in de kluis en servers voor waarvoor de initiële replicatie wordt momenteel uitgevoerd.

**V:** Welke replicatiescenario's is de infrastructuurweergave beschikbaar voor? </br>
**A:**
>[!div class="mx-tdBreakAll"]
>|Replicatiescenario  | VM-status  | Infrastructuurweergave beschikbaar  |
>|---------|---------|---------|
>|Virtuele machines die worden gerepliceerd tussen twee on-premises sites     | -        | Nee      |
>|Alle     | Failover is uitgevoerd         |  Nee       |
>|Virtuele machines die worden gerepliceerd tussen twee Azure-regio 's     | Initiële replicatie wordt uitgevoerd of beveiligd         | Ja         |
>|Virtuele VMware-machines repliceren naar Azure     | Initiële replicatie wordt uitgevoerd of beveiligd        | Ja        |
>|Virtuele VMware-machines repliceren naar Azure     | Failover van virtuele machines die worden gerepliceerd naar een on-premises VMware-Site         | Nee        |
>|Hyper-V virtuele machines repliceren naar Azure     | Initiële replicatie wordt uitgevoerd of beveiligd        | Ja       |
>|Hyper-V virtuele machines repliceren naar Azure     | Failover / Failback in voortgang        |  Nee       |


### <a name="7-recovery-plans"></a>7. Plannen voor herstel

Het aantal plannen voor herstel zien het gedeelte abonnementen in de kluis. Klik op het getal in om te zien van de lijst met herstelplannen, nieuwe herstelplannen maken of bestaande relaties bewerken. 

### <a name="8-jobs"></a>8. Taken

Azure Site Recovery-taken bijhouden de status van Azure Site Recovery-bewerkingen. De meeste bewerkingen in Azure Site Recovery wordt asynchroon uitgevoerd met een tracering-taak die wordt gebruikt voor het bijhouden van de voortgang van de bewerking.  Zie voor meer informatie over het bewaken van de status van een bewerking, de [Monitor Azure Site Recovery-taken/bewerkingen](#monitor-azure-site-recovery-jobsoperations) sectie.

Deze sectie van de taken van het dashboard biedt de volgende informatie:

<table>
<tr>
    <td>Mislukt</td>
    <td>Kan de Azure Site Recovery-taken niet in de afgelopen 24 uur</td>
</tr>
<tr>
    <td>Wordt uitgevoerd</td>
    <td>Azure Site Recovery-taken die momenteel uitgevoerd worden</td>
</tr>
<tr>
    <td>Wachten op invoer</td>
    <td>Azure Site Recovery-taken die wachten op invoer van de gebruiker is momenteel onderbroken.</td>
</tr>
</table>

De weergave alle koppelen naast de sectiekop is een snelkoppeling naar de pagina van de lijst met taken.

## <a name="monitor-and-troubleshoot-replication-issues"></a>Bewaken en problemen oplossen van problemen met replicatie

Naast de informatie die beschikbaar zijn in de dashboardpagina van de kluis krijgt u extra informatie en informatie over probleemoplossing in de pagina van de lijst met virtuele machines en de pagina met details van de virtuele machine. U kunt de lijst met beveiligde virtuele machines in de kluis weergeven door het selecteren van de **gerepliceerde items** optie in het kluismenu. U kunt ook naar een gefilterde lijst van de beveiligde items ophalen door te klikken op een van de scoped snelkoppelingen die beschikbaar zijn op de dashboardpagina van de kluis.

![Site Recovery gerepliceerd weergave van de lijst met items](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

De filteroptie op de pagina van de lijst met gerepliceerde items kunt u verschillende filters, zoals de replicatiestatus en beleid voor wachtwoordreplicatie toepassen. 

De kolom selector-optie kunt u extra kolommen moeten worden weergegeven, zoals RPO, problemen met de doel-configuratie en replicatiefouten opgeven. U kunt bewerkingen op een virtuele machine of fouten weergeven die invloed hebben op de virtuele machine met de rechtermuisknop op een bepaalde rij van de lijst met computers op te starten.

Als u wilt verder inzoomen, selecteert u een virtuele machine door erop te klikken. Hiermee opent u de pagina met details van de virtuele machine. De overzichtspagina onder de details van de virtuele machine bevat een dashboard waar u vindt aanvullende gegevens met betrekking tot de machine. 

Op de overzichtspagina voor de replicerende machine vindt u het:
- RPO (recovery point objective): huidige RPO voor de virtuele machine en de tijd waarbinnen de RPO voor het laatst is berekend.
- Meest recente beschikbare herstelpunten voor de machine
- Configuratieproblemen als die mogelijk van invloed op de failover-gereedheid van de machine. Klik op de koppeling voor meer informatie.
- Details van fout: lijst met replicatie fout symptomen momenteel waargenomen op de machine samen met mogelijke oorzaken en aanbevolen herstelbewerkingen
- Gebeurtenissen: Een chronologische lijst met recente gebeurtenissen die invloed hebben op de machine. Details van deze fout ziet u de symptomen momenteel waarneembare fout op de machine, gebeurtenissen is een historisch overzicht van de diverse gebeurtenissen die mogelijk van invloed de machine met inbegrip van fout-problemen die zijn eerder voor de machine opgemerkt mogelijk.
- Infrastructuurweergave voor machines die repliceren naar Azure

![Site Recovery gerepliceerd item details/overzicht](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Het actiemenu aan de bovenkant van de pagina biedt opties voor het uitvoeren van verschillende bewerkingen zoals failover op de virtuele machine testen. De knop details van fout in het actiemenu kunt u zien van alle actieve fouten zoals replicatiefouten, problemen met configuratie en configuratie van aanbevolen procedures op basis van waarschuwingen voor de virtuele machine.

> [!TIP]
> Hoe verschilt RPO of recovery point objective van de meest recente beschikbare herstelpunt?
> 
>Azure Site Recovery maakt gebruik van een asynchroon proces van meerdere virtuele machines repliceren naar Azure. Recente wijzigingen in de virtuele machine samen met metagegevens worden in de voorlaatste stap van de replicatie gekopieerd naar een logboekbestand/cache-opslagaccount. Nadat deze wijzigingen samen met de code voor het identificeren van een punt hersteld naar het opslagaccount in de doelregio is geschreven, heeft Azure Site Recovery de informatie die nodig is voor het genereren van een herstelbare punt voor de virtuele machine. Het RPO heeft op dit moment is voldaan om de wijzigingen die zijn geüpload naar het opslagaccount dat tot nu toe. Met andere woorden, de RPO voor de virtuele machine op dit moment uitgedrukt in tijd, is gelijk aan hoeveel tijd verstrijkt tussen de tijdstempel die overeenkomt met het herstelbare punt.
>
>De Azure Site Recovery-service, die op de achtergrond, kiest de geüploade gegevens uit het opslagaccount en worden deze toegepast op de replicaschijven die zijn gemaakt voor de virtuele machine. Genereert een herstelpunt en het is nu beschikbaar voor herstel na een failover. De meest recente beschikbare herstelpunt geeft aan dat de tijdstempel die overeenkomt met de meest recente herstelpunt dat al is verwerkt en toegepast op de replicaschijven.
>> [!WARNING]
> Een ongelijke klok of onjuist systeemtijd op de replicerende bronmachine of de on-premises infrastructuurservers wordt een verkeerd de berekende RPO-waarde. Om ervoor te zorgen nauwkeurige rapportage van de RPO waarden ervoor te zorgen dat de systeemklok op de servers die betrokken zijn bij replicatie correct is. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>Monitor voor Azure Site Recovery-taken/bewerkingen

Azure Site Recovery wordt de bewerkingen die u asynchroon opgeeft uitgevoerd. Voorbeelden van bewerkingen die u kunt uitvoeren zijn replicatie inschakelen, herstelplan maken, testen van failover, replicatie-instellingen enz bijwerken. Elke deze bewerking heeft een bijbehorende taak die is gemaakt om te volgen en controleren van de bewerking. Het taakobject heeft alle benodigde informatie nodig hebt om de status en de voortgang van de bewerking te houden. U kunt de status van de verschillende bewerkingen van de Site Recovery voor de kluis op de pagina taken volgen. 

Overzicht van Site Recovery-taken voor de kluis gaat de **bewaking en rapporten** sectie van de kluismenu en selecteer taken > Site Recovery-taken. Selecteer een taak in de lijst met taken op de pagina door te klikken voor meer informatie over de gespecificeerde taak. Als een taak is nog niet voltooid of fouten heeft, kunt u meer informatie de fout en mogelijke herstel zien door te klikken op de knop van de details van fout aan de bovenkant van de pagina met details taak (ook toegankelijk is vanaf de pagina van de lijst met taken met de rechtermuisknop op de mislukte de taak.) U kunt gebruik de filteroptie in het actiemenu boven aan de pagina van de lijst met taken voor het filteren van de lijst op basis van een bepaald criterium voldoen en gebruik de knop exporteren om details van de geselecteerde taken naar excel exporteren. U kunt ook toegang tot de weergave van de lijst met taken van de snelkoppeling beschikbaar op de dashboardpagina van de Site Recovery. 

 Voor bewerkingen die u vanuit de Azure-portal uitvoert, kunnen de taak en de huidige status ook worden bijgehouden in de sectie meldingen (het belpictogram in de rechterbovenhoek) van de Azure-portal.

## <a name="subscribe-to-email-notifications"></a>Abonneren op e-mailmeldingen

De ingebouwde e-mailmeldingsfunctie kunt u abonneren om te ontvangen van e-mailmeldingen voor kritieke gebeurtenissen. Als u zich hebt geabonneerd, worden e-mailmeldingen worden verzonden voor de volgende gebeurtenissen:
- De status van de replicatie van een replicerende machine vernederen in kritiek.
- Er is geen connectiviteit tussen de onderdelen van de on-premises infrastructuur en de Azure Site Recovery-service. Verbinding met de Site Recovery-service van de on-premises infrastructuur-onderdelen, zoals de configuratieserver (VMware) of System Center Virtual Machine Manager(Hyper-V) geregistreerd bij de kluis is gedetecteerd met een heartbeat-mechanisme.
- Failover-bewerkingsfouten indien van toepassing.

Om u te abonneren voor het ontvangen van e-mailmeldingen voor Azure Site Recovery, gaat u de **bewaking en rapporten** gedeelte van het kluismenu en:
1. Waarschuwingen en gebeurtenissen selecteren > Site Recovery-gebeurtenissen.
2. Selecteer 'E-mailmeldingen' in het menu boven op de pagina gebeurtenissen dat wordt geopend.
3. Gebruik de wizard e-mailmelding inschakelen of uitschakelen van e-mailmeldingen en selecteer ontvangers voor meldingen. U kunt opgeven dat alle abonnementsbeheerders meldingen worden verzonden en/of geef een lijst met e-mailadressen u meldingen wilt verzenden. 
