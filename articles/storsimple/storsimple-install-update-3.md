---
title: Installeer Update 3 op uw StorSimple-apparaat | Microsoft Docs
description: Legt uit hoe StorSimple 8000 Series Update 3 op uw StorSimple 8000 series apparaat installeert.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72b004a6c2604e0fc20b71b4b69217622f8f9ea0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>Update 3 op uw StorSimple 8000 series apparaat installeert

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe Update 3 installeren op een StorSimple-apparaat waarop een eerdere softwareversie wordt uitgevoerd via de klassieke Azure portal en het gebruik van de hotfix-methode. De hotfix-methode wordt gebruikt wanneer een gateway is geconfigureerd op een andere netwerkinterface dan DATA 0 van de StorSimple-apparaat en u probeert bij te werken vanaf een versie 1 vóór het bijwerken.

Update 3 omvat apparaatsoftware, LSI stuurprogramma en firmware, Storport en Spaceport updates. Als het bijwerken van de Update 2 of een eerdere versie, kunt u zich ook vereist voor het toepassen van iSCSI, WMI, en in bepaalde gevallen schijf firmware-updates. De software voor apparaten, WMI iSCSI-, LSI stuurprogramma, Spaceport en Storport oplossingen ononderbroken updates beschikbaar zijn en kunnen worden toegepast via de klassieke Azure portal. De schijf firmware-updates verstoren updates beschikbaar zijn en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat. 

> [!IMPORTANT]
> * Een aantal handmatige en automatische eerste controles worden uitgevoerd voordat de installatie om te bepalen van de apparaatstatus in termen van hardware-staat en de netwerkverbinding. Deze controles vooraf worden alleen uitgevoerd als u de updates via de klassieke Azure portal toepassen.
> * Het is raadzaam dat u de software en stuurprogramma's via de klassieke Azure portal installeert. U moet alleen gaat u naar de Windows PowerShell-interface van het apparaat (om updates te installeren) als de controle van de gateway vóór het bijwerken is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, de updates kunnen uren duren voordat 1.5 2.5 installeren. De updates van de modus onderhoud moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Als onderhoud modus updates verstoren updates beschikbaar zijn, resulteert dit in een uitvaltijd voor uw apparaat.
> * Als de optionele StorSimple Snapshot Manager wordt uitgevoerd, zorg ervoor dat u uw Snapshot Manager-versie hebt bijgewerkt naar Update 2 vóór het bijwerken van het apparaat.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Update 3 installeren via de klassieke Azure portal
Voer de volgende stappen uit voor het bijwerken van uw apparaat om te [Update 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Als u bij het toepassen van Update 2 of hoger (inclusief Update 2.1), zich Microsoft voor het ophalen van aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan als onze teamleden identificeert de apparaten die problemen ondervindt, zijn wij beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren. Update 2 of hoger accepteren, kunnen wij deze proactieve ondersteuning.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Controleren of uw apparaat wordt uitgevoerd **StorSimple 8000 Series Update 3 (6.3.9600.17759)**. De **datum laatst bijgewerkt** moet ook worden gewijzigd. 
   - Als u een eerdere versie dan Update 2 bijwerkt, ook ziet u dat de modus onderhoud updates beschikbaar zijn (dit bericht kan nog wel weergegeven tot 24 uur nadat u de updates hebt geïnstalleerd).
     Onderhoud modus updates zijn verstoren updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u Update 1.2 uitvoert uw schijf firmware mogelijk al up-to-date zijn, in dat geval moet u geen onderhoud modus updates installeren.
   - Als u bijwerken van de Update 2 of hoger, het apparaat worden nu recente. U kunt de volgende stap overslaan.

De modus onderhoud updates downloaden met behulp van de stappen in [downloaden hotfixes](#to-download-hotfixes) zoeken naar en downloaden van KB3121899, welke schijf firmware-updates zijn geïnstalleerd (de andere updates al is geïnstalleerd door nu). Volg de stappen die worden vermeld in [installeren en testen van onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) updates voor het installeren van de onderhoudsmodus. 

## <a name="install-update-3-as-a-hotfix"></a>Update 3 installeren als een hotfix
Gebruik deze procedure als u de controle van de gateway niet bij het installeren van de updates via de klassieke Azure portal. De controle mislukt als er een gateway die is toegewezen aan een niet-DATA 0-netwerkinterface en uw apparaat een softwareversie dan Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt met de hotfix-methode zijn:

* Update 0.1, 0,2, 0,3
* Update 1, 1.1, 1.2
* 2, 2.1, 2.2 bijwerken 

> [!IMPORTANT]
> * Als uw apparaat kan Release (GA)-versie wordt uitgevoerd, contact op met [Microsoft Support](storsimple-contact-microsoft-support.md) om u te helpen met de update.
> 
> 

De hotfix-methode omvat de volgende drie stappen:

1. De hotfixes downloaden vanaf Microsoft Update-catalogus.
2. Installeren en controleer of de hotfixes normale modus.
3. Installeren en controleer of de hotfix onderhoud-modus (alleen bij het bijwerken van 2 software vóór het bijwerken).

#### <a name="download-updates-for-your-device"></a>Updates voor uw apparaat downloaden
**Als uw apparaat wordt uitgevoerd in de Update 2.1 of 2.2**, u moet downloaden en installeren van de volgende hotfixes in de aangegeven volgorde:

| Volgorde | KB | Beschrijving | Updatetype | Installatietijd |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Software-update &#42; |Reguliere <br></br>Niet-verstoren |~ 45 minuten |
| 2. |KB3186859 |LSI stuurprogramma's en -firmware |Reguliere <br></br>Niet-verstoren |~ 20 minuten |
| 3. |KB3121261 |Storport en Spaceport oplossing </br> Windows Server 2012 R2 |Reguliere <br></br>Niet-verstoren |~ 20 minuten |

&#42;  *Dat de software-update uit twee binaire bestanden bestaat Opmerking: de software-update apparaat vooraf worden gegaan door `all-hcsmdssoftwareupdate` en de configuratie-items en Mds-agent die vooraf worden gegaan door `all-cismdsagentupdatebundle`. De software-update van het apparaat moet worden geïnstalleerd voordat de configuratie-items en Mds-agent. U moet ook opnieuw starten de actieve controller via de `Restart-HcsController` cmdlet na het toepassen van de configuratie-items en Mds-agent bijwerken (en voordat de resterende toepassen van updates).* 

**Als uw apparaat Update 0.1, 0,2, 0,3, 1.0, 1.1, 1.2 of 2.0 wordt uitgevoerd**, u moet downloaden en installeren van de volgende hotfixes naast de software LSI stuurprogramma en firmware-updates (weergegeven in de voorgaande tabel), in de aangegeven volgorde:

| Volgorde | KB | Beschrijving | Updatetype | Installatietijd |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |iSCSI-pakket |Reguliere <br></br>Niet-verstoren |~ 20 minuten |
| 5. |KB3103616 |WMI-pakket |Reguliere <br></br>Niet-verstoren |~ 12 minuten |

<br></br>

**Als uw apparaat wordt uitgevoerd op versies 0,2, 0,3, 1.0, 1.1 en 1.2**, u moet mogelijk ook schijf firmware-updates boven op de updates die zijn aangegeven in de voorgaande tabellen te installeren. U kunt controleren of u de schijf firmware-updates door het uitvoeren van moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, en u niet wilt dat deze updates te installeren.

| Volgorde | KB | Beschrijving | Updatetype | Installatietijd |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Schijf-firmware |Onderhoud <br></br>Verstoren |~ 30 minuten |

<br></br>

> [!IMPORTANT]
> * Deze procedure moet slechts één keer worden uitgevoerd om toe te passen Update 3. U kunt de klassieke Azure portal gebruiken daaropvolgende updates toe te passen.
> * Als het bijwerken van de Update 2.2, is de totale installatietijd dicht bij 1.1 uur.
> * Voordat u deze procedure de update toe te passen, zorg ervoor dat zowel de apparaatcontrollers online zijn en alle hardwareonderdelen zijn in orde.
> 
> 

De volgende stappen uitvoeren om te downloaden en installeren van hotfixes.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [release van Update 3](storsimple-update3-release-notes.md).

