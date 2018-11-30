---
title: Azure Site Recovery voor probleemoplossing voor problemen met replicatie van Azure naar Azure en fouten | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen van Azure
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: asgang
ms.openlocfilehash: 22ea3d955fe2910dc99ab4015165008da899d48e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52312847"
---
# <a name="troubleshoot-azure-to-azure-vm-ongoing-replication-issues"></a>Oplossen van problemen met de voortdurende replicatie van Azure naar Azure VM

Dit artikel beschrijft de voorkomende problemen in Azure Site Recovery bij het repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio en wordt uitgelegd hoe u oplossingen voor deze problemen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).


## <a name="recovery-points-not-getting-generated"></a>Herstelpunten niet gegenereerd

Foutbericht: Er is geen crashconsistent herstelpunt beschikbaar voor de virtuele machine in de afgelopen 60 minuten.</br>
FOUT-ID: 153007 </br>

Azure Site Recovery repliceert gegevens van de bronregio naar de Dr-regio consistent en crash-consistent herstelpunt maakt om de 5 minuten. Als u Site Recovery kan maken van herstelpunten gedurende 60 minuten, waarschuwt gebruikers. Hieronder vindt u de oorzaken die tot deze fout leiden kunnen:

**1 oorzaak: [hoge gegevenswijzigingssnelheid op de virtuele bronmachine](#high-data-change-rate-on-the-source-virtal-machine)**    
**2 oorzaak: [connectiviteitsprobleem met het netwerk ](#Network-connectivity-issue)**

## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Hoge gegevenswijzigingssnelheid op de bron-VM
Azure Site Recovery wordt een gebeurtenis geactiveerd als de veranderingssnelheid van gegevens op de virtuele bronmachine hoger dan de ondersteunde limieten is. Als u wilt controleren of het probleem door het hoge verloop is, gaat u naar gerepliceerde items > virtuele machine > Klik op ' gebeurtenissen-laatste 72 uur '.
U ziet de gebeurtenis 'gegevens wijzigingssnelheid buiten ondersteunde limieten' zoals wordt weergegeven in de onderstaande schermafbeelding

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Als u op de gebeurtenis klikt, ziet u de exacte schijfgegevens zoals wordt weergegeven in de onderstaande schermafbeelding

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


#### <a name="azure-site-recovery-limits"></a>Azure Site Recovery-limieten
De volgende tabel bevat de Azure Site Recovery-limieten. Deze limieten zijn gebaseerd op onze tests, maar dekken niet alle mogelijke toepassings-I/O-combinaties. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. We moeten ook Houd er rekening mee dat er zijn twee limieten rekening houden met per schijf gegevens verloop en verloop per gegevens van virtuele machines.
Bijvoorbeeld, als we kijken naar de schijf P20 Premium in de onderstaande tabel, Site Recovery kan voor het afhandelen van 5 MB/s verloop per schijf met op maximaal vijf dergelijke schijven per virtuele machine vanwege de limiet van 25 MB/s totale verloop per virtuele machine.

**Beoogde replicatieopslag** | **Gemiddelde I/O-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |10 MB/s | 842 GB per schijf

### <a name="solution"></a>Oplossing
We moeten begrijpen dat Azure Site Recovery gegevens wijzigen op basis van het type schijf frequentielimieten heeft. De gegevens worden gewijzigd als u wilt weten of dit probleem terugkerend is verbroken, is het belangrijk om te zoeken patroon van de snelheid van de betreffende virtuele machine.
Als u wilt zoeken veranderingssnelheid de van gegevens van de betreffende virtuele machine. Ga naar de virtuele bronmachine > metrische gegevens onder controleren en de metrische gegevens toe te voegen, zoals hieronder weergegeven.

![high_data_change_rate](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Klik op 'Add metrische waarde' en "Besturingssysteemschijf geschreven Bytes per seconde" en 'Gegevensschijf geschreven Bytes per seconde' toevoegen.
2. Controleer de piek zoals wordt weergegeven in de schermafbeelding.
3. U ziet dat het totale aantal schrijft bewerking in de schijf met besturingssysteem en alle gegevensschijven die zijn gecombineerd. Deze metrische gegevens worden nu vertelt u misschien niet per gegevens op schijf maar is een goede indicatie van het totale verlooppatroon.

In gevallen, zoals hierboven als dit een gegevensburst af en toe is en de veranderingssnelheid van gegevens is groter dan 10 MBps (voor Premium) en 2 MBps (voor Standard) voor enige tijd en wordt geleverd, wordt replicatie achterstand. Maar als het verloop dan de ondersteunde limiet voor de meeste gevallen is, klikt u vervolgens kunt u overwegen een van de onderstaande optie indien mogelijk:

**Optie 1:** uitsluiten van de schijf die wordt veroorzaakt door hoge gegevenswijzigingssnelheid: </br>
U kunt op dit moment uitsluiten voor de schijf met [Site Recovery Powershell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine)

**Optie 2:** de disaster recovery opslaglaag schijf wijzigen: </br>
Deze optie is alleen mogelijk als de gegevensverloop van bronschijf minder dan 10 MB/s is. Stel dat een virtuele machine met P10, kunnen heeft schijf een gegevensverloop van meer dan 8 MB/s, maar minder dan 10 MB/s. Als klant P30-schijf voor de doel-opslagaccount tijdens beveiliging gebruiken kunt, kan het probleem worden opgelost.

### <a name="Network-connectivity-issue"></a>Probleem met de netwerkverbinding

#### <a name="network-latency-to-cache-storage-account-"></a>De netwerklatentie voor Cache-opslagaccount:
 Site Recovery gerepliceerde gegevens verzendt naar de cache-opslagaccount en het probleem kan optreden als het uploaden van de gegevens van virtuele machine naar het cacheopslagaccount trager is die 4 MB in 3 seconden. Om te controleren of er een probleem met betrekking tot latentie gebruik [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) voor het uploaden van gegevens van de virtuele machine naar de cache-opslagaccount.<br>
Als de latentie hoog is, controleert u als u een virtuele netwerkapparaten gebruikt voor het beheren van uitgaande netwerkverkeer van virtuele machines. Het apparaat mogelijk te maken met beperkingen als het replicatieverkeer wordt doorgegeven via de NVA. Het wordt aangeraden om een service-eindpunt van het netwerk maken in uw virtuele netwerk voor 'Opslag', zodat het replicatieverkeer niet naar de NVA. Raadpleeg [netwerkconfiguratie virtueel apparaat](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration)

#### <a name="network-connectivity"></a>Verbinding met het netwerk
Voor Site Recovery-replicatie met werk, uitgaande connectiviteit voor bepaalde URL's of IP-bereiken zijn van de virtuele machine. Als uw VM zich achter een firewall bevindt of regels voor network security group (NSG) gebruikt voor het beheren van uitgaande connectiviteit, kunt u een van deze problemen kan tegenkomen.</br>
Raadpleeg [uitgaande connectiviteit voor URL's van Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) om te controleren of alle URL's zijn verbonden 