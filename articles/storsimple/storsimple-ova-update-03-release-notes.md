---
title: StorSimple Virtual Array Updates opmerkingen bij de release | Microsoft Docs
description: Beschrijving van kritieke openstaande problemen en oplossingen voor de StorSimple Virtual Array Update 0.3 wordt uitgevoerd.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: 635b5f4edf5d403c569b4957540fc105997b3e8e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117670"
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple Virtual Array Update 0.3 release-opmerkingen
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen identificeren de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure StorSimple Virtual Array updates.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw StorSimple Virtual Array implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0.3 komt overeen met de softwareversie **10.0.10288.0**.

> [!NOTE]
> Updates opnieuw moet worden opgestart en uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, heeft het apparaat leidt tot downtime.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Wat is er nieuw in de Update 0.3
Update 0.3 is voornamelijk een opgelost probleem build. In deze versie zijn zijn meerdere fouten, wat resulteert in mislukte back-ups in de vorige versie opgelost.

## <a name="issues-fixed-in-the-update-03"></a>Problemen in de Update 0.3 opgelost
De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-ups |Een probleem is gedetecteerd in de eerdere versie wanneer de back-ups mislukken om voor een bestandsshare te voltooien. Als dit probleem is opgetreden, mislukken de back-uptaak en een kritieke waarschuwing is gegenereerd op de StorSimple Manager-service voor de gebruiker waarschuwen. Dit probleem heeft geen invloed op de gegevens op de shares of toegang tot de gegevens. De hoofdoorzaak is geïdentificeerd en opgelost in deze release. <br></br> De oplossing geldt niet met terugwerkende kracht voor shares die dit probleem al zien. Klanten die dit probleem ziet moeten eerst Update 0.3 toepassen en vervolgens contact op met Microsoft Support om uit te voeren een volledige back-up om het probleem te verhelpen. In plaats van het contact opneemt met Microsoft Support, kunnen klanten ook herstellen naar een nieuwe share vanuit een goede back-up voor de betrokken shares. |
| 2 |iSCSI |Een probleem is gedetecteerd in de eerdere versie wanneer de volumes verdwijnen bij het kopiëren van gegevens naar een volume op de StorSimple Virtual Array. Dit probleem is opgelost in deze release. <br></br> De oplossingen van kracht alleen op volumes van een nieuw gemaakt. De oplossingen niet van toepassing met terugwerkende kracht op volumes die dit probleem al zien. Klanten wordt aangeraden om te zetten van de betreffende volumes online via de klassieke Azure portal, het uitvoeren van een back-up voor deze volumes en herstel vervolgens deze volumes op nieuwe volumes. |

## <a name="known-issues-in-the-update-03"></a>Bekende problemen in de Update 0.3
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

## <a name="next-step"></a>Volgende stap
[Update 0.3 installeren](storsimple-ova-install-update-01.md) op uw StorSimple Virtual Array.

## <a name="references"></a>Verwijzingen
Wilt u een oudere releaseopmerking? Ga naar: 

* [StorSimple Virtual Array Update 0.1 en 0.2 opmerkingen bij de Release](storsimple-ova-update-01-release-notes.md)
* [Opmerkingen bij de Release van StorSimple Virtual Array algemene beschikbaarheid](storsimple-ova-pp-release-notes.md)

