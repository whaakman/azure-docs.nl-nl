---
title: StorSimple Virtual Array Updates opmerkingen bij de release | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 0.2 en 0.1 wordt uitgevoerd.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: aad60024187ca180c002f119f4b975e8f69796e5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60629285"
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Opmerkingen bij de release van de StorSimple Virtual Array Update 0.2 en 0.1
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates. (Microsoft Azure StorSimple Virtual Array is ook wel bekend als de on-premises StorSimple-apparaat of de virtuele StorSimple-apparaat.) 

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw virtuele StorSimple-apparaat implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0.2 komt overeen met de softwareversie **10.0.10280.0**; Update 0.1 is versie **10.0.10279.0**. De onderstaande secties worden de wijzigingen voor elke update. 

> [!NOTE]
> Updates opnieuw moet worden opgestart en wordt uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, wordt het apparaat in rekening gebracht downtime.
> 
> 

## <a name="issues-fixed-in-the-update-02"></a>Problemen in de Update 0.2 opgelost
Update 0.2 bevat alle wijzigingen van Update 0.1 naast de oplossing in de volgende tabel beschreven:

| Functie | Probleem |
| --- | --- |
| Updates |In de laatste versie zijn niet updates automatisch gedetecteerd in de klassieke Azure portal, zodat moest u de lokale Webgebruikersinterface gebruiken om updates te installeren. Dit probleem is opgelost in deze release. Na de installatie van Update 0.2, kunt u toekomstige updates met behulp van de klassieke Azure-portal installeren. |

## <a name="whats-new-in-the-update-01"></a>Wat is er nieuw in de Update 0.1
Update 0.1 bevat de volgende oplossingen voor problemen en verbeteringen. 

* **Verbeterde tolerantie bij uitval van de cloud**: Deze release bevat verschillende oplossingen voor problemen rond herstel na noodgevallen, back-up, herstel en lagen in het geval van een onderbreking van de connectiviteit van de cloud. 
* **Verbeterde prestaties terugzetten**: Deze release bevat oplossingen voor problemen die u hebt de voltooiingstijd van de hersteltaken aanzienlijk verminderen.
* **Ruimte vrijmaken optimalisatie geautomatiseerde**: Wanneer gegevens worden verwijderd op dun ingerichte volumes, moeten de opslag van niet-gebruikte blokken worden vrijgemaakt. Deze release is het proces voor het vrijmaken van ruimte vanuit de cloud die leidt tot de ongebruikte ruimte wordt sneller beschikbaar in vergelijking met vorige versies worden verbeterd.
* **Nieuwe installatiekopieën van virtuele schijven**: Nieuwe VHD, VHDX en VMDK zijn nu beschikbaar via de klassieke Azure portal. U kunt deze installatiekopieën voor het inrichten van nieuwe Update 0.1-apparaten kunt downloaden.
* **De nauwkeurigheid van de status van de taken in de portal van**: In de eerdere versie van software is taakstatus rapportage in de portal niet nauwkeurig. Dit probleem wordt opgelost in deze release.
* **Domain-join-ervaring**: Oplossingen voor problemen met betrekking tot toevoegen aan het domein en het wijzigen van de naam van het apparaat.

## <a name="issues-fixed-in-the-update-01"></a>Problemen opgelost in de Update 0.1
De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |VMDK |De besturingssysteemschijf is in sommige versies van VMware gezien als sparse waarschuwingen veroorzaken en normale verstoren. Dit is opgelost in deze release. |
| 2 |iSCSI-server |In de laatste versie was de gebruiker vereist om op te geven van een gateway voor elke ingeschakelde netwerkinterface van uw virtuele StorSimple-apparaat. Dit gedrag is gewijzigd in deze release, zodat de gebruiker heeft om ten minste één gateway voor de ingeschakelde netwerkinterfaces te configureren. |
| 3 |Ondersteuningspakket |In de eerdere versie van software, ondersteuning voor pakketverzameling is mislukt tijdens de pakket-grootten groter dan 1 GB zijn. Dit probleem is opgelost in deze release. |
| 4 |Cloudtoegang |In de laatste versie als de StorSimple Virtual Array is geen verbinding met het netwerk en opnieuw is opgestart, zou de gebruikersinterface voor het lokale hebben problemen met de netwerkverbinding. Dit probleem is opgelost in deze release. |
| 5 |Bewakingsgrafieken |De cloud capaciteit gebruik grafieken weergegeven in de vorige release, na een failover van het apparaat, onjuiste waarden in de klassieke Azure portal. Dit is opgelost in de huidige release. |

## <a name="known-issues-in-the-update-01"></a>Bekende problemen in de Update 0.1
De volgende tabel bevat een overzicht van de bekende problemen voor de StorSimple Virtual Array en bevat de problemen release hebt genoteerd uit de vorige versies. **De versie van de problemen die u hebt genoteerd in deze release zijn gemarkeerd met een sterretje. Bijna alle problemen in deze lijst zijn overgedragen van de GA-versie van StorSimple Virtual Array.**

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele apparaten zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie voor algemene beschikbaarheid. |Deze virtuele apparaten, moeten u failover voor de algemeen beschikbare versie met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Als u een gegevensschijf van een bepaalde opgegeven grootte hebt ingericht en de bijbehorende virtuele StorSimple-apparaat hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Er wordt geprobeerd om dit te doen, leidt het verlies van alle gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat is het domein, toepassen van een Groepsbeleid kan nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele array kan in een eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale web-UI |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige lokale web-UI-pagina's, zoals probleemoplossing of onderhoud niet goed werkt. Knoppen op deze pagina's kunnen ook niet werken. |Geavanceerde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale web-UI |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-UI worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. Hyper-V bevat altijd 10 Gbps voor virtuele netwerkadapters. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing opnemen in de lokaal vastgemaakte volumes in plaats van gelaagde volumes.<br></br>*Voorbehoud*: Als u met behulp van lokaal vastgemaakte volumes en byte bereik vergrendeling is ingeschakeld, worden op de hoogte dat de lokaal vastgemaakte volume online worden kan, zelfs nog voor het herstellen voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet klikt u vervolgens u wachten voor de herstelbewerking is voltooid. |
| **7.** |Gelaagde bestandsshares |Werken met grote bestanden kan leiden tot trage laag uitschalen. |Als u werkt met grote bestanden, wordt u aangeraden het grootste bestand kleiner is dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteitsplanning voor bestandsshares |Mogelijk ziet u delen van gebruik in de afwezigheid van alle gegevens op de share. Dit is omdat de gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen naar een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit wordt geïmplementeerd in een latere versie. |
| **10.** |Azure PowerShell |De virtuele StorSimple-apparaten worden niet beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden gedaan via de klassieke Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de indeling en-US. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Bovendien, als u de CHAP-referenties wijzigt, moet u zet de volumes offline en brengt u deze online voor de wijziging door te voeren. |Dit wordt opgelost in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kunnen mogelijk verschillen in de StorSimple Manager-service en de iSCSI-host. |De iSCSI-host is de weergave van het bestandssysteem.<br></br>Het apparaat ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestand server * |Als een bestand in een map is een alternatieve gegevens Stream (AD) die ermee verbonden zijn, wordt de ADVERTENTIES niet een back-up of via herstel na noodgevallen, klonen en herstel op itemniveau wordt hersteld. | |

## <a name="next-step"></a>Volgende stap
[Updates installeren](storsimple-ova-install-update-01.md) op uw StorSimple Virtual Array.

