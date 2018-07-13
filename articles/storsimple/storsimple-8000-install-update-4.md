---
title: Update 4 installeren op StorSimple 8000-apparaat | Microsoft Docs
description: Wordt uitgelegd hoe u StorSimple 8000 serie Update 4 installeren op uw StorSimple 8000-apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 57d6d63c55f8ad4da5d1905a1e209da454b0491c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38630191"
---
# <a name="install-update-4-on-your-storsimple-device"></a>Update 4 installeren op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u Update 4 installeren op een StorSimple-apparaat waarop een eerdere softwareversie wordt uitgevoerd via Azure portal en het gebruik van de hotfix-methode. De hotfix-methode wordt gebruikt als een gateway is geconfigureerd op een andere netwerkinterface dan DATA 0 van het StorSimple-apparaat en u probeert bij te werken vanuit een softwareversie van pre-update 1.

Update 4 bevat de software voor apparaten, voor Gebruikersbeheer firmware, LSI-stuurprogramma en firmware, Storport en Spaceport, security updates van het besturingssysteem en een groot aantal andere updates van het besturingssysteem.  Software van het apparaat, voor Gebruikersbeheer firmware, Spaceport, Storport en andere updates van het besturingssysteem niet-storende updates beschikbaar zijn. De niet-storende of regelmatige updates kunnen worden toegepast via Azure portal of via de hotfix-methode. Updates van de schijffirmware zijn updates waarvoor de computer en kunnen alleen worden toegepast via de hotfix-methode met behulp van de Windows PowerShell-interface van het apparaat.

> [!IMPORTANT]
> * Een set handmatige en automatische controles van systeemtabel moeten worden uitgevoerd voordat de installatie om te bepalen van de apparaatstatus in termen van hardware-staat en de netwerkverbinding. Deze controles van systeemtabel worden alleen uitgevoerd als u de updates vanuit Azure portal toepassen.
> * U wordt aangeraden dat u de software en andere regelmatig updates via de Azure-portal installeert. U moet alleen naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) gaan als de gatewaycontrole vóór het bijwerken is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, de updates kunnen 4 uur duren (of hoger) om te installeren. Updates van de onderhoudsmodus moeten ook worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Updates voor de onderhoudsmodus zijn updates waarvoor de computer, resulteert dit in een uitvaltijd voor uw apparaat.
> * Als de optionele StorSimple Snapshot Manager uitvoert, controleert u of u uw Snapshot Manager-versie hebt bijgewerkt naar Update 4 vóór het bijwerken van het apparaat.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Update 4 installeren via de Azure-portal
De volgende stappen uitvoeren om uw apparaat bijwerken [Update 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Microsoft haalt aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan bij ons team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Controleren of uw apparaat wordt uitgevoerd **StorSimple 8000 serie Update 4 (6.3.9600.17820)**. De **datum laatst bijgewerkt** moet ook worden gewijzigd.

* U ziet nu dat de updates voor de onderhoudsmodus beschikbaar zijn (dit bericht kan nog wel weergegeven voor het tot 24 uur nadat u de updates hebt geïnstalleerd). Updates voor de onderhoudsmodus zijn updates waarvoor de computer die leiden tot stilstand van apparaten en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat.

* Updates van de onderhoudsmodus downloaden met behulp van de stappen in [hotfixes downloaden](#to-download-hotfixes) zoeken naar en downloaden van KB4011837, welke installeert schijf firmware-updates (de overige updates al is geïnstalleerd door nu). Volg de stappen die worden vermeld in [installeren en controleren van hotfixes in de onderhoudsmodus](#to-install-and-verify-maintenance-mode-hotfixes) updates voor het installeren van de onderhoudsmodus.

## <a name="install-update-4-as-a-hotfix"></a>Update 4 installeren als een hotfix
De aanbevolen methode voor het installeren van Update 4 wordt via Azure portal.

Gebruik deze procedure als u de gatewaycontrole mislukt bij het installeren van de updates via de Azure-portal. De controle mislukt als u een gateway die is toegewezen aan een niet-DATA 0-netwerkinterface hebt en uw apparaat een versie ouder dan Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt met de hotfix-methode zijn:

* Update 0.1, 0.2, 0.3
* Update 1, 1.1, 1.2
* Update 2, 2.1, 2.2
* Update 3, 3.1


De hotfix-methode omvat de volgende drie stappen:

1. De hotfixes downloaden vanaf Microsoft Update-catalogus.
2. Hotfixes installeren en controleren de normale modus.
3. Installeren en controleer of de hotfix van de onderhoudsmodus.

#### <a name="download-updates-for-your-device"></a>Downloaden van updates voor uw apparaat

U moet downloaden en installeren van de volgende hotfixes in de aangegeven volgorde en de voorgestelde mappen:

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie |Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Software-update |Reguliere <br></br>Niet-verstorend |~ 25 minuten |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI-stuurprogramma's en firmware-updates <br> Voor Gebruikersbeheer firmware-update (versie 3.38) |Reguliere <br></br>Niet-verstorend |~ 3 uur <br> (inclusief 2A. + 2B HEBT OPGEZOCHT. (+ 2 C.)|SecondOrderUpdate|
| 2B HEBT OPGEZOCHT. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |OS-updates-beveiligingspakket <br> Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstorend |- |SecondOrderUpdate|
| 2C. |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |OS-updates-pakket <br> Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstorend |- |SecondOrderUpdate|

U moet mogelijk ook installeren van updates van de schijffirmware boven op alle updates die wordt weergegeven in de voorgaande tabellen. U kunt controleren of u de schijf firmware-updates door te voeren moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, en u niet hoeft om deze updates te installeren.

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie | Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Schijffirmware |Onderhoud <br></br>Verstorend |~ 30 mins | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Deze procedure moet slechts één keer worden uitgevoerd om toe te passen Update 4. De Azure-portal kunt u latere updates toe te passen.
> * Als het bijwerken van de Update 3 of 3.1, is de totale installatietijd dicht bij 4 uur.
> * Voordat u deze procedure de update toe te passen, zorg ervoor dat zowel de apparaatcontrollers online zijn en alle hardwareonderdelen in orde zijn.

Voer de volgende stappen uit om te downloaden en installeren van de hotfixes.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [release van Update 4](storsimple-update4-release-notes.md).

