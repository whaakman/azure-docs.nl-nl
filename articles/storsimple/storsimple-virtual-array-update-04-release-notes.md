---
title: StorSimple virtuele matrix Update 0,4 release-opmerkingen | Microsoft Docs
description: Kritieke open problemen en oplossingen beschrijft voor de virtuele StorSimple-matrix met Update 0,4.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: cc2b025b7f3e28954c7f95409ffab03e5cbcf13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple virtuele matrix Update 0,4 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Voordat u uw virtuele StorSimple-matrix implementeert, zorgvuldig door de gegevens in de release-opmerkingen.

Update 0,4 komt overeen met de softwareversie **10.0.10289.0**.

> [!NOTE]
> Updates kunnen verstoren, start het apparaat. Als i/o's uitgevoerd worden, leidt uitvaltijd ertoe dat het apparaat.


## <a name="whats-new-in-the-update-04"></a>Wat is er nieuw in de Update 0,4
Update 0,4 is voornamelijk een bug fix build samen met enkele verbeteringen. In deze versie, is enkele fouten, wat leidt tot mislukte back-ups in de vorige versie behandeld. De belangrijkste verbeteringen en oplossingen voor problemen zijn als volgt:

- **Back-up prestatieverbeteringen** -verschillende belangrijke verbeteringen voor het verbeteren van de back-upprestaties heeft gemaakt, worden deze release. De back-ups waarbij een groot aantal bestanden zien als gevolg hiervan een significante vermindering van de tijd in beslag voor volledige en incrementele back-ups.

- **Verbeterde prestaties terugzetten** -deze release bevat verbeteringen in de restore-prestaties aanzienlijk verbeteren wanneer u veel bestanden. Als 2-4 miljoen bestanden gebruikt, raden wij aan dat u een virtuele-matrix met 16 GB RAM-geheugen voor een overzicht van de verbeteringen inrichten. Wanneer u minder dan 2 miljoen bestanden gebruikt, blijft de minimumvereiste voor de virtuele machine worden 8 GB RAM-geheugen.

- **Verbeteringen in ondersteuningspakket** -de verbeteringen omvatten logboekregistratie in de statistieken voor de schijf, CPU, geheugen, netwerk- en cloud in de ondersteuningspakket waardoor het proces van het apparaat problemen diagnosticeren/foutopsporing te verbeteren.

- **Limiet lokaal vastgemaakt iSCSI-volumes 200 GB** -voor lokaal vastgemaakte volumes is het raadzaam dat u tot een 200 GB iSCSI-volume op uw virtuele StorSimple-matrix beperkt. De lokale reservering voor gelaagde volumes blijft 10% van de grootte van het betreffende volume, maar is beperkt tot 200 GB. 

- **Back-up-gerelateerde oplossingen voor problemen** - In eerdere versies van de software, er zijn problemen met betrekking tot back-ups die leiden mislukte back-ups tot zou. Deze fouten zijn gericht in deze release.


## <a name="issues-fixed-in-the-update-04"></a>Problemen in de Update 0,4 opgelost

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-upprestaties|In de eerdere versies, zou de back-ups met betrekking tot groot aantal bestanden lang duren om uit te voeren (in volgorde van dagen). In deze release zien zowel de volledige en incrementele back-ups een significante vermindering van de tijd voor voltooiing. |
| 2 |Ondersteuningspakket|Schijf, CPU, geheugen, netwerk en statistieken van de cloud zijn nu aangemeld bij de ondersteuningspakketten zeer effectief zijn bij het oplossen van problemen apparaat waardoor logboeken van de ondersteuning.|
| 3 |Back-up |In eerdere versies kan langlopende back-ups leiden tot een crisis ruimte op het apparaat waardoor mislukte back-ups. Deze fout wordt beschreven in deze release doordat niet meer dan 5 back-ups in één keer in de wachtrij.|
| 4 |iSCSI | In eerdere versies is de lokale reservering voor gelaagde of lokaal vastgemaakte volumes 10% van de grootte van het betreffende volume. In deze release de lokale reservering voor alle iSCSI-volumes (lokaal vastgemaakt of gelaagde) is beperkt tot 10% met een maximum van maximaal 200 GB (voor gelaagde volumes die groter is dan 2 TB) waardoor vrijmaken meer ruimte op de lokale schijf. U wordt aangeraden dat lokaal vastgemaakte volumes in deze release beperkt tot 200 GB zijn.|


## <a name="known-issues-in-the-update-04"></a>Bekende problemen in de Update 0,4

De volgende tabel bevat een samenvatting van bekende problemen voor de virtuele StorSimple-matrix en omvat problemen met de release genoteerd uit de vorige versies. 

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele apparaten die zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. |Deze virtuele apparaten moeten failover worden uitgevoerd voor de algemene beschikbaarheid release met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Als u een gegevensschijf van een bepaalde opgegeven omvang hebt ingericht en de bijbehorende virtuele StorSimple-apparaat hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Wilt doen resulteert in een verlies van alle gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat lid van een domein is, toepassen van een Groepsbeleid kunt nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale webgebruikersinterface |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige gebruikersinterface lokale webpagina's zoals probleemoplossing of onderhoud niet correct werkt. Knoppen op deze pagina's ook werkt mogelijk niet. |Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale webgebruikersinterface |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-gebruikersinterface worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. 10 Gbps voor virtuele netwerkadapters worden altijd weergegeven in Hyper-V. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bereik aan bytes is vergrendeld in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing in lokaal vastgemaakte volumes in plaats van gelaagde volumes plaatsen.<br></br>*Hoewel*: wanneer met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, lokaal vastgemaakt volume online kan worden voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens wachten op de herstelbewerking is voltooid. |
| **7.** |Gelaagde shares |Werken met grote bestanden kan leiden tot een trage laag. |Wanneer u werkt met grote bestanden, raden we aan dat het grootste bestand kleiner dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteit voor shares gebruikt |Mogelijk ziet u verbruik delen wanneer er geen gegevens op de share zijn. Dit is omdat het gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen op een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere release. |
| **10.** |Azure PowerShell |Het virtuele StorSimple-apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden uitgevoerd via de klassieke Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de indeling en-US. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de CHAP-referenties wijzigt, moet u offline zetten van de volumes en brengt u ze online om de wijziging door te voeren. |Dit probleem is verholpen in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kan niet verschillen in de StorSimple Manager-service en de iSCSI-host. |De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map heeft een alternatieve gegevens stroom (ADS) gekoppeld, wordt de ADVERTENTIES geen back-up gemaakt of hersteld via herstel na noodgevallen, klonen en herstel Item. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het virtuele StorSimple-matrix bestand server resulteren in een niet-ondersteunde configuratie.  | |

## <a name="next-step"></a>Volgende stap
[Installeer Update 0,4](storsimple-virtual-array-install-update-04.md) op uw virtuele StorSimple-matrix.

## <a name="references"></a>Verwijzingen
Zoekt u een oudere versie Opmerking? Ga naar: 

* [StorSimple virtuele matrix Update 0,3 Release-opmerkingen](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuele matrix Update 0,1-0,2 Release-opmerkingen](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuele matrix algemene beschikbaarheid Release-opmerkingen](storsimple-ova-pp-release-notes.md)

