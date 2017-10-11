---
title: Controleren en problemen oplossen van beveiliging voor virtuele machines en fysieke servers | Microsoft Docs
description: "Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines op de lokale servers naar Azure of een secundair datacenter. Gebruik dit artikel om te controleren en problemen met beveiliging van Virtual Machine Manager- of Hyper-V-site."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: 5bcb5dcb6afc3909e34dde31f845e014e7c539e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Controleren en problemen oplossen van beveiliging voor virtuele machines en fysieke servers
Deze handleiding bewaking en probleemoplossing vindt u informatie over het bijhouden van de status van replicatie en problemen oplossen technieken voor Azure Site Recovery.

## <a name="understand-the-components"></a>Inzicht in de onderdelen
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Virtuele VMware-machine of fysieke server site-implementatie voor replicatie tussen de on-premises en Azure
Als u databaseherstel tussen een on-premises VMware virtuele machine of fysieke server en Azure instelt, moet u voor het instellen van de configuratieserver, hoofddoelserver en serveronderdelen proces op de virtuele machine of de server. Wanneer u beveiliging voor de bronserver inschakelt, installeert Azure Site Recovery de functie Mobile Apps van Microsoft Azure App Service. Na een storing van de lokale en de bron-server is overgeschakeld naar Azure, klanten nodig hebben voor het instellen van een processerver in Azure en een hoofddoelserver on-premises opnieuw opbouwen van de bronserver on-premises.

![VMware of fysieke-site-implementatie voor replicatie tussen de on-premises en Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Implementatie van de Virtual Machine Manager-sites voor replicatie tussen on-premises sites
Als u databaseherstel tussen twee locaties van de lokale instelt, moet u de Azure Site Recovery provider downloaden en installeren op de Virtual Machine Manager-beheerserver. De provider moet verbinding met Internet om ervoor te zorgen dat de bewerkingen die worden geactiveerd via de Azure portal naar lokale bewerkingen ophalen vertaald.

![Implementatie van de Virtual Machine Manager-sites voor replicatie tussen on-premises sites](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Implementatie van de Virtual Machine Manager-sites voor replicatie tussen lokale locaties en Azure
Wanneer u databaseherstel tussen lokale locaties en Azure hebt ingesteld, moet u de Azure Site Recovery provider downloaden en installeren op de Virtual Machine Manager-beheerserver. U moet ook de Azure Recovery Services-Agent die moet worden geïnstalleerd op elke Hyper-V-host te installeren. [Meer informatie](site-recovery-hyper-v-azure-architecture.md) voor meer informatie.

![Implementatie van de Virtual Machine Manager-sites voor replicatie tussen lokale locaties en Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Hyper-V-site-implementatie voor replicatie tussen lokale locaties en Azure
Dit proces is vergelijkbaar met de Virtual Machine Manager-implementatie. Het enige verschil is dat de Azure Site Recovery provider en de Azure Recovery Services Agent geïnstalleerd op de Hyper-V-host zelf. [Meer informatie](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Bewaak de bewerkingen van configuratie-, beveiligings- en hersteltaken
Elke bewerking in de Azure Site Recovery wordt gecontroleerd en bijgehouden onder de **taken** tabblad. Voor elk configuratie, beveiliging of herstel fout, gaat u naar de **taken** tabblad en zoekt u fouten.

![Het filter is mislukt op het tabblad taken](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Als u fouten optreden bij de **taken** tabblad, klik op de taak en klikt u op **FOUTDETAILS** voor die taak.

![De knop FOUTDETAILS](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Details van de fout kunt u een mogelijke oorzaak en de aanbeveling voor het probleem te identificeren.

![Dialoogvenster met daarin de foutdetails voor een specifieke taak](media/site-recovery-monitoring-and-troubleshooting/image5.png)

In het vorige voorbeeld lijkt op een andere bewerking die wordt uitgevoerd te worden veroorzaakt door de configuratie van de beveiliging mislukken. Los het probleem op basis van de aanbeveling en klik vervolgens op **RESART** starten van de bewerking opnieuw uit.

![De knop opnieuw opstarten op het tabblad taken](media/site-recovery-monitoring-and-troubleshooting/image6.png)

De **opnieuw** optie is niet beschikbaar voor alle bewerkingen. Als er een bewerking geen de **opnieuw** optie, gaat u terug naar het object en opnieuw uitvoeren van de bewerking opnieuw uit. U kunt elke taak die in voortgang annuleren met behulp van de **annuleren** knop.

![De knop Annuleren](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Status van de replicatie voor virtuele machines controleren
U kunt de Azure-portal op afstand controleren Azure Site Recovery-providers voor elk van de beveiligde entiteiten. Klik op **beveiligde ITEMS**, en klik vervolgens op **VMM-CLOUDS** of **BEVEILIGINGSGROEPEN**. De **VMM-CLOUDS** tabblad is alleen beschikbaar voor implementaties die zijn gebaseerd op Virtual Machine Manager. Voor andere scenario's, de beveiligde entiteiten zijn onder de **BEVEILIGINGSGROEPEN** tabblad.

![De opties voor VMM-Clouds en BEVEILIGINGSGROEPEN](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Klik op een beveiligde entiteit onder de respectievelijke cloud of beveiligingsgroep om alle beschikbare bewerkingen te bekijken in het onderste deelvenster worden weergegeven.

![Beschikbare opties voor een geselecteerde beveiligde entiteit](media/site-recovery-monitoring-and-troubleshooting/image9.png)

In de vorige schermafbeelding ziet de status van de virtuele machine is **Kritiek**. U kunt klikken op de **FOUTDETAILS** knop op onder de fout aanhoudt. Op basis van de **mogelijke oorzaken** en **aanbeveling**, los het probleem.

![De knop foutdetails](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Fouten en aanbevelingen in het dialoogvenster Details van fouten](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Als er geen actieve bewerkingen worden uitgevoerd of is mislukt, gaat u naar de **taken** zoals eerder gezegd om de fout voor een bepaalde taak weer te geven.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Problemen met lokale Hyper-V
Verbinding maken met de lokale Hyper-V manager-console, selecteer de virtuele machine en de replicatiestatus zien.

![Optie replicatiestatus weergeven in de Hyper-V manager-console](media/site-recovery-monitoring-and-troubleshooting/image12.png)

In dit geval **replicatiestatus** is **Kritiek**. Met de rechtermuisknop op de virtuele machine en klik vervolgens op **replicatie** > **replicatiestatus weergeven** om de details te bekijken.

![Status van de replicatie voor een specifieke virtuele machine](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Als de replicatie is onderbroken voor de virtuele machine, met de rechtermuisknop op de virtuele machine en klik vervolgens op **replicatie** > **replicatie hervatten**.

![Optie voor het hervatten van de replicatie in de Hyper-V manager-console](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Als een virtuele machine wordt gemigreerd een nieuwe Hyper-V-host die zich binnen het cluster of een zelfstandige computer en de Hyper-V-host via de Azure Site Recovery is geconfigureerd, wouldn't replicatie voor de virtuele machine worden beïnvloed. Zorg ervoor dat de nieuwe Hyper-V-host voldoet aan alle vereisten en wordt geconfigureerd met behulp van Azure Site Recovery.

### <a name="event-log"></a>Gebeurtenislogboek
| Bronnen van gebeurtenissen | Details |
| --- |:--- |
| **Toepassingen en Service-logboeken/Microsoft/beheer voor virtuele machines/Server/Admin** (Virtual Machine Manager-beheerserver) |Biedt nuttige logboekregistratie om problemen met te veel verschillende Virtual Machine Manager. |
| **Toepassingen en Service MicrosoftAzureRecoveryServices-logboeken/replicatie** (Hyper-V-host) |Biedt nuttige logboekregistratie voor het oplossen van problemen met veel Microsoft Azure Recovery Services Agent. <br/> ![Locatie van de gebeurtenisbron replicatie voor Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Toepassingen en Service Microsoft-logboeken/Azure Site Recovery/Provider/operationeel** (Hyper-V-host) |Biedt nuttige logboekregistratie voor het oplossen van problemen met veel Microsoft Azure Site Recovery Service. <br/> ![Locatie van de bron van de operationele gebeurtenissen voor Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Toepassingen en Service-logboeken/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V-host) |Biedt nuttige logboekregistratie voor het oplossen van problemen van veel management voor Hyper-V virtuele machine. <br/> ![Locatie van de Virtual Machine Manager-gegevensbron voor Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Opties voor logboekregistratie van Hyper-V-replicatie
Alle gebeurtenissen die betrekking op de Hyper-V-replicatie hebben worden vastgelegd in de Hyper-V-VMMS\\bevindt zich onder logboeken toepassingen en Services van beheerder logboek\\Microsoft\\Windows. Bovendien kunt u een analytische logboek inschakelen voor de Hyper-V Virtual Machine Management-Service. Om dit logboek, Controleer eerst de logboeken voor analyse en foutopsporing zichtbaar in Logboeken. Open Logboeken en klik vervolgens op **weergave** > **analytische weergeven logboeken en foutopsporing**.

![De optie analytische weergeven en logboeken voor foutopsporing](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Er is een analytische logboek zichtbaar onder **Hyper-V-VMMS**.

![De analysen aanmelden in de Event Viewer-structuur](media/site-recovery-monitoring-and-troubleshooting/image15.png)

In de **acties** deelvenster, klikt u op **logboek inschakelen**. Nadat deze ingeschakeld, wordt deze weergegeven in **Prestatiemeter** als een **gebeurtenistraceersessies** zich onder **gegevensverzamelaarsets.**

![Gebeurtenistraceersessies in de structuur van Prestatiemeter](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Als u de verzamelde informatie, moet u eerst de traceringssessie stoppen door het logboek uit te schakelen. Het logboek, opslaan en opnieuw openen in Logboeken of andere hulpprogramma's gebruiken om deze te converteren naar wens.

## <a name="reach-out-for-microsoft-support"></a>Voor Microsoft Support bereiken
### <a name="log-collection"></a>Logboekgegevens verzameld
Raadpleeg voor de beveiliging van de Virtual Machine Manager-site, [Azure Site Recovery logboekgegevens verzameld met ondersteuning Diagnostics Platform (SDP) hulpprogramma](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) voor het verzamelen van Logboeken van de vereiste.

Voor de beveiliging van Hyper-V-site, downloadt u de [hulpprogramma](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) en uit te voeren op de Hyper-V-host voor het verzamelen van de logboeken.

Raadpleeg voor VMware of fysieke-serverscenario's [Azure Site Recovery logboekgegevens verzameld voor VMware en fysieke site beveiliging](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) voor het verzamelen van Logboeken van de vereiste.

Het hulpprogramma verzamelt de logboeken lokaal in een willekeurige naam submap onder % LocalAppData%\ElevatedDiagnostics.

![Voorbeeldstappen uit de beveiliging van Hyper-V-site.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Een ondersteuningsticket opent
Als u wilt een ondersteuningsticket voor Azure Site Recovery verhogen, bereiken voor de ondersteuning van Azure met behulp van de URL op <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>KB-artikelen
* [Het behouden van de stationsletter voor de beveiligde virtuele machines die failover of gemigreerd naar Azure](http://support.microsoft.com/kb/3031135)
* [On-premises naar Azure-beveiliging gebruik van netwerkbandbreedte beheren](https://support.microsoft.com/kb/3056159)
* [Azure Site Recovery: 'kan de cluster-bron niet gevonden' Fout bij het inschakelen van de beveiliging voor een virtuele machine](http://support.microsoft.com/kb/3010979)
* [Begrijpen en oplossen van Hyper-V-replicatie helpen](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Veelvoorkomende fouten voor Azure Site Recovery en hun oplossingen
Hieronder vindt u veelvoorkomende fouten en hun oplossingen. Elke fout wordt beschreven in een afzonderlijke wiki-pagina.

### <a name="general"></a>Algemeen
* <span style="color:green;">NIEUWE</span> [taken mislukken met fout "Er is een bewerking is uitgevoerd." Fout bij het 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">NIEUWE</span> [taken mislukken met fout 'De Server niet is verbonden met Internet'. Fout bij het 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Instellen
* [De Virtual Machine Manager-beheerserver kan niet worden geregistreerd vanwege een interne fout. Raadpleeg de taakweergave in de Site Recovery-portal voor meer informatie over de fout. Voer setup opnieuw uit om te registreren van de server.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Een verbinding kan niet worden gemaakt met de Hyper-V Recovery Manager-kluis. Controleer of de proxy-instellingen of probeer het later opnieuw.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Configuratie
* [Kan niet worden gemaakt van de beveiligingsgroep: Er is een fout opgetreden tijdens het ophalen van de lijst met servers.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Hyper-V-hostcluster bevat ten minste één statische netwerkadapter of er zijn geen verbonden adapters zijn geconfigureerd voor gebruik van DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Virtual Machine Manager heeft geen machtigingen om een actie te voltooien.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Kan geen selecteren de storage-account in het abonnement bij het configureren van beveiliging.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Beveiliging
* <span style="color:green;">NIEUWE</span> [beveiliging inschakelen is mislukt met fout 'Kan beveiliging niet configureren voor de virtuele machine'. Fout 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">NIEUWE</span> [beveiliging inschakelen is mislukt met fout 'Beveiliging kan niet worden ingeschakeld voor de virtuele machine'. Fout bij het 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">NIEUWE</span> [livemigratie fout 23848 - de virtuele machine wordt verplaatst met behulp van type Live gaat. Dit kan beschermingsstatus voor herstel van de virtuele machine.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Schakel de beveiliging is mislukt, omdat de Agent is niet geïnstalleerd op de hostmachine.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Geen geschikte host voor de replica virtuele machine kan niet worden gevonden - vanwege lage rekenresources.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Geen geschikte host voor de replica virtuele machine kan niet worden gevonden - vanwege geen logisch netwerk is gekoppeld.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Kan geen verbinding maken met de replica host-machine - kan geen verbinding worden gemaakt.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Herstel
* Virtual Machine Manager kan de hostbewerking niet voltooien:
  * [Failover naar het geselecteerde herstelpunt voor de virtuele machine: algemene fout: toegang geweigerd.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Failover naar het geselecteerde herstelpunt voor de virtuele machine van Hyper-V is mislukt: bewerking is afgebroken.  Probeer een meer recente herstelpunt. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Een verbinding met de server kan niet worden vastgesteld (0x00002EFD).
    * [Inschakelen van omgekeerde replicatie voor virtuele machine van Hyper-V is mislukt.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Hyper-V is mislukt voor replicatie voor virtuele machine virtuele machine in te schakelen.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Failover voor virtuele machine kan niet worden doorgevoerd.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Het herstelplan bevat virtuele machines die niet gereed voor geplande failover.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [De virtuele machine is niet gereed voor geplande failover.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Virtuele machine wordt niet uitgevoerd en wordt niet uitgeschakeld.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Buiten-band-bewerking opgetreden op een virtuele machine en de doorvoering van de failover is mislukt.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testfailover
  * [Failover kan niet worden gestart omdat de testfailover wordt uitgevoerd.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NIEUWE</span> Failover een time-out optreedt bij PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout vanwege de configuratie-instellingen op de Netwerkbeveiligingsgroep die zijn gekoppeld aan de virtuele Machine of het subnet waartoe de computer behoort. Raadpleeg [PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) voor meer informatie.

### <a name="configuration-server-process-server-master-target"></a>Configuratieserver, processerver, hoofddoel
* [De ESXi-host waarop de PS/CS wordt gehost als een virtuele machine is mislukt met een paarse scherm.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Extern bureaublad probleemoplossing na een failover
* Veel klanten hebben problemen met verbinding maken met de mislukte geconfronteerd via de virtuele machine in Azure. [Gebruik het probleemoplossing document RDP in de virtuele machine](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Een openbaar IP-adres toe te voegen op een virtuele machine van de resource manager
Als de **Connect** knop in de portal grijs wordt weergegeven, en u niet verbonden bent met Azure via een Express Route of Site-naar-Site VPN-verbinding, moet u maken en uw virtuele machine een openbare IP-adres toewijzen voordat u extern bureaublad kunt / Gedeelde Shell. Vervolgens kunt u een openbaar IP-adres toevoegen aan de netwerkinterface van de virtuele machine.  

![Virtuele machine een openbaar IP-adres toe te voegen aan de netwerkinterface van failover](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
