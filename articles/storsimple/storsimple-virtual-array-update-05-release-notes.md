---
title: StorSimple Virtual Array Update 0.5 release-opmerkingen | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 0.5 wordt uitgevoerd.
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
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: 385d9126d578250064659153f6f0f54eec696790
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60870669"
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>StorSimple Virtual Array Update 0.5 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw StorSimple Virtual Array implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0.5 komt overeen met de softwareversie **10.0.10290.0**.

> [!NOTE]
> Updates opnieuw moet worden opgestart en uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, heeft het apparaat leidt tot downtime. Ga voor gedetailleerde instructies voor het toepassen van de update naar [Installeer Update 0.5](storsimple-virtual-array-install-update-05.md).


## <a name="whats-new-in-the-update-05"></a>Wat is er nieuw in de Update 0.5
Update 0.5 is voornamelijk een opgelost probleem build. De belangrijkste verbeteringen en oplossingen voor problemen zijn als volgt:

- **Back-up van tolerantie verbeteringen** -deze release bevat oplossingen ter verbetering van de back-up tolerantie. Back-ups zijn in de eerdere versies is opnieuw uitgevoerd alleen voor bepaalde uitzonderingen. Deze release pogingen van de back-uitzonderingen en zorgt ervoor dat de back-ups beter bestand is tegen.

- **Updates voor het gebruik van opslagbewaking** -vanaf 30 juni 2017, de opslag gebruiksbewaking voor serie van StorSimple virtuele apparaten worden buiten gebruik gesteld. Dit geldt voor de bewakingsgrafieken op de virtuele matrices Update 0.4 of lager wordt uitgevoerd. Deze update bevat de wijzigingen die vereist zijn voor u kunt doorgaan met het gebruik van opslag gebruiksbewaking in Azure portal. Deze essentiële update vóór 30 juni 2017 om door te gaan met de functie voor controle.


## <a name="issues-fixed-in-the-update-05"></a>Problemen opgelost in de Update 0.5

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-up tolerantie| Back-ups zijn in de eerdere versies is opnieuw uitgevoerd alleen voor bepaalde uitzonderingen. Deze release bevat een oplossing voor back-ups beter bestand is tegen maken door opnieuw te proberen de back-uitzonderingen.|
| 2 |Bewaking| Gebruiksbewaking opslag voor virtuele StorSimple-apparaat-serie worden afgeschaft vanaf 30 juni 2017. Deze actie heeft gevolgen voor de bewakingsgrafieken van de StorSimple Device Manager-service die wordt uitgevoerd op virtuele StorSimple-matrices (1200-model). Deze release bevat updates waarmee gebruikers kunnen doorgaan met het gebruik van opslag gebruiksbewaking op de virtuele matrices na 30 juni 2017.|
| 3 |Bestandsserver| In de eerdere releases, kan een gebruiker per ongeluk versleutelde bestanden kopiëren naar de virtuele matrix. Deze release bevat een oplossing waarmee geen versleutelde bestanden worden gekopieerd naar de virtuele matrix. Als uw apparaat heeft een bestaande versleutelde bestanden vóór de update, wordt back-ups blijft mislukken tot de versleutelde bestanden worden verwijderd uit het systeem. |


## <a name="known-issues-in-the-update-05"></a>Bekende problemen in de Update 0.5

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
| **8.** |Capaciteitsplanning voor bestandsshares |U ziet mogelijk verbruik delen wanneer er geen gegevens op de share zijn. Dit verbruik is omdat de gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen naar een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere versie. Ga voor meer informatie naar [Failover en herstel na noodgevallen voor uw StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |De virtuele StorSimple-apparaten worden niet beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden gedaan via de Azure-portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in en-us indeling op het toetsenbord. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Bovendien, als u de CHAP-referenties wijzigt, moet u zet de volumes offline en brengt u deze online voor de wijziging door te voeren. |Dit probleem is opgelost in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kunnen mogelijk verschillen in de StorSimple Device Manager-service en de iSCSI-host. |De iSCSI-host is de weergave van het bestandssysteem.<br></br>Het apparaat ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map is een alternatieve gegevens Stream (AD) die ermee verbonden zijn, wordt de ADVERTENTIES niet een back-up of via herstel na noodgevallen, klonen en herstel op itemniveau wordt hersteld. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het resultaat van de StorSimple Virtual Array bestand server in een niet-ondersteunde configuratie.  | |

## <a name="next-step"></a>Volgende stap
[Update 0.5 installeren](storsimple-virtual-array-install-update-05.md) op uw StorSimple Virtual Array.

## <a name="references"></a>Verwijzingen
Wilt u een oudere releaseopmerking? Ga naar:

* [StorSimple Virtual Array Update 0.4 Release Notes](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Virtual Array Update 0.3 Release Notes](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 en 0.2 opmerkingen bij de Release](storsimple-ova-update-01-release-notes.md)
* [Opmerkingen bij de Release van StorSimple Virtual Array algemene beschikbaarheid](storsimple-ova-pp-release-notes.md)

