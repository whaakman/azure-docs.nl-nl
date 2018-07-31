---
title: Opmerkingen bij de release van de StorSimple Virtual Array Update 1.1 | Microsoft Docs
description: Beschrijving van kritieke openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 1.1 uitgevoerd.
services: storsimple
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2018
ms.author: alkohli
ms.openlocfilehash: d73f45ec5ff1ffbe207fc45a1f87dcbe4f8ff021
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348321"
---
# <a name="storsimple-virtual-array-update-11-release-notes"></a>Opmerkingen bij de release van de StorSimple Virtual Array Update 1.1

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw StorSimple Virtual Array implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 1.1 komt overeen met de softwareversie **10.0.10307.0**.

> [!IMPORTANT]
> - Updates opnieuw moet worden opgestart en uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, heeft het apparaat leidt tot downtime. Ga voor gedetailleerde instructies voor het toepassen van de update naar [bijwerken 1.1 installeren](storsimple-virtual-array-install-update-11.md).
>
> - 1.1 update is beschikbaar voor u via de Azure-portal alleen als uw apparaat Update 1.0 wordt uitgevoerd.

## <a name="whats-new-in-update-11"></a>Wat is er nieuw in Update 1.1

Deze update bevat de volgende uitbreiding en oplossingen voor problemen:

 - **Back-up van fouten** -zijn verbeterd doordat bestendigheid met betrekking tot cloud-fouten en hoog CPU-verbruik.
 - **Logboekregistratie** -wijzigingen zijn aangebracht in de modus voor uitgebreide logboekregistratie wanneer het apparaat zich in de sessie voor ondersteuning.


## <a name="issues-fixed-in-update-11"></a>Problemen in 1.1 Update worden opgelost

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-ups| Deze release bevat wijzigingen die de back-upfouten zijn verbeterd doordat bestendigheid met betrekking tot cloud-fouten en hoog CPU-gebruik.|
| 2 |Logboekregistratie| Deze release bevat wijzigingen in de logboekregistratie terwijl het apparaat zich in Support-sessie in de uitgebreide modus.|


## <a name="known-issues-in-update-11"></a>Bekende problemen in Update 1.1

De volgende tabel bevat een overzicht van de bekende problemen voor de StorSimple Virtual Array en bevat de problemen release hebt genoteerd uit de vorige versies.

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele matrices die zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie voor algemene beschikbaarheid. |Deze virtuele matrices moeten failover voor de algemeen beschikbare versie met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Nadat u een gegevensschijf van een bepaalde opgegeven grootte hebt ingericht en de bijbehorende StorSimple Virtual Array hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Er wordt geprobeerd om te doen, resulteert in een verlies van de gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat is het domein, toepassen van een Groepsbeleid kan nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele array kan in een eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale web-UI |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige lokale web-UI-pagina's, zoals probleemoplossing of onderhoud niet goed werkt. Knoppen op deze pagina's kunnen ook niet werken. |Geavanceerde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale web-UI |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-UI worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. Hyper-V bevat altijd 10 Gbps voor virtuele netwerkadapters. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing opnemen in de lokaal vastgemaakte volumes in plaats van gelaagde volumes.<br></br>*Voorbehoud*: wanneer met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, het lokaal vastgemaakte volume online kan worden voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet klikt u vervolgens u wachten voor de herstelbewerking is voltooid. |
| **7.** |Gelaagde bestandsshares |Werken met grote bestanden kan leiden tot trage laag uitschalen. |Als u werkt met grote bestanden, wordt u aangeraden het grootste bestand kleiner is dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteitsplanning voor bestandsshares |U ziet mogelijk verbruik delen wanneer er geen gegevens op de share zijn. Dit verbruik is omdat de gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen naar een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere versie. Ga voor meer informatie naar [Failover en herstel na noodgevallen voor uw StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |De virtuele StorSimple-matrices kan niet worden beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden gedaan via de Azure-portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in en-us indeling op het toetsenbord. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Bovendien, als u de CHAP-referenties wijzigt, moet u zet de volumes offline en brengt u deze online voor de wijziging door te voeren. |Dit probleem is opgelost in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kunnen mogelijk verschillen in de StorSimple Device Manager-service en de iSCSI-host. |De iSCSI-host is de weergave van het bestandssysteem.<br></br>Het apparaat ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map is een alternatieve gegevens Stream (AD) die ermee verbonden zijn, wordt de ADVERTENTIES niet een back-up of via herstel na noodgevallen, klonen en herstel op itemniveau wordt hersteld. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het resultaat van de StorSimple Virtual Array bestand server in een niet-ondersteunde configuratie.  | |
| **17.** |Updates |Als u deze fout ziet code: 2359302 (hex 0x240006) bij een poging om een hotfix via de gebruikersinterface voor het lokaal te installeren, klikt u vervolgens dit houdt in dat de hotfix al is geïnstalleerd op uw apparaat.   | |
| **18.** |Updates |Als u de lokale webgebruikersinterface Update 1 installeren op uw virtuele array gebruikt, moet u ervoor zorgen dat u Update 0.6 worden uitgevoerd. Als u een versie lager is dan Update 0.6, moet u eerst Update 0.6 installeren en vervolgens toepassen Update 1. Als u Update 1.0 voor het rechtstreeks vanaf een 0,6 versie vóór het bijwerken installeren, klikt u vervolgens wordt u bepaalde updates gemist en de bewakingsgrafieken werkt niet.   | |


## <a name="next-steps"></a>Volgende stappen
[Installeren van Update 1.1](storsimple-virtual-array-install-update-11.md) op uw StorSimple Virtual Array.

## <a name="references"></a>Verwijzingen
Wilt u een oudere releaseopmerking? Ga naar:
* [Opmerkingen bij de StorSimple Virtual Array Update 1.0 Release](storsimple-virtual-array-update-1-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 0.6 Release](storsimple-virtual-array-update-06-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 0.5 Release](storsimple-virtual-array-update-05-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 0.4 Release](storsimple-virtual-array-update-04-release-notes.md)
* [Opmerkingen bij de StorSimple Virtual Array Update 0.3 Release](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 en 0.2 opmerkingen bij de Release](storsimple-ova-update-01-release-notes.md)
* [Opmerkingen bij de Release van StorSimple Virtual Array algemene beschikbaarheid](storsimple-ova-pp-release-notes.md)
