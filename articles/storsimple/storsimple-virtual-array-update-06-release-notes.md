---
title: StorSimple virtuele matrix Update 0,6 release-opmerkingen | Microsoft Docs
description: Kritieke open problemen en oplossingen beschrijft voor de virtuele StorSimple-matrix met Update 0,6.
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
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: af202cf652300ee7897eb2dede33f38058fc2837
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a>StorSimple virtuele matrix Update 0,6 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Voordat u uw virtuele StorSimple-matrix implementeert, zorgvuldig door de gegevens in de release-opmerkingen.

Update 0,6 komt overeen met de softwareversie **10.0.10293.0**.

> [!IMPORTANT]
> - Updates kunnen verstoren, start het apparaat. Als i/o's uitgevoerd worden, leidt uitvaltijd ertoe dat het apparaat. Ga voor gedetailleerde instructies voor het toepassen van de update naar [installeert u Update 0,6](storsimple-virtual-array-install-update-06.md).
>
> - Het is raadzaam dat u 0,6 installeert onmiddellijk omdat het bevat kritieke beveiligingsproblemen.


## <a name="whats-new-in-the-update-06"></a>Wat is er nieuw in de Update 0,6
Update 0,6 is een essentiële update en onmiddellijk moet worden geïmplementeerd. Deze update bevat de volgende oplossingen: 

- **Windows-beveiliging corrigeert** -deze release is **kritieke beveiligingsproblemen Windows**. Controleer de volgende beveiligingsupdates voor meer informatie over de beveiligingsproblemen en de bijbehorende oplossingen:
    - [December 2016 beveiliging alleen kwaliteit Update voor Windows 8.1 en WindowsServer 2012 R2](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [Maart 2017 beveiliging alleen kwaliteit Update voor Windows 8.1 en WindowsServer 2012 R2](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [9 mei 2017: KB4019213 (beveiliging alleen-lezen-update)](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- **Herstellen van de oplossing** -In eerdere versies is een fout die voorkomen het herstel is voltooid dat. Deze fout is verholpen in deze release.


## <a name="issues-fixed-in-the-update-06"></a>Problemen in de Update 0,6 opgelost

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Beveiliging| Deze release bevat essentiële updates voor Windows-beveiliging. Het is raadzaam dat u deze update onmiddellijk te installeren.|
| 2 |Herstellen| Tijdens een terugzetbewerking is er een zeldzame situatie die voorkomen de hersteltaak voltooien dat. De oplossing bug lost deze race condition.|


## <a name="known-issues-in-the-update-06"></a>Bekende problemen in de Update 0,6

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
| **8.** |Capaciteit voor shares gebruikt |Mogelijk ziet u verbruik delen wanneer er geen gegevens op de share zijn. Dit verbruik is omdat het gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgevallen |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen op een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere release. Ga voor meer informatie naar [Failover en herstel na noodgevallen voor uw virtuele StorSimple-matrix](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |Het virtuele StorSimple-apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden uitgevoerd via de Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de norm en-us indeling toetsenbord. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de CHAP-referenties wijzigt, moet u offline zetten van de volumes en brengt u ze online om de wijziging door te voeren. |Dit probleem is verholpen in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kan niet verschillen in de Apparaatbeheer StorSimple-service en de iSCSI-host. |De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map heeft een alternatieve gegevens stroom (ADS) gekoppeld, wordt de ADVERTENTIES geen back-up gemaakt of hersteld via herstel na noodgevallen, klonen en herstel Item. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het virtuele StorSimple-matrix bestand server resulteren in een niet-ondersteunde configuratie.  | |
| **17.** |Updates |Als u fout ziet code: 2359302 (hex 0x240006) wanneer u probeert een hotfix via de lokale gebruikersinterface installeren, klikt u vervolgens dit betekent dat de hotfix al is geïnstalleerd op uw apparaat.   | |

## <a name="next-step"></a>Volgende stap
[Installeer Update 0,6](storsimple-virtual-array-install-update-06.md) op uw virtuele StorSimple-matrix.

## <a name="references"></a>Verwijzingen
Zoekt u een oudere versie Opmerking? Ga naar:

* [StorSimple virtuele matrix Update 0,5 Release-opmerkingen](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple virtuele matrix Update 0,4 Release-opmerkingen](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple virtuele matrix Update 0,3 Release-opmerkingen](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuele matrix Update 0,1-0,2 Release-opmerkingen](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuele matrix algemene beschikbaarheid Release-opmerkingen](storsimple-ova-pp-release-notes.md)

