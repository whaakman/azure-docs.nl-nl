---
title: Update 5 installeren op StorSimple 8000-apparaat | Microsoft Docs
description: Wordt uitgelegd hoe u StorSimple 8000 serie Update 5 op uw StorSimple 8000-apparaat installeert.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: d6e17c7609fd41b8f4457edda373f6882a1a9d2b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38698707"
---
# <a name="install-update-5-on-your-storsimple-device"></a>Update 5 installeren op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u Update 5 installeren op een StorSimple-apparaat waarop een eerdere softwareversie wordt uitgevoerd via Azure portal en het gebruik van de hotfix-methode. De hotfix-methode wordt gebruikt wanneer u probeert te installeren Update 5 op een apparaat met 3 versies vóór het bijwerken. De hotfix-methode wordt ook gebruikt als een gateway is geconfigureerd op een andere netwerkinterface dan DATA 0 van het StorSimple-apparaat en u probeert bij te werken vanuit een softwareversie van pre-update 1.

Update 5 bevat software van apparaten, Storport en Spaceport, OS-beveiligingsupdates en updates van het besturingssysteem en updates van de schijffirmware.  Software van het apparaat, Spaceport Storport, beveiliging en andere updates van het besturingssysteem niet-storende updates beschikbaar zijn. De niet-storende of regelmatige updates kunnen worden toegepast via Azure portal of via de hotfix-methode. Updates van de schijffirmware zijn updates waarvoor de computer en worden toegepast wanneer het apparaat zich in de onderhoudsmodus via de hotfix-methode met behulp van de Windows PowerShell-interface van het apparaat.

> [!IMPORTANT]
> * Update 5 is een verplichte update en moet onmiddellijk worden geïnstalleerd. Zie voor meer informatie, [opmerkingen bij de release van Update 5](storsimple-update5-release-notes.md).
> * Een set handmatige en automatische controles van systeemtabel moeten worden uitgevoerd voordat de installatie om te bepalen van de apparaatstatus in termen van hardware-staat en de netwerkverbinding. Deze controles van systeemtabel worden alleen uitgevoerd als u de updates vanuit Azure portal toepassen.
> * Het is raadzaam dat tijdens het bijwerken van een apparaat met versies vóór Update 3, u de updates installeren met behulp van de hotfix-methode. Als u problemen ondervindt, [een ondersteuningsticket](storsimple-8000-contact-microsoft-support.md).
> * U wordt aangeraden dat u de software en andere regelmatig updates via de Azure-portal installeert. U moet alleen naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) gaan als de gatewaycontrole vóór het bijwerken is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, de updates kunnen 4 uur duren (of hoger) om te installeren. Updates van de onderhoudsmodus moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Updates voor de onderhoudsmodus zijn updates waarvoor de computer, wordt deze resulteren in een uitvaltijd voor uw apparaat.
> * Als de optionele StorSimple Snapshot Manager uitvoert, controleert u of u uw Snapshot Manager-versie hebt bijgewerkt naar de Update 5 vóór het bijwerken van het apparaat.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Update 5 installeren via de Azure-portal
De volgende stappen uitvoeren om uw apparaat bijwerken [Update 5](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft haalt aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan bij ons team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Controleren of uw apparaat wordt uitgevoerd **StorSimple 8000 serie Update 5 (6.3.9600.17845)**. De **datum laatst bijgewerkt** moet worden gewijzigd.

U ziet nu dat de updates voor de onderhoudsmodus beschikbaar zijn (dit bericht kan nog wel weergegeven voor het tot 24 uur nadat u de updates hebt geïnstalleerd). De stappen voor het installeren van onderhoudsmodusupdate worden beschreven in de volgende sectie.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Update 5 installeren als een hotfix

De softwareversies die kunnen worden bijgewerkt met de hotfix-methode zijn:

* Update 0.1, 0.2, 0.3
* Update 1, 1.1, 1.2
* Update 2, 2.1, 2.2
* Update 3, 3.1
* Update 4

> [!NOTE] 
> De aanbevolen methode voor het installeren van Update 5 is via Azure portal tijdens het bijwerken van Update 3 en hoger. Gebruik deze procedure bij het bijwerken van een apparaat met Update 3 voor eerdere versies. U kunt deze procedure ook gebruiken als u de gatewaycontrole mislukt bij het installeren van de updates via de Azure-portal. De controle mislukt wanneer u een gateway die is toegewezen aan een niet-DATA 0-netwerkinterface hebt en uw apparaat een versie ouder dan Update 1 wordt uitgevoerd.

De hotfix-methode omvat de volgende drie stappen:

1. De hotfixes downloaden vanaf Microsoft Update-catalogus.
2. Hotfixes installeren en controleren de normale modus.
3. Installeren en controleer of de hotfix van de onderhoudsmodus.

#### <a name="download-updates-for-your-device"></a>Downloaden van updates voor uw apparaat

U moet downloaden en installeren van de volgende hotfixes in de aangegeven volgorde en de voorgestelde mappen:

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie |Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Software-update<br> Download beide _HcsSoftwareUpdate.exe_ en _CisMSDAgent.exe_ |Reguliere <br></br>Niet-verstorend |~ 25 minuten |FirstOrderUpdate|

Als het bijwerken van een apparaat met Update 4, moet u alleen voor het installeren van de cumulatieve updates van het besturingssysteem als de tweede belangrijkste updates.

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie |Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |Besturingssysteem van de cumulatieve updates pakket <br> Versie van Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstorend |- |SecondOrderUpdate|

Als installeren vanaf een apparaat met Update 3 of eerder, installeert u de volgende naast de cumulatieve updates.

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie |Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 2B HEBT OPGEZOCHT. |KB4011841 <br> KB4011842 |LSI-stuurprogramma's en firmware-updates <br> Voor Gebruikersbeheer firmware-update (versie 3.38) |Reguliere <br></br>Niet-verstorend |~ 3 uur <br> (inclusief 2A. + 2B HEBT OPGEZOCHT. (+ 2 C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |OS-updates-beveiligingspakket <br> Versie van Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstorend |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |OS-updates-pakket <br> Versie van Windows Server 2012 R2 downloaden |Reguliere <br></br>Niet-verstorend |- |SecondOrderUpdate|


U moet mogelijk ook installeren van updates van de schijffirmware boven op alle updates die wordt weergegeven in de voorgaande tabellen. U kunt controleren of u de schijf firmware-updates door te voeren moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, en u niet hoeft om deze updates te installeren.

| Volgorde | KB | Beschrijving | Updatetype | Tijd van installatie | Installeren in map|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Schijffirmware |Onderhoud <br></br>Verstorend |~ 30 mins | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Als het bijwerken van de Update 4, is de totale installatietijd dicht bij 4 uur.
> * Voordat u deze procedure de update toe te passen, zorg ervoor dat zowel de apparaatcontrollers online zijn en alle hardwareonderdelen in orde zijn.

Voer de volgende stappen uit om te downloaden en installeren van de hotfixes.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [release van Update 5](storsimple-update5-release-notes.md).

