---
title: StorSimple Virtual Array Update 0.4 release-opmerkingen | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 0.4 wordt uitgevoerd.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 06a3469507631d032535bce62b01d964e99dc603
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319398"
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple Virtual Array Update 0.4 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw StorSimple Virtual Array implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0.4 komt overeen met de softwareversie **10.0.10289.0**.

> [!NOTE]
> Updates opnieuw moet worden opgestart en uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, heeft het apparaat leidt tot downtime.


## <a name="whats-new-in-the-update-04"></a>Wat is er nieuw in de Update 0.4
Update 0.4 is voornamelijk een opgelost probleem build in combinatie met een aantal verbeteringen. In deze versie zijn zijn meerdere fouten, wat resulteert in mislukte back-ups in de vorige versie opgelost. De belangrijkste verbeteringen en oplossingen voor problemen zijn als volgt:

- **Back-up prestatieverbeteringen** -deze release heeft enkele belangrijke verbeteringen voor het verbeteren van de back-upprestaties gemaakt. De back-ups die betrekking hebben op een groot aantal bestanden zien als gevolg hiervan een significante vermindering van de tijd in beslag, voor volledige en incrementele back-ups.

- **Verbeterde prestaties terugzetten** -deze release bevat verbeteringen die de restore-prestaties aanzienlijk verbeteren bij het gebruik van groot aantal bestanden. Als 2-4 miljoen bestanden gebruikt, wordt u aangeraden dat u een virtuele matrix met 16 GB RAM-geheugen om te zien van de verbeteringen inrichten. Wanneer u minder dan 2 miljoen bestanden gebruikt, blijft de minimumvereiste voor de virtuele machine 8 GB RAM-geheugen.

- **Verbeteringen in ondersteuningspakket** -de verbeteringen omvatten logboekregistratie in de statistieken voor de schijf, CPU, geheugen, netwerk en de cloud in het ondersteuningspakket waardoor het proces voor het vaststellen van/foutopsporing van problemen met het apparaat.

- **Limiet lokaal vastgemaakt volumes van de iSCSI-200 GB** -voor lokaal vastgemaakte volumes, wordt aangeraden om te beperken tot een 200 GB iSCSI-volume op uw StorSimple Virtual Array. De lokale ruimte gereserveerd voor gelaagde volumes blijft 10% van de ingerichte volumegrootte, maar wordt beperkt tot 200 GB. 

- **Oplossingen voor problemen met betrekking tot back-up** - In eerdere versies van software, er zijn problemen met betrekking tot back-ups die kan leiden de back-upfouten dat ertoe. Deze fouten zijn opgelost in deze release.


## <a name="issues-fixed-in-the-update-04"></a>Problemen in de Update 0.4 opgelost

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-upprestaties|In de eerdere releases, zou de back-ups met betrekking tot groot aantal bestanden een veel tijd (in volgorde van dagen) duren. In deze release Zie zowel de volledige en incrementele back-ups een significante vermindering van de tijd voor voltooiing. |
| 2 |Ondersteuningspakket|Schijf, CPU, geheugen, netwerk en statistieken van de cloud zijn nu aangemeld bij de ondersteuning van Logboeken waardoor de ondersteuningspakketten zeer effectief zijn bij het oplossen van eventuele problemen met het apparaat.|
| 3 |Backup |In eerdere versies, kan langlopende back-ups resulteren in een ruimte benut op het apparaat leidt tot mislukte back-ups. Deze fout wordt beschreven in deze release doordat niet meer dan 5 back-ups naar de wachtrij in één keer.|
| 4 |iSCSI | In eerdere versies is de lokale ruimte gereserveerd voor gelaagde of lokaal vastgemaakte volumes 10% van de ingerichte volumegrootte. In deze release de lokale ruimte gereserveerd voor alle iSCSI-volumes (lokaal vastgemaakt of lagen) is beperkt tot 10% met een maximum van maximaal 200 GB (voor gelaagde volumes die groter zijn dan 2 TB) waardoor vrijmaken meer ruimte vrij op de lokale schijf. We raden aan dat de lokaal vastgemaakte volumes in deze release beperkt tot 200 GB worden.|


## <a name="known-issues-in-the-update-04"></a>Bekende problemen in de Update 0.4

De volgende tabel bevat een overzicht van de bekende problemen voor de StorSimple Virtual Array en bevat de problemen release hebt genoteerd uit de vorige versies. 

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele apparaten zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie voor algemene beschikbaarheid. |Deze virtuele apparaten, moeten u failover voor de algemeen beschikbare versie met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Als u een gegevensschijf van een bepaalde opgegeven grootte hebt ingericht en de bijbehorende virtuele StorSimple-apparaat hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Er wordt geprobeerd om te doen, resulteert in een verlies van de gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat is het domein, toepassen van een Groepsbeleid kan nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele array kan in een eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale web-UI |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige lokale web-UI-pagina's, zoals probleemoplossing of onderhoud niet goed werkt. Knoppen op deze pagina's kunnen ook niet werken. |Geavanceerde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale web-UI |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-UI worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. Hyper-V bevat altijd 10 Gbps voor virtuele netwerkadapters. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing opnemen in de lokaal vastgemaakte volumes in plaats van gelaagde volumes.<br></br>*Voorbehoud*: Wanneer met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, kan het lokaal vastgemaakte volume online zijn, zelfs nog voor het herstellen voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet klikt u vervolgens u wachten voor de herstelbewerking is voltooid. |
| **7.** |Gelaagde bestandsshares |Werken met grote bestanden kan leiden tot trage laag uitschalen. |Als u werkt met grote bestanden, wordt u aangeraden het grootste bestand kleiner is dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteitsplanning voor bestandsshares |U ziet mogelijk verbruik delen wanneer er geen gegevens op de share zijn. Dit is omdat de gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen naar een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere versie. |
| **10.** |Azure PowerShell |De virtuele StorSimple-apparaten worden niet beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden gedaan via de klassieke Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de indeling en-US. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Bovendien, als u de CHAP-referenties wijzigt, moet u zet de volumes offline en brengt u deze online voor de wijziging door te voeren. |Dit probleem is opgelost in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kunnen mogelijk verschillen in de StorSimple Manager-service en de iSCSI-host. |De iSCSI-host is de weergave van het bestandssysteem.<br></br>Het apparaat ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map is een alternatieve gegevens Stream (AD) die ermee verbonden zijn, wordt de ADVERTENTIES niet een back-up of via herstel na noodgevallen, klonen en herstel op itemniveau wordt hersteld. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het resultaat van de StorSimple Virtual Array bestand server in een niet-ondersteunde configuratie.  | |

## <a name="next-step"></a>Volgende stap
[Update 0.4 installeren](storsimple-virtual-array-install-update-04.md) op uw StorSimple Virtual Array.

## <a name="references"></a>Verwijzingen
Wilt u een oudere releaseopmerking? Ga naar: 

* [StorSimple Virtual Array Update 0.3 Release Notes](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 en 0.2 opmerkingen bij de Release](storsimple-ova-update-01-release-notes.md)
* [Opmerkingen bij de Release van StorSimple Virtual Array algemene beschikbaarheid](storsimple-ova-pp-release-notes.md)

