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
ms.openlocfilehash: 9ff756270c368d39b7ef78d7c1046f7c91169668
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56299595"
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

* **Uitsluiten van de schijf die wordt veroorzaakt door een hoge veranderingssnelheid van de gegevens**: U kunt de schijf uitsluiten met behulp van [PowerShell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine).
* **Wijzigen van de laag van de opslagschijf voor disaster recovery**: Deze optie is alleen mogelijk als de gegevensverloop van bronschijf minder dan 10 MB/s is. Stel dat een virtuele machine met een P10-schijf heeft een gegevensverloop van meer dan 8 MB/s, maar minder dan 10 MB/s. Als de klant een P30-schijf voor de doel-opslagaccount tijdens beveiliging gebruiken kunt, kan het probleem worden opgelost.

## <a name="Network-connectivity-problem"></a>Problemen met de netwerkverbinding

### <a name="network-latency-to-a-cache-storage-account"></a>De netwerklatentie voor een cache-opslagaccount
Site Recovery verzendt gerepliceerde gegevens naar het cacheopslagaccount. U ziet de netwerklatentie als het uploaden van de gegevens van een virtuele machine naar het cacheopslagaccount is langzamer dan 4 MB in 3 seconden. 

Gebruiken om te controleren of er een probleem met betrekking tot latentie, [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) voor het uploaden van gegevens van de virtuele machine naar de cache-opslagaccount. Als de latentie hoog is, controleert u of u een virtueel netwerkapparaat (NVA) gebruikt voor het beheren van uitgaande netwerkverkeer van virtuele machines. Het apparaat mogelijk te maken met beperkingen als het replicatieverkeer wordt doorgegeven via de NVA. 

Het wordt aangeraden om een service-eindpunt van het netwerk maken in uw virtuele netwerk voor 'Opslag', zodat het replicatieverkeer niet u naar de NVA gaat. Zie voor meer informatie, [virtueel apparaat netwerkconfiguratie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Verbinding met het netwerk
Voor Site Recovery-replicatie met werk, uitgaande connectiviteit voor bepaalde URL's of IP-bereiken zijn van de virtuele machine. Als uw VM zich achter een firewall bevindt of regels voor network security group (NSG) gebruikt voor het beheren van uitgaande connectiviteit, kunt u een van deze problemen kan tegenkomen. Als u wilt controleren of alle URL's zijn verbonden, Zie [uitgaande connectiviteit voor URL's van Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 