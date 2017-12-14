---
title: Instellen van herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen van virtuele VMware-machines, of Windows- en Linux fysieke servers, naar een secundaire site met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauarvd
editor: 
ms.assetid: 68616d15-398c-4f40-8323-17b6ae1e65c0
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 9de0cb3d22a4e500b66640f70f936ae955bbadb8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Instellen van herstel na noodgevallen van on-premises VMware-virtuele machines of fysieke servers naar een secundaire site

InMage Scout in [Azure Site Recovery](site-recovery-overview.md) biedt realtime replicatie tussen on-premises VMware-sites. InMage Scout is opgenomen in de Azure Site Recovery-service-abonnementen. 


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- [Bekijk](site-recovery-support-matrix-to-sec-site.md) de ondersteuningsvereisten voor alle onderdelen.
- Zorg ervoor dat de machines die u wilt repliceren voldoen [gerepliceerde machine ondersteuning](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).


## <a name="create-a-vault"></a>Een kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="choose-a-protection-goal"></a>Kies een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik op **siteherstel** > **infrastructuur voorbereiden** > **beveiligingsdoel**.
2. Selecteer **met site recovery** > **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.
3. In **Scout Setup**, de InMage Scout 8.0.1 GA-software en de registratiesleutel downloaden. De setup-bestanden voor alle onderdelen zijn opgenomen in het gedownloade ZIP-bestand.

## <a name="download-and-install-component-updates"></a>Onderdeelupdates downloaden en installeren

 Bekijk en installeer de meest recente [updates](#updates). Updates moeten worden geïnstalleerd op servers in de volgende volgorde:

1. RX-server (indien van toepassing)
2. Van configuratieservers
3. Processervers
4. Het hoofddoel servers
5. vContinuum-servers
6. Bronserver (Windows en Linux-Servers)

Als volgt te werk om de updates te installeren:

> [!NOTE]
>Updateversie alle Scout-onderdelen mogelijk niet hetzelfde als in het ZIP-bijwerkbestand. De oudere versie aangeven dat er is geen wijziging in het onderdeel sinds de vorige update deze update.

Download de [bijwerken](https://aka.ms/asr-scout-update6) ZIP-bestand. Het bestand bevat de volgende onderdelen: 
  - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
  - CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe
  - UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
  - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
  - vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe
  - UA update4 bits voor RHEL5, OL5, OL6, SUSE 10, 11 SUSE: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
1. Pak het ZIP-bestanden.
2. **RX server**: kopie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** naar de server RX en pak het bestand. Voer in de uitgepakte map **/installeren**.
3. **Configuratieserver en de processerver**: kopie **CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe** voor de configuratieserver en de processerver. Dubbelklik op uit te voeren.<br>
4. **Windows hoofddoelserver**: kopiëren voor het bijwerken van de unified agent **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** naar de server. Dubbelklik erop uit te voeren. De unified agentupdate is ook van toepassing op de bronserver. Als de bron is niet bijgewerkt tot en met 4 bijwerken, moet u de unified agent bijwerken.
  De update niet wilt toepassen op de Hoofddoelserver doel voorbereid met **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** omdat dit nieuwe GA installatieprogramma met de meest recente wijzigingen.
5. **vContinuum-server**: kopie **vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe** naar de server.  Zorg ervoor dat u de vContinuum-wizard hebt gesloten. Dubbelklik op het bestand uit te voeren.
    De update niet wilt toepassen op de Hoofddoelserver voorbereid met **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** omdat dit nieuwe GA installatieprogramma met de meest recente wijzigingen.
6. **Server voor Linux-hoofddoel**: kopiëren voor het bijwerken van de unified agent **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar het hoofdniveau doelserver en pak het bestand. Voer in de uitgepakte map **/installeren**.
7. **Windows-bronserver**: kopiëren voor het bijwerken van de unified agent **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** naar de bronserver. Dubbelklik op het bestand uit te voeren. 
    U hoeft niet te installeren van de Update 5-agent op de bronserver als deze al is bijgewerkt naar Update 4 of bronagent is geïnstalleerd met de meest recente versie base installer **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe**.
8. **Linux-bronserver**: voor het bijwerken van de unified agent, de bijbehorende versie van de unified agent-bestand kopiëren naar de Linux-server en pakt. Voer in de uitgepakte map **/installeren**.  Voorbeeld: Voor RHEL 6,7 64-bits-server kopiëren **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de server en pak het bestand. Voer in de uitgepakte map **/installeren**.

## <a name="enable-replication"></a>Replicatie inschakelen

1. Instellen van de replicatie tussen de bron en doel van de VMware-sites.
2. Raadpleeg de volgende documenten voor meer informatie over de installatie, beveiliging en herstel:

   * [Releaseopmerkingen](https://aka.ms/asr-scout-release-notes)
   * [Compatibiliteit matrix](https://aka.ms/asr-scout-cm)
   * [Gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
   * [RX-gebruikershandleiding](https://aka.ms/asr-scout-rx-user-guide)
   * [Handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Updates

### <a name="site-recovery-scout-801-update-6"></a>Site Recovery Scout 8.0.1 Update 6 
Bijgewerkte: 12 oktober 2017

Download [Scout update 6](https://aka.ms/asr-scout-update6).

Scout Update 6 is een cumulatieve update. Deze bevat alle correcties van Update 1 voor Update 5 plus de nieuwe oplossingen en verbeteringen die hieronder worden beschreven. 

#### <a name="new-platform-support"></a>Nieuwe platformondersteuning
* Is er ondersteuning toegevoegd voor bron Windows Server 2016
* Is er ondersteuning toegevoegd voor de volgende Linux-besturingssystemen:
    - Red Hat Enterprise Linux (RHEL) 6,9
    - CentOS 6,9
    - Oracle Linux 5,11
    - Oracle Linux 6,8
* Is er ondersteuning toegevoegd voor VMware Center 6.5

> [!NOTE]
> * Base Agent(UA) Unified installer voor Windows is vernieuwd voor ondersteuning van Windows Server 2016. De nieuwe installer **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe** wordt verpakt met de base Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Hetzelfde installatieprogramma wordt gebruikt voor alle ondersteunde Windows-versie. 
> * Windows vContinuum & hoofddoel baseren installatieprogramma is vernieuwd voor ondersteuning van Windows Server 2016. De nieuwe installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** wordt verpakt met de base Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Hetzelfde installatieprogramma wordt gebruikt om Windows 2016 hoofddoel en Windows 2012R2 hoofddoel te implementeren.
> * Het pakket GA downloaden vanuit de portal, zoals beschreven in [een kluis maken](#create-a-vault).
>

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen
- Failback beveiliging mislukt voor Linux-VM met de lijst met schijven worden gerepliceerd aan het einde van het configuratiebestand leeg is.

### <a name="site-recovery-scout-801-update-5"></a>Site Recovery Scout 8.0.1 Update 5
Scout Update 5 is een cumulatieve update. Het bevat alle correcties uit Update 1 tot en met Update 4 en de nieuwe oplossingen die hieronder worden beschreven.
- Hotfixes van Site Recovery Scout Update 4 tot en met Update 5 zijn specifiek voor de master doel en de vContinuum-onderdelen.
- Als bronservers, het hoofddoel, configuratie, proces en RX-servers zijn Update 4 al wordt uitgevoerd, gaat u deze alleen op de hoofddoelserver toe te passen. 

#### <a name="new-platform-support"></a>Nieuwe platformondersteuning
* SUSE Linux Enterprise Server 11 4(SP4) servicepack
* SLES 11 SP4 64 bits **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** wordt verpakt met de base Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA.zip**). Het pakket GA downloaden vanuit de portal, zoals beschreven in [een kluis maken](#create-a-vault).


#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen

* Oplossingen voor verbeterde cluster van Windows bieden ondersteuning voor betrouwbaarheid:
    * Vaste-sommige van de P2V-MSCS clusterschijven worden RAW na het herstel.
    * Fixed-P2V MSCS-cluster herstellen mislukt vanwege een niet-overeenkomend schijf volgorde.
    * De MSCS vaste - bewerking voor het toevoegen van schijven mislukt met een incompatibel fout van de grootte van het cluster.
    * Vaste de gereedheidscontrole voor de bron MSCS-cluster met de toewijzing van RDM LUN's in grootte-verificatie is mislukt.
    * Beveiliging van de cluster Fixed-één knooppunt mislukt vanwege een probleem met de SCSI-verschil. 
    * Opnieuw Fixed-beveiliging van de P2V-Windows clusterserver mislukt als doel clusterschijven aanwezig zijn. 
    
* Vaste: Tijdens de failback beveiliging, als de doelserver van het geselecteerde model niet op dezelfde server als de beveiligde bronmachine ESXi (tijdens forward beveiliging) en vervolgens de verkeerde hoofddoelserver vContinuum tijdens failback herstel en het herstel opneemt bewerking is mislukt.

> [!NOTE]
> * De P2V-cluster-oplossingen zijn alleen van toepassing op fysieke MSCS-clusters die nieuw zijn beveiligd met Site Recovery Scout Update 5. Voor het installeren van de cluster-oplossingen op beveiligde P2V MSCS-clusters met oudere updates, volgt u de upgrade stappen die worden vermeld in de sectie 12 van de [Site Recovery Scout Release-opmerkingen](https://aka.ms/asr-scout-release-notes).
> * Als op het moment van beveiliging opnieuw dezelfde set met schijven actief op elk van de clusterknooppunten zijn als ze in eerste instantie beveiligd waren, vervolgens opnieuw de beveiliging van een fysiek MSCS-cluster kan alleen opnieuw gebruiken bestaande doel-schijven. Als dit niet het geval is, voert u de handmatige stappen in de sectie 12 van [Site Recovery Scout Release-opmerkingen](https://aka.ms/asr-scout-release-notes), het doel aan clientzijde schijven verplaatsen naar het pad juist gegevensarchief voor gebruik tijdens het opnieuw beveiligen. Als u de MSCS-cluster in de P2V-modus zonder dat de Upgradestappen beveiligt, maakt een nieuwe schijf op de doelserver ESXi. U moet de oude schijven handmatig verwijderen van het gegevensarchief.
> * Wanneer een bron SLES11 of SLES11 (waarbij alle servicepacks)-server opnieuw is gestart zonder problemen vervolgens handmatig aangeven dat de **hoofdmap** schijf replicatie paren voor hernieuwde synchronisatie. Er is geen melding in de CX-interface. Als u niet de hoofdmap schijf voor hersynchronisatie markeert, ziet u mogelijk problemen met de gegevensintegriteit.


### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 is een cumulatieve update. Dit omvat alle oplossingen van Update 1 naar Update 3 en de nieuwe oplossingen die hieronder worden beschreven.

#### <a name="new-platform-support"></a>Nieuwe platformondersteuning

* Is er ondersteuning toegevoegd voor de vCenter/vSphere 6.0, 6.1 en 6.2
* Is er ondersteuning toegevoegd voor deze Linux-besturingssystemen:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 en 7.2
  * CentOS 7.0, 7.1 en 7.2
  * Red Hat Enterprise Linux (RHEL) 6,8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bits **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wordt verpakt met de base Scout GA pakket **InMage_Scout_Standard_8.0.1 GA.zip**. Download het pakket Scout GA via de portal zoals beschreven in [een kluis maken](#create-a-vault).

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen

* Verbeterde verwerking voor de volgende Linux-besturingssystemen en klonen om ongewenste hernieuwde synchronisatieproblemen te voorkomen wordt afgesloten:
    * Red Hat Enterprise Linux (RHEL) 6.x
    * Oracle Linux (OL) 6.x
* Voor Linux worden alle map toegangsmachtigingen in de installatiemap van de unified agent nu beperkt tot alleen de lokale gebruiker.
* In Windows, een oplossing voor een time-out optreedt probleem dat is opgetreden bij het verlenen van algemene gedistribueerde consistentie bladwijzers belast op zwaar gedistribueerde toepassingen zoals SQL Server en SharePoint-clusters.
* Een oplossing voor gerelateerde logboekbestanden in de base installatieprogramma van de configuratie.
* Een koppeling naar VMware vCLI 6.0 downloadlocatie is toegevoegd aan het Windows-installatieprogramma baseren op het hoofddoel.
* Extra controles en de logboeken zijn toegevoegd voor wijzigingen tijdens de failover- en disaster recovery zoomt netwerkconfiguratie.
* Een oplossing voor een probleem waardoor bewaren van gegevens niet te worden gerapporteerd aan de configuratieserver.  
* Voor fysieke clusters, een oplossing voor een probleem dat volume de grootte van mislukken in de wizard vContinuum bij het verkleinen van het bronvolume veroorzaakt.
* Een oplossing voor een cluster beveiliging probleem dat is mislukt met fout: 'Is mislukt voor het vinden van de schijfhandtekening' wanneer de clusterschijf is een schijf PRDM.
* Een oplossing voor cxps transport server vastlopen van een veroorzaakt door een uitzondering van buiten het bereik.
* Naam en IP-adres kolommen zijn nu formaat kunt wijzigen in de **Push-installatie** pagina van de vContinuum-wizard.
* RX-API-uitbreidingen voor:
  * De vijf meest recente beschikbare algemene consistentie wijst nu beschikbaar (alleen gegarandeerd labels).
  * Capaciteit en beschikbare ruimte details worden weergegeven voor alle beveiligde apparaten.
  * Status van de Scout stuurprogramma op de bronserver is beschikbaar.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** base pakket heeft:
    * Het installatieprogramma voor een basis van een bijgewerkte configuratie server (**InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe**)
    * Een Windows hoofddoel base installer (**InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**).
    * Gebruik de nieuwe configuratieserver en Windows hoofddoel GA bits voor alle nieuwe installaties.
> * Update 4 kan worden toegepast op 8.0.1 rechtstreeks algemene beschikbaarheid.
> * De configuratieserver en RX updates kunnen niet worden hersteld nadat ze zijn toegepast.


### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 Update 3

Alle Site Recovery-updates zijn cumulatief. Update 3 bevat alle correcties uit Update 1 en 2 van de Update. Update 3 kan rechtstreeks worden toegepast op 8.0.1 algemene beschikbaarheid. De configuratieserver en RX updates kunnen niet worden hersteld nadat ze zijn toegepast.

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen
Update 3 worden de volgende problemen:

* De configuratieserver en RX worden niet geregistreerd in de kluis wanneer ze zich achter de proxy.
* Het aantal uren waarin het beoogde herstelpunt (RPO) niet is bereikt, is niet bijgewerkt in het statusrapport.
* De configuratieserver wordt niet gesynchroniseerd met RX wanneer de gegevens van de hardware ESX of details van het netwerk, UTF-8 tekens bevatten.
* Windows Server 2008 R2-domeincontrollers starten niet na het herstel.
* Offlinesynchronisatie werkt niet zoals verwacht.
* Na een failover van de virtuele machine niet verwijderen van een combinatie van replicatie de voortgang in de configuratieconsole van de server gedurende een lange periode. Gebruikers kunnen niet de failback voltooien of te hervatten bewerkingen.
* De algemene door de consistentietaak van de momentopname zijn geoptimaliseerd, verbreekt zoals SQL Server-clients te reduceren van de toepassing.
* Consistentie hulpprogramma (VACP.exe) prestaties zijn verbeterd. Geheugengebruik is vereist voor het maken van momentopnamen op Windows, is verminderd.
* De push-bewerking installeren service loopt vast als het wachtwoord is groter dan 16 tekens.
* vContinuum niet controleren en vragen om nieuwe vCenter referenties als referenties worden gewijzigd.
* Op Linux, is niet hoofddoel Cachebeheer (cachemgr) downloaden van bestanden vanaf de processerver. Dit leidt tot replicatie paar beperking.
* Als u de volgorde van fysieke failover cluster (MSCS) schijf niet hetzelfde zijn op alle knooppunten, moet replicatie is niet ingesteld voor enkele van de clustervolumes. Het cluster moet opnieuw worden beveiligd om te profiteren van deze oplossing.  
* SMTP-functionaliteit werkt niet zoals verwacht, nadat RX Scout 7.1 is bijgewerkt naar Scout 8.0.1.
* Meer statistieken zijn toegevoegd in het logboek voor de bewerking ongedaan maken om bij te houden hoeveel tijd het duurt om deze te voltooien.
* Is er ondersteuning toegevoegd voor Linux-besturingssystemen op de bronserver:
  * Red Hat Enterprise Linux (RHEL) 6 update 7
  * 6 van centOS bijwerken 7
* De configuratieserver en RX consoles nu meldingen weergeven voor het paar, probeert het bitmapmodus.
* De volgende oplossingen zijn in RX toegevoegd:
    * Autorisatie via parameter knoeien negeren: toegang beperkt tot gebruikers die niet van toepassing zijn.
    * Aanvraagvervalsing op meerdere sites: het concept van pagina-token is geïmplementeerd en er wordt willekeurig gegenereerd voor elke pagina. Dit betekent dat er slechts één aanmelden exemplaar voor dezelfde gebruiker en pagina vernieuwen niet werkt. In plaats daarvan wordt hij omgeleid naar het dashboard.
    * Schadelijk bestand uploaden: bestanden zijn beperkt tot specifieke extensies: z, aiff, AVP, avi, bmp, csv, doc, docx, fla, FLV-, gif, gz, gzip, JPEG-, jpg, logboek mid mov, mp3, mp4, mpc, mpeg, mpg, ods odt, PDF-, png, ppt, pptx, pxd, qt, ram, rar, DB, rmi, rmvb, RTF-indeling , sdc, sitd, swf, sxc, sxw, tar, tgz, tif, TIFF-, txt, vsd, wav, wma, wmv, xls, xlsx, xml en zip.
    * Permanente cross-site scripting: Input-validaties zijn toegevoegd.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (03 december 15 Update)

Verbeteringen in Update 2 zijn onder andere:

* **Configuratieserver**: problemen waardoor 31 dagen gratis softwarelicentiecontrole functie werkt zoals verwacht wordt bij de configuratieserver is geregistreerd in Site Recovery.
* **Unified agent**: los van een probleem in Update 1 dat heeft geresulteerd in de update niet wordt tijdens de upgrade van versie 8.0-8.0.1 meer op de hoofddoelserver geïnstalleerd.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Update 1 bevat de volgende oplossingen voor problemen en nieuwe functies:

* 31 dagen van gratis bescherming per serverexemplaar. Hiermee kunt u functionaliteit testen, of een bewijs van concept instellen.
* Alle bewerkingen op de server, inclusief failover en failback, zijn beschikbaar voor de eerste 31 dagen. De tijd wordt gestart wanneer een server eerst is beveiligd met Site Recovery Scout. Vanaf de 32nd dag elke beveiligde server is in rekening gebracht standaard exemplaar voor de Site Recovery-beveiliging met een site die eigendom zijn van de klant.
* Het aantal beveiligde servers die momenteel worden kosten in rekening gebracht op elk gewenst moment is beschikbaar op de **Dashboard** in de kluis.
* Ondersteuning is toegevoegd voor vSphere opdrachtregelinterface (vCLI) 5.5 Update 2.
* Er is ondersteuning toegevoegd voor deze Linux-besturingssystemen op de bronserver:
    * RHEL 6 Update 6
    * RHEL 5 11 bijwerken
    * CentOS 6 Update 6
    * CentOS 5 Update 11
* Oplossingen voor het oplossen van de volgende problemen:
  * Het registreren van de kluis is mislukt voor de configuratieserver of RX-server.
  * Clustervolumes worden niet weergegeven als verwachte wanneer geclusterde dat virtuele machines worden opnieuw beveiligd als ze weer.
  * Failback mislukt als de hoofddoelserver wordt gehost op een andere ESXi-server van de lokale productie virtuele machines.
  * Bestandsmachtigingen configuratie worden gewijzigd wanneer u een naar 8.0.1 upgrade. Deze wijziging beveiliging en bewerkingen.
  * De hersynchronisatie-drempelwaarde niet afgedwongen zoals verwacht, waardoor replicatie inconsistent gedrag.
  * De RPO-instellingen weergegeven niet correct in de configuratieconsole van de server. De waarde niet-gecomprimeerde gegevens geeft ten onrechte de gecomprimeerde waarde.
  * De verwijderbewerking niet zoals verwacht in de wizard vContinuum verwijderen en replicatie is niet verwijderd uit de configuratieconsole van de server.
  * In de wizard vContinuum de schijf is automatisch uitgeschakeld wanneer u klikt op **Details** in de weergave schijf tijdens het beveiligen van virtuele machines MSCS.
  * Vereiste HP-services (zoals CIMnotify en CqMgHost) worden niet in het scenario fysiek naar virtueel (P2V) verplaatst naar de handleiding bij het herstellen van de virtuele machine. Dit probleem resulteert in extra opstarten.
  * Linux-VM beveiliging mislukt wanneer er meer dan 26 schijven op de hoofddoelserver zijn.

