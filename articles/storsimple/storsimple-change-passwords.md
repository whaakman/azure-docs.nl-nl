---
title: Wijzigen van wachtwoorden via StorSimple Apparaatbeheer | Microsoft Docs
description: Beschrijft hoe de StorSimple Manager-service gebruiken om te wijzigen van de administrator-wachtwoord StorSimple Snapshot Manager en het apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d890b59595628ca3eeff1df258847c2bb54d29ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>De StorSimple Manager-service gebruiken om uw StorSimple-wachtwoorden wijzigen
## <a name="overview"></a>Overzicht
De klassieke Azure portal **configureren** pagina bevat alle apparaatparameters die u kunt opnieuw configureren op een StorSimple-apparaat dat wordt beheerd door een StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de **configureren** pagina om de apparaatbeheerder van uw of StorSimple Snapshot Manager-wachtwoord te wijzigen.

## <a name="change-the-device-administrator-password"></a>Het beheerderswachtwoord voor het apparaat wijzigen
Wanneer u Windows PowerShell-interface gebruiken voor toegang tot het StorSimple-apparaat, hoeft u een apparaat administrator-wachtwoord moeten invoeren. Als de eerste StorSimple-apparaat is geregistreerd met een service, wordt het standaardwachtwoord voor deze interface is *Wachtwoord1*. Voor de beveiliging van uw gegevens moet u dit wachtwoord aan het einde van het registratieproces wijzigen. U kunt niet afsluiten van het registratieproces zonder dat u dit wachtwoord wijzigt. Zie voor meer informatie [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat u eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan vervolgens worden gewijzigd via de klassieke Azure portal. De volgende stappen uitvoeren om te wijzigen van het beheerderswachtwoord van het apparaat.

#### <a name="to-change-the-device-administrator-password"></a>Het beheerderswachtwoord van het apparaat wijzigen
1. Klik in de klassieke portal **apparaten** > **configureren** voor uw apparaat.
2. Schuif omlaag naar de **apparaat beheerderswachtwoord** sectie. Geef een administrator-wachtwoord tussen 8 en 15 tekens bevat. Het wachtwoord moet een combinatie van 3 of meer van hoofdletters, kleine letters, numerieke en speciale tekens.
3. Bevestig het wachtwoord.
4. Klik op **Opslaan** onder aan de pagina.

Het beheerderswachtwoord van het apparaat moet nu worden bijgewerkt. U kunt dit gewijzigde wachtwoord gebruiken voor toegang tot de Windows PowerShell-interface.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Wijzig het wachtwoord voor StorSimple Snapshot Manager
De StorSimple Snapshot Manager-software bevindt zich op uw Windows-host. Met deze software kunnen beheerders de back-ups van uw StorSimple-apparaat beheren. De back-ups zijn in feite momentopnamen die lokaal en in de cloud worden opgeslagen.

Wanneer u een apparaat in StorSimple Snapshot Manager configureert, wordt u gevraagd de IP-adres en het wachtwoord voor verificatie van uw opslagapparaat opgeven. Dit wachtwoord wordt eerst worden geconfigureerd via de Windows PowerShell-interface. Zie voor meer informatie [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat u eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan vervolgens worden gewijzigd via de klassieke portal. Voer de volgende stappen uit om het StorSimple Snapshot Manager-wachtwoord te wijzigen.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Wijzig het wachtwoord voor StorSimple Snapshot Manager
1. Klik in de klassieke portal **apparaten** > **configureren** voor uw apparaat.
2. Schuif omlaag naar de **StorSimple Snapshot Manager** sectie. Voer een wachtwoord 14 of 15 tekens. Zorg ervoor dat het wachtwoord een combinatie van 3 of meer van hoofdletters, kleine letters, numerieke en speciale tekens bevat.
3. Bevestig het wachtwoord.
4. Klik op **Opslaan** onder aan de pagina.

Het wachtwoord voor StorSimple Snapshot Manager moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-security.md).
* Meer informatie over [wijzigen van de configuratie van uw apparaat](storsimple-modify-device-config.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

