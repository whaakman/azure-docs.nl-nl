---
title: Instellen van herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar een secundaire site | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe lokale VMware-machines of Windows of Linux-fysieke servers repliceren naar een secundaire site, met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2017
ms.author: raynew
ms.openlocfilehash: 8cfaa56735c1f4e2e01b58fdde2ad0e77b388762
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-disaster-recovery-of-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Instellen van herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar een secundaire site


InMage Scout in Azure Site Recovery biedt realtime replicatie tussen on-premises VMware-sites. InMage Scout is opgenomen in de Azure Site Recovery-service-abonnementen.

Als u een Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/pricing/free-trial/) voordat u begint.


## <a name="create-a-vault"></a>Een kluis maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/) > **Recovery Services**.
2. Klik op Nieuw > Beheer > back-up en siteherstel (OMS). U kunt ook klikken op Bladeren > Recovery Services-kluis > toevoegen.
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u een van uw abonnementen.
4. In **resourcegroep** een nieuwe resourcegroep maken of een bestaande set selecteren. Geef een Azure-regio voor het voltooien van de vereiste velden.
5. In **locatie**, selecteer de geografische regio voor de kluis. Ondersteunde regio's, Zie [prijzen van Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Als u wilt voor snel toegang tot de kluis vanuit het Dashboard vastmaken aan dashboard en klik op maken.
7. De nieuwe kluis wordt weergegeven op het Dashboard > alle resources en op de belangrijkste Recovery Services-kluizen pagina.

## <a name="configure-the-vault-and-download-inmage-scout-components"></a>Configureren van de kluis en InMage Scout onderdelen downloaden
1. De Recovery Services kluizen pagina, selecteer uw kluis en klik op **instellingen**.
2. In **instellingen** > **aan de slag** klikt u op **siteherstel** > stap 1: **infrastructuur voorbereiden**  >  **Beveiligingsdoel**.
3. In **beveiligingsdoel** Selecteer met site recovery en selecteer Ja, met VMware vSphere-Hypervisor. Klik vervolgens op OK.
4. In **Scout setup**, klik op downloaden om te downloaden InMage Scout 8.0.1 GA software en registratie-sleutel. De setup-bestanden voor alle vereiste onderdelen zijn in het gedownloade ZIP-bestand.

## <a name="step-3-install-component-updates"></a>Stap 3: Component updates installeren
Meer informatie over de meest recente [updates](#updates). Installeert u de updatebestanden op servers in de volgende volgorde:

1. RX-server, indien van toepassing
2. Van configuratieservers
3. Processervers
4. Hoofddoelservers
5. vContinuum-servers
6. Bronserver (Windows en Linux-Server)

Als volgt te werk om de updates te installeren:

1. Download de [bijwerken](https://aka.ms/asr-scout-update5) ZIP-bestand. Dit zip-bestand bevat de volgende bestanden:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * UA update4 bits voor RHEL5, OL5, OL6, SUSE 10, 11 SUSE: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Pak het ZIP-bestanden.<br>
3. **Voor de server RX**: kopie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** naar de server RX en pak het bestand. Voer in de uitgepakte map **/installeren**.<br>
4. **Voor de server-proces configuratieserver**: kopie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** voor de configuratieserver en de processerver. Dubbelklik op uit te voeren.<br>
5. **Voor de Windows-hoofddoelserver**: kopiëren voor het bijwerken van de unified agent **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** op de hoofddoelserver. Dubbelklik erop uit te voeren. Houd er rekening mee dat de unified agent ook van toepassing op de bronserver is als bron niet tot Update4 bijgewerkt is. U moet deze installeren op de bronserver en, zoals verderop in deze lijst worden vermeld.<br>
6. **Voor de vContinuum-server**: kopie **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** naar de vContinuum-server.  Zorg ervoor dat u de vContinuum-wizard hebt gesloten. Dubbelklik op het bestand uit te voeren.<br>
7. **Voor de Linux-hoofddoelserver**: kopiëren voor het bijwerken van de unified agent **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar het hoofdniveau doelserver en pak het bestand. Voer in de uitgepakte map **/installeren**.<br>
8. **Voor de bronserver Windows**: U hoeft niet te installeren Update 5-agent op de bron-als Update 4 het al wordt uitgevoerd. Als er minder dan Update 4 wordt uitgevoerd, de Update 5-agent van toepassing.
Voor het bijwerken van de unified agent kopiëren **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** naar de bronserver. Dubbelklik erop uit te voeren. <br>
9. **Voor de Linux-bronserver**: voor het bijwerken van de unified agent bijbehorende versie van UA bestand kopiëren naar de Linux-server en pak het bestand. Voer in de uitgepakte map **/installeren**.  Voorbeeld: Voor RHEL 6,7 64-bits-server kopiëren **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de server en pak het bestand. Voer in de uitgepakte map **/installeren**.

## <a name="step-4-set-up-replication"></a>Stap 4: De replicatie instellen
1. Instellen van de replicatie tussen de bron en doel van de VMware-sites.
2. Gebruik de InMage Scout documentatie die gedownload met het product voor instructies. U kunt ook de documentatie als volgt openen:

   * [Releaseopmerkingen](https://aka.ms/asr-scout-release-notes)
   * [Compatibiliteit matrix](https://aka.ms/asr-scout-cm)
   * [Gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
   * [RX-gebruikershandleiding](https://aka.ms/asr-scout-rx-user-guide)
   * [Handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Updates
### <a name="azure-site-recovery-scout-801-update-5"></a>Azure Site Recovery Scout 8.0.1 Update 5
Scout Update 5 is een cumulatieve update. Heeft de oplossingen van update1 tot update4 en volgende nieuwe oplossingen voor problemen en verbeteringen.
Oplossingen die van Site Recovery Scout update4 worden toegevoegd aan update5 zijn specifiek voor het hoofddoel en vContinuum-onderdelen. Als u al uw bronservers, hoofddoel, configuratieserver, processerver en RX zich al op de Site Recovery Scout update4 moet u update 5 toepassing alleen op de hoofddoelserver. 

**Nieuwe platformondersteuning**
* SUSE Linux Enterprise Server 11 4(SP4) servicepack

> [!NOTE]
> SLES 11 SP4 64 bits **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** wordt verpakt met base Scout GA pakket **InMage_Scout_Standard_8.0.1 GA.zip**. Download Scout GA pakket van de portal, zoals vermeld in [stap 1](#step-1-create-a-vault).
>

**Oplossingen voor problemen en verbeteringen**

* Verbetering van de betrouwbaarheid van Windows Cluster-ondersteuning
    * Vaste Sometime enkele van de P2V-MSCS clusterschijven worden RAW na herstel
    * Herstel van Fixed-P2V MSCS-cluster is mislukt omdat de schijf order niet overeenkomen
    * Fixed-MSCS-cluster toevoegen schijven bewerking is mislukt met de schijf komt niet overeen
    * Fixed-bron MSCS-cluster met de gereedheidscontrole toewijzing van RDM LUN's in grootte-verificatie is mislukt
    * Beveiliging van de cluster Fixed-één knooppunt mislukt als gevolg van SCSI-verschil probleem 
    * Fixed-opnieuw beveiligen van de P2V-Windows clusterserver mislukt als doel clusterschijven aanwezig zijn. 
    
* Tijdens de failback beveiliging als geselecteerde MT niet op dezelfde server als die van de beveiligde bronmachine ESXi (tijdens het doorsturen van beveiliging is), de verkeerde MT vContinuum tijdens Failback herstel opneemt en vervolgens de herstelbewerking mislukt.

> [!NOTE]
> 
> * Hierboven P2V cluster oplossingen van toepassing zijn op alleen de fysieke MSCS-clusters die zijn opnieuw worden beveiligd met Site Recovery Scout update5. Gebruik van het cluster worden opgelost op het al beveiligde P2V MSCS-cluster met oudere updates, moet u de upgrade stappen die worden vermeld in de sectie 12, Upgrade beveiligd P2V MSCS-clusters op Scout Update 5 van de [release-opmerkingen](https://aka.ms/asr-scout-release-notes) .
> 
> * Opnieuw beveiligen van fysieke MSCS-cluster kunt bestaande doel schijven hergebruiken alleen als op het moment van opnieuw beveiliging, dezelfde set met schijven zijn actief is op elk van de clusterknooppunten als ze in eerste instantie beveiligd waren. Als niet zo is, klikt u vervolgens er handmatige stappen zoals vermeld in de sectie 12 van de [release-opmerkingen](https://aka.ms/asr-scout-release-notes) de doel-zijde schijven verplaatsen naar het pad juist gegevensarchief opnieuw te gebruiken tijdens het opnieuw beveiligen. Als u de MSCS-cluster in de modus voor P2V beveiligt zonder de volgende stappen de upgrade wordt het nieuwe schijf gemaakt op de doelserver ESXi. U moet de oude schijven handmatig verwijderen uit het gegevensarchief.
> 
> * Wanneer de gegevensbron SLES11 of SLES11 met een andere service pack server probleemloos opnieuw wordt opgestart, wordt een moet handmatig worden gemarkeerd de **hoofdmap** paren van replicatie voor het opnieuw synchroniseren schijf zoals deze wordt niet gewaarschuwd in CX UI. Als u niet de hoofdmap schijf voor resync markeert, ziet u mogelijk problemen met de gegevensintegriteit (DI).
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 is een cumulatieve update. Heeft de oplossingen van update1 tot update3 en volgende nieuwe oplossingen voor problemen en verbeteringen.

**Nieuwe platformondersteuning**

* Is er ondersteuning toegevoegd voor de vCenter/vSphere 6.0, 6.1 en 6.2
* Is er ondersteuning toegevoegd voor de volgende Linux-besturingssystemen
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 en 7.2
  * CentOS 7.0, 7.1 en 7.2
  * Red Hat Enterprise Linux (RHEL) 6,8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bits **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wordt verpakt met base Scout GA pakket **InMage_Scout_Standard_8.0.1 GA.zip**. Download Scout GA pakket van de portal, zoals vermeld in [stap 1](#step-1-create-a-vault).
>
>

**Oplossingen voor problemen en verbeteringen**

* Verbeterde verwerking voor de volgende Linux OSes en klonen te voorkomen dat problemen ongewenste opnieuw synchroniseren is afgesloten.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Voor Linux, toegang tot een map de machtigingen in de installatiemap van de unified agent nu alleen naar de lokale gebruiker beperkt zijn te voltooien.
* In Windows geladen probleem time-out tijdens het uitgeven van de algemene gedistribueerde consistentie bladwijzer sterk op gedistribueerde toepassingen zoals SQL en SharePoint-clusters.
* Toegevoegde logboek gerelateerde herstellen in base CX-installatieprogramma.
* VMware vCLI 6.0 downloadkoppeling wordt toegevoegd aan Windows hoofddoel base installatieprogramma.
* Meer controle en logboeken voor wijzigingen in het netwerk configuraties tijdens failover en Noodherstel zoomt toegevoegd.
* Sometime bewaren van gegevens is niet gerapporteerd aan de CX.  
* Voor fysieke cluster mislukt volume bewerking formaat wijzigen via de wizard vContinuum als de bron volume verkleinen plaatsvindt.
* Cluster-beveiliging is mislukt met fout 'Is mislukt voor het vinden van de schijfhandtekening' wanneer clusterschijf PRDM schijf is.
* cxps transport server vastlopen vanwege uitzondering van buiten het bereik.
* Servernaam en IP-kolommen zijn nu formaat kunt wijzigen in push installeren pagina van de vContinuum-wizard.
* RX API-uitbreidingen
  * Biedt de vijf meest recente beschikbare algemene consistentiepunten (alleen gegarandeerd-tags).
  * Capaciteit en details van de vrije ruimte biedt voor alle beveiligde apparaten.
  * Biedt Scout stuurprogramma status op de bronserver.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** base pakket heeft nu bijgewerkte CX base installatieprogramma **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** en base hoofddoel Windows-installatieprogramma **InMage_ Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Gebruik voor alle nieuwe installatie nieuwe CX- en Windows-hoofddoel GA-bits.
> * Update 4 rechtstreeks kan worden toegepast op 8.0.1 algemene beschikbaarheid.
> * De configuratieserver en RX updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 Update 3
Update 3 bevat de volgende oplossingen voor problemen en de volgende verbeteringen:

* De configuratieserver en RX niet registreren bij de Site Recovery-kluis wanneer ze zich achter de proxy.
* Het aantal uren dat het beoogde herstelpunt (RPO) nog niet is voldaan, wordt niet bijgewerkt in het statusrapport.
* De configuratieserver wordt niet gesynchroniseerd met RX wanneer de gegevens van de hardware ESX of netwerkdetails UTF-8 tekens bevatten.
* Windows Server 2008 R2-domeincontrollers niet kunnen worden opgestart na het herstel.
* Offlinesynchronisatie werkt niet zoals verwacht.
* Na een failover van de virtuele machine (VM), replicatie paar verwijdering vast gedurende een lange periode in de CX-UI en gebruikers niet de failback voltooien of hervatten.
* De algemene verbroken momentopname bewerkingen die worden uitgevoerd door de consistentietaak te reduceren van toepassing zijn geoptimaliseerd zoals SQL-clients.
* De prestaties van het hulpprogramma consistentiecontrole (VACP.exe) is vermindert het gebruik van het geheugen dat is vereist voor het maken van momentopnamen op Windows verbeterd.
* De push-bewerking installeren service loopt vast als het wachtwoord is groter dan 16 tekens.
* vContinuum is niet controleren en te vragen om nieuwe vCenter referenties wanneer de referenties worden gewijzigd.
* Op Linux, is hoofddoel Cachebeheer (cachemgr) niet downloaden van bestanden vanaf de processerver, wat tot replicatie paar beperking leidt.
* Wanneer u de volgorde fysieke failover cluster (MSCS)-schijf is niet hetzelfde zijn op alle knooppunten, wordt replicatie is niet ingesteld voor enkele van de clustervolumes.
  <br/>Houd er rekening mee dat het cluster opnieuw worden beveiligd moet om te profiteren van deze oplossing.  
* SMTP-functionaliteit werkt niet zoals verwacht na RX Scout 7.1 is bijgewerkt naar Scout 8.0.1.
* Meer statistieken zijn toegevoegd in het logboek voor de terugdraaibewerking voor het bijhouden van de tijd die nodig is om deze te voltooien.
* Is er ondersteuning toegevoegd voor Linux-besturingssystemen op de bronserver:
  * Red Hat Enterprise Linux (RHEL) 6 update 7
  * 6 van centOS bijwerken 7
* De CX en RX-gebruikersinterface kunt u nu de melding voor het paar, probeert het bitmapmodus weergeven.
* De volgende oplossingen zijn in RX toegevoegd:

| **Beschrijving van probleem** | **Procedures voor implementatie** |
| --- | --- |
| Autorisatie via parameter knoeien overslaan |Beperkte toegang tot gebruikers die niet van toepassing zijn. |
| Aanvraagvervalsing op meerdere sites |Het concept van pagina-token, dat wordt willekeurig gegenereerd voor elke pagina geïmplementeerd. <br/>Hiermee ziet u het: <li> Er is slechts één aanmelden exemplaar voor dezelfde gebruiker.</li><li>Pagina vernieuwen werkt niet--het omleidt naar het dashboard.</li> |
| Schadelijk bestand uploaden |Bestanden met beperkte toegang tot bepaalde uitbreidingen. Extensies zijn toegestaan: 7z, aiff, AVP avi, bmp, csv, document, docx, fla, FLV-, gif, gz, gzip, JPEG-, jpg, logboek mid mov, mp3, mp4, mpc, mpeg, mpg, ods odt, PDF-, png, ppt, pptx, pxd, qt, ram, rar, DB, rmi, rmvb, RTF-, sdc, sitd, swf, sxc, sxw, tar , tgz, tif-, TIFF-, txt, vsd, wav, wma, wmv, xls, xlsx, xml en zip. |
| Permanente cross-site scripting |Input-validaties toegevoegd. |

> [!NOTE]
> * Alle Site Recovery-updates zijn cumulatief. Update 3 heeft de oplossingen van Update 1 en 2 van de Update. Update 3 kan rechtstreeks worden toegepast op 8.0.1 algemene beschikbaarheid.
> * De configuratieserver en RX updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (03 december 15 Update)
Verbeteringen in Update 2 zijn onder andere:

* **Configuratieserver**: Fix voor een probleem dat de functie 31 dagen gratis softwarelicentiecontrole niet werken zoals verwacht wanneer de configuratieserver is geregistreerd in Site Recovery.
* **Unified agent**: voor een probleem in Update 1 dat heeft geresulteerd in de update niet wordt geïnstalleerd op de hoofddoelserver wanneer deze is bijgewerkt van versie 8.0-8.0.1 oplossen.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Update 1 bevat de volgende oplossingen voor problemen en nieuwe functies:

* 31 dagen van gratis bescherming per serverexemplaar. Hiermee kunt u de functionaliteit testen of een bewijs van concept instellen.
  * Alle bewerkingen op de server, inclusief failover en failback, zijn beschikbaar voor de eerste 31 dagen vanaf het moment dat een server eerst is beveiligd met Site Recovery Scout.
  * Na de 32nd en hoger, worden elke beveiligde server op het standaard exemplaar tarief voor de Azure Site Recovery-beveiliging met een site die eigendom zijn van de klant berekend.
  * Het aantal beveiligde servers die zijn momenteel in rekening worden gebracht is op elk gewenst moment beschikbaar zijn op de pagina Dashboard van de Azure Site Recovery-kluis.
* Ondersteuning toegevoegd voor vSphere opdrachtregelinterface (vCLI) 5.5 Update 2.
* Ondersteuning toegevoegd voor Linux-besturingssystemen op de bronserver:
  * RHEL 6 Update 6
  * RHEL 5 11 bijwerken
  * CentOS 6 Update 6
  * CentOS 5 Update 11
* Oplossingen voor het oplossen van de volgende problemen:
  * Het registreren van de kluis is mislukt voor de configuratieserver of RX-server.
  * Clustervolumes niet zoals verwacht wanneer de geclusterde virtuele machines worden opnieuw beveiligd wanneer ze weer.
  * Failback mislukt als de hoofddoelserver wordt gehost op een andere ESXi-server van de on-premises productie virtuele machines.
  * Bestandsmachtigingen configuratie worden gewijzigd wanneer u bijwerkt naar 8.0.1 die van invloed op beveiliging en bewerkingen.
  * De hersynchronisatie-drempelwaarde niet afgedwongen zoals verwacht, wat resulteert in replicatie inconsistent gedrag.
  * De RPO-instellingen worden niet correct weergegeven in de configuratie-serverinterface. De waarde niet-gecomprimeerde gegevens geeft ten onrechte de gecomprimeerde waarde.
  * De verwijderbewerking niet zoals verwacht in de wizard vContinuum verwijderen en replicatie van de configuratie van server-interface wordt niet verwijderd.
  * In de wizard vContinuum de schijf is automatisch uitgeschakeld wanneer u klikt op **Details** in de weergave van de schijf tijdens het beveiligen van MSCS virtuele machines.
  * Tijdens de fysiek-naar-virtueel (P2V)-scenario zijn niet vereist HP-services, zoals CIMnotify en CqMgHost, verplaatst naar handleiding bij het herstellen van de virtuele machine. Dit leidt tot extra opstarten.
  * Beveiliging van Linux virtuele machine mislukt wanneer er meer dan 26 schijven op de hoofddoelserver zijn.

## <a name="next-steps"></a>Volgende stappen
Na de eventuele vragen die u hebt op de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
