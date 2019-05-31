---
title: Azure Site Recovery voor probleemoplossing voor lopende replicatieproblemen met Azure-naar-Azure | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen van Azure
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: bf24b2d1395e128dc73361670ea93ac938574146
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258791"
---
# <a name="troubleshoot-ongoing-problems-in-azure-to-azure-vm-replication"></a>Problemen met doorlopende oplossen in Azure-naar-Azure VM-replicatie

Dit artikel worden veelvoorkomende problemen met Azure Site Recovery bij het repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio. Ook wordt uitgelegd hoe u oplossingen voor deze problemen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).

Azure Site Recovery repliceert gegevens uit de regio van de gegevensbron naar de Dr-regio consistent en maakt een crash-consistente herstelpunt om de 5 minuten. Als u Site Recovery geen herstelpunten maken gedurende 60 minuten, er wordt een waarschuwing met de volgende informatie:

Foutbericht: "Er is geen crashconsistent herstelpunt beschikbaar voor de virtuele machine in de afgelopen 60 minuten."</br>
Fout-ID: 153007 </br>

De volgende secties beschrijven de oorzaken en oplossingen.

## <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Hoge gegevenswijzigingssnelheid op de virtuele bronmachine
Azure Site Recovery wordt een gebeurtenis geactiveerd als de veranderingssnelheid van gegevens op de virtuele bronmachine hoger dan de ondersteunde limieten is. Als u wilt controleren of het probleem door het hoge verloop is, gaat u naar **gerepliceerde items** > **VM** > **gebeurtenissen-laatste 72 uur**.
U ziet de gebeurtenis 'Gegevenswijzigingssnelheid buiten ondersteunde limieten':

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Als u de gebeurtenis selecteert, ziet u informatie over de exacte schijf:

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


### <a name="azure-site-recovery-limits"></a>Azure Site Recovery-limieten
De volgende tabel bevat de Azure Site Recovery-limieten. Deze limieten zijn gebaseerd op onze tests maar dekken niet alle mogelijke toepassings-i/o-combinaties. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. 

Er zijn twee limieten te overwegen, gegevensverloop per schijf en gegevensverloop per virtuele machine. Bijvoorbeeld: we kijken naar de schijf P20 Premium in de volgende tabel. Site Recovery kan verwerken 5 MB/s van het verloop per schijf met een maximum van vijf dergelijke schijven per virtuele machine, vanwege de limiet van 25 MB/s van het totale verloop per virtuele machine.

**Beoogde replicatieopslag** | **Gemiddelde i/o-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop per dag voor gegevens van bronschijf**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |10 MB/s | 842 GB per schijf

### <a name="solution"></a>Oplossing
Azure Site Recovery heeft limieten op veranderingssnelheid van gegevens, op basis van het type van de schijf. Als u wilt weten of dit probleem terugkerende of kortstondige, vinden dat de gegevenswijzigingssnelheid van de betreffende virtuele machine. Ga naar de bron-VM, vindt de metrische gegevens onder **bewaking**, en voeg de metrische gegevens zoals weergegeven in deze schermafbeelding:

![Drie stappen voor het vinden van de veranderingssnelheid van gegevens](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Selecteer **metrische waarde toevoegen**, en voeg **OS schijf geschreven Bytes per seconde** en **gegevensschijf geschreven Bytes per seconde**.
2. Controleer de piek zoals wordt weergegeven in de schermafbeelding.
3. Weergave van het totale aantal schrijfbewerkingen in besturingssysteemschijven en alle gegevensschijven die zijn gecombineerd. Deze metrische gegevens kan niet krijgt u informatie op het niveau van de per schijf, maar ze geven aan het totale gegevensverloop-patroon.

Als een piek is van een af en toe pieken en de gegevens wijzigen tarief is groter dan 10 MB/s (voor Premium) en 2 MB/s (voor Standard) voor een aantal keer dat en wordt geleverd omlaag, replicatie wordt achterstand. Maar als het verloop is dan de ondersteunde de meeste gevallen beperken, kunt u een van deze opties indien mogelijk:

* **Uitsluiten van de schijf die wordt veroorzaakt door een hoge veranderingssnelheid van de gegevens**: U kunt de schijf uitsluiten met behulp van [PowerShell](./azure-to-azure-exclude-disks.md). Als u wilt uitsluiten van de schijf die u moet eerst de replicatie uitschakelen. 
* **Wijzigen van de laag van de opslagschijf voor disaster recovery**: Deze optie is alleen mogelijk als de gegevensverloop van bronschijf minder dan 10 MB/s is. Stel dat een virtuele machine met een P10-schijf heeft een gegevensverloop van meer dan 8 MB/s, maar minder dan 10 MB/s. Als de klant een P30-schijf voor de doel-opslagaccount tijdens beveiliging gebruiken kunt, kan het probleem worden opgelost.

## <a name="Network-connectivity-problem"></a>Problemen met de netwerkverbinding

### <a name="network-latency-to-a-cache-storage-account"></a>De netwerklatentie voor een cache-opslagaccount
Site Recovery verzendt gerepliceerde gegevens naar het cacheopslagaccount. U ziet de netwerklatentie als het uploaden van de gegevens van een virtuele machine naar het cacheopslagaccount is langzamer dan 4 MB in 3 seconden. 

Gebruiken om te controleren of er een probleem met betrekking tot latentie, [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) voor het uploaden van gegevens van de virtuele machine naar de cache-opslagaccount. Als de latentie hoog is, controleert u of u een virtueel netwerkapparaat (NVA) gebruikt voor het beheren van uitgaande netwerkverkeer van virtuele machines. Het apparaat mogelijk te maken met beperkingen als het replicatieverkeer wordt doorgegeven via de NVA. 

Het wordt aangeraden om een service-eindpunt van het netwerk maken in uw virtuele netwerk voor 'Opslag', zodat het replicatieverkeer niet u naar de NVA gaat. Zie voor meer informatie, [virtueel apparaat netwerkconfiguratie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Verbinding met het netwerk
Voor Site Recovery-replicatie met werk, uitgaande connectiviteit voor bepaalde URL's of IP-bereiken zijn van de virtuele machine. Als uw VM zich achter een firewall bevindt of regels voor network security group (NSG) gebruikt voor het beheren van uitgaande connectiviteit, kunt u een van deze problemen kan tegenkomen. Als u wilt controleren of alle URL's zijn verbonden, Zie [uitgaande connectiviteit voor URL's van Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Fout-ID 153006 - er is geen toepassingsconsistent herstelpunt beschikbaar voor de virtuele machine in de laatste 'XXX' minuten

Enkele van de meest voorkomende problemen worden hieronder vermeld.

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>1 oorzaak: Bekend probleem in SQL server 2008/2008 R2 
**Voor het oplossen van** : Er is een bekend probleem met SQL server 2008/2008 R2. Raadpleeg dit KB-artikel [Azure Site Recovery-Agent of andere niet-onderdeel VSS back-up mislukt voor een server waarop SQL Server 2008 R2](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-autoclose-dbs"></a>2 oorzaak: Azure Site Recovery-taken mislukken op servers die als host fungeert voor een willekeurige versie van SQL Server-exemplaren met Auto_sluiten DB 's 
**Voor het oplossen van** : Raadpleeg de Kb [artikel](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) 


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>3 oorzaak: Bekend probleem in SQL Server 2016 en 2017
**Voor het oplossen van** : Raadpleeg de Kb [artikel](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) 

#### <a name="cause-4-you-are-using-storage-spaces-direct-configuration"></a>4 oorzaak: U gebruikt Storage spaces direct-configuratie
**Voor het oplossen van** : Azure Site Recovery kan toepassingsconsistent herstelpunt voor Storage spaces direct-configuratie niet maken. Raadpleeg het artikel correct [configureren van het replicatiebeleid](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-s2d-vms)

### <a name="more-causes-due-to-vss-related-issues"></a>Problemen met betrekking tot meer oorzaken vanwege VSS:

Om het probleem verder oplossen, controleert u de bestanden op de broncomputer om de exacte foutcode van de fout:
    
    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Hoe kan ik de fouten in het bestand vinden?
Zoeken naar de tekenreeks 'vacpError' door het openen van het bestand vacp.log in een editor naar
        
    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

In het bovenstaande voorbeeld **2147754994** wordt de foutcode die u over de fout, vertelt zoals hieronder wordt weergegeven

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS-schrijver is niet geïnstalleerd - fout 2147221164 

*Voor het oplossen van*: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Een VSS-Provider voor de werking ervan om consistentie van momentopnamen voor een app te worden geïnstalleerd. Deze VSS-Provider wordt geïnstalleerd als een service. In het geval de VSS-Provider-service niet is geïnstalleerd, is het maken van de momentopname van de toepassing consistentie mislukt met de foutcode 0x80040154 'Klasse is niet geregistreerd'. </br>
Raadpleeg [artikel voor de VSS writer installatieproblemen oplossen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS-schrijver wordt uitgeschakeld - fout 2147943458

**Voor het oplossen van**: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Een VSS-Provider voor de werking ervan om consistentie van momentopnamen voor een app te worden geïnstalleerd. Deze VSS-Provider wordt geïnstalleerd als een service. In het geval de VSS-Provider-service is uitgeschakeld, mislukt het maken van de momentopname van de toepassing consistentie met de id van de fout 'de opgegeven service is uitgeschakeld en kan niet worden started(0x80070422)'. </br>

- Als de VSS is uitgeschakeld,
    - Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service

####  <a name="vss-provider-notregistered---error-2147754756"></a>VSS-PROVIDER NOT_REGISTERED - fout 2147754756

**Voor het oplossen van**: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Controleer of de service Azure Site Recovery VSS Provider is geïnstalleerd of niet. </br>

- De Provider-installatie met behulp van de volgende opdrachten opnieuw uitvoeren:
- Bestaande-provider verwijderen: C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Opnieuw te installeren: C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd
 
Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service
