---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 350dbc286c060ad10c21ebe1e8715b910871f837
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164554"
---
<!--author=alkohli last changed: 11/02/17 -->

#### <a name="to-install-updates-via-the-azure-portal"></a>Updates installeren via Azure Portal

1. Ga naar uw StorSimple Device Manager en selecteer **Apparaten**. Selecteer in de lijst met apparaten die zijn verbonden met uw service, het apparaat dat u wilt bijwerken.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. Klik op de blade **Instellingen** op **Apparaatupdates**.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. Als er software-updates beschikbaar zijn, wordt er een bericht weergegeven. U kunt ook op **Scannen** klikken om naar updates te zoeken. Noteer de versie van de software die u uitvoert. 

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    U krijgt een melding wanneer de scan wordt gestart en voltooid is.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. Nadat de updates zijn gescand, klikt u op **Updates downloaden**.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. In de **nieuwe updates** blade opmerkingen bij de release bekijken. Let ook op nadat de updates zijn gedownload, moet u de installatie bevestigen. Klik op **OK**.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. U ontvangt een melding wanneer de upload wordt gestart en wanneer deze is voltooid.

     ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. Klik op de blade **Apparaatupdates** op **Installeren**.

     ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. Op de blade **Nieuwe updates** krijgt u een waarschuwing dat de update verstorend is. Als de virtuele matrix een apparaat met een enkel knooppunt is, wordt het apparaat opnieuw opgestart nadat het is bijgewerkt. Dit verstoort alle actieve IO. Klik op **OK** om de updates te installeren.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. U ontvangt een melding wanneer de installatietaak wordt gestart.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Nadat de installatietaak is voltooid, klikt u op de koppeling **Taak weergeven** op de blade **Apparaatupdates** om de installatie te controleren. 

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Deze actie gaat u naar de **Updates installeren** blade. Hier kunt u gedetailleerde informatie over de taak weergeven.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Als u begint met een virtuele matrix met softwareversie Update 0.6 (10.0.10293.0), kunt u nu Update 1 worden uitgevoerd en klaar bent. U kunt de resterende stappen overslaan. Als u begint met een virtuele matrix met een softwareversie vóór Update 0.6 (10.0.10293.0), wordt u nu bijgewerkt met Update 0.6. Ziet u een ander bericht weergegeven dat aangeeft dat er updates beschikbaar zijn. Herhaal de stappen 4 tot 8 Update 1 te installeren.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

