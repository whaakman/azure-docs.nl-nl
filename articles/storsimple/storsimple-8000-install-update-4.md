---
title: Update 4 installeren op StorSimple 8000 series apparaat | Microsoft Docs
description: Legt uit hoe StorSimple 8000 Series Update 4 installeren op uw StorSimple 8000 serie-apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 57d6d63c55f8ad4da5d1905a1e209da454b0491c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>Update 4 installeren op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u Update 4 installeren op een StorSimple-apparaat waarop een eerdere softwareversie wordt uitgevoerd via de Azure-portal en het gebruik van de hotfix-methode. De hotfix-methode wordt gebruikt wanneer een gateway is geconfigureerd op een andere netwerkinterface dan DATA 0 van de StorSimple-apparaat en u probeert bij te werken vanaf een versie 1 vóór het bijwerken.

Update 4 bevat device-software, USM firmware LSI stuurprogramma en firmware, Storport en Spaceport, OS beveiligingsupdates en tal van andere updates voor het besturingssysteem.  Software van het apparaat, USM firmware Spaceport, Storport en andere updates OS ononderbroken updates beschikbaar zijn. De ononderbroken of regelmatige updates kunnen worden toegepast via de Azure-portal of de hotfix-methode. De schijf firmware-updates verstoren updates beschikbaar zijn en kunnen alleen worden toegepast via de hotfix-methode met de Windows PowerShell-interface van het apparaat.

> [!IMPORTANT]
> * Een aantal handmatige en automatische eerste controles worden uitgevoerd voordat de installatie om te bepalen van de apparaatstatus in termen van hardware-staat en de netwerkverbinding. Deze controles vooraf worden alleen uitgevoerd als u de updates vanuit de Azure-portal toepassen.
> * Het is raadzaam dat u de software en andere regelmatig updates via de Azure-portal installeert. U moet alleen gaat u naar de Windows PowerShell-interface van het apparaat (om updates te installeren) als de controle van de gateway vóór het bijwerken is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, de updates kunnen 4 uur duren (of hoger) te installeren. De updates van de modus onderhoud moeten ook worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Als onderhoud modus updates verstoren updates beschikbaar zijn, resulteert dit in een uitvaltijd voor uw apparaat.
> * Als de optionele StorSimple Snapshot Manager wordt uitgevoerd, zorg ervoor dat u uw Snapshot Manager-versie hebt bijgewerkt naar Update 4 vóór het bijwerken van het apparaat.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Update 4 installeren via de Azure-portal
Voer de volgende stappen uit voor het bijwerken van uw apparaat om te [Update 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Microsoft haalt aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan als onze teamleden identificeert de apparaten die problemen ondervindt, zijn wij beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Controleren of uw apparaat wordt uitgevoerd **StorSimple 8000 Series Update 4 (6.3.9600.17820)**. De **datum laatst bijgewerkt** moet ook worden gewijzigd.

* U ziet nu de onderhoud modus updates zijn beschikbaar (dit bericht kan nog wel weergegeven tot 24 uur nadat u de updates hebt geïnstalleerd). Onderhoud modus updates zijn verstoren updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat.

* De modus onderhoud updates downloaden met behulp van de stappen in [downloaden hotfixes](#to-download-hotfixes) zoeken naar en downloaden van KB4011837, welke schijf firmware-updates zijn geïnstalleerd (de andere updates al is geïnstalleerd door nu). Volg de stappen die worden vermeld in [installeren en testen van onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) updates voor het installeren van de onderhoudsmodus.

## <a name="install-update-4-as-a-hotfix"></a>Update 4 installeren als een hotfix
De aanbevolen methode voor het installeren van Update 4 is via de Azure-portal.

Gebruik deze procedure als u de controle van de gateway niet bij het installeren van de updates via de Azure portal. De controle mislukt als er een gateway die is toegewezen aan een niet-DATA 0-netwerkinterface en uw apparaat een softwareversie dan Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt met de hotfix-methode zijn:

* Update 0.1, 0,2, 0,3
* Update 1, 1.1, 1.2
* 2, 2.1, 2.2 bijwerken
* Update 3, 3.1


De hotfix-methode omvat de volgende drie stappen:

1. De hotfixes downloaden vanaf Microsoft Update-catalogus.
2. Installeren en controleer of de hotfixes normale modus.
3. Installeren en controleer of de hotfix onderhoud-modus.

#### <a name="download-updates-for-your-device"></a>Updates voor uw apparaat downloaden

U moet downloaden en installeren van de volgende hotfixes in de aangegeven volgorde en de voorgestelde mappen:

| Volgorde | KB | Beschrijving | Updatetype | Installatietijd |Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Software-update |Reguliere <br></br>Niet-verstoren |~ 25 minuten |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI stuurprogramma's en firmware-updates <br> USM firmware-update (versie 3.38) |Reguliere <br></br>Niet-verstoren |~ 3 uur <br> (inclusief 2A. + 2B. (+ 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |OS-beveiligingspakket updates <br> Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstoren |- |SecondOrderUpdate|
| 2C. |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |OS-updates-pakket <br> Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstoren |- |SecondOrderUpdate|

U moet mogelijk ook installeren schijf firmware-updates boven op de updates die zijn aangegeven in de voorgaande tabellen. U kunt controleren of u de schijf firmware-updates door het uitvoeren van moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, en u niet wilt dat deze updates te installeren.

| Volgorde | KB | Beschrijving | Updatetype | Installatietijd | Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Schijf-firmware |Onderhoud <br></br>Verstoren |~ 30 minuten | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Deze procedure moet slechts één keer worden uitgevoerd om toe te passen Update 4. De Azure-portal kunt u latere updates toe te passen.
> * Als het bijwerken van de Update 3 of 3.1, is de totale installatietijd bijna 4 uur.
> * Voordat u deze procedure de update toe te passen, zorg ervoor dat zowel de apparaatcontrollers online zijn en alle hardwareonderdelen zijn in orde.

De volgende stappen uitvoeren om te downloaden en installeren van hotfixes.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Update 4 release](storsimple-update4-release-notes.md).

