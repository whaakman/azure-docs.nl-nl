---
title: bestand opnemen
description: bestand opnemen
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 07/18/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: a83095b8330ccf08d777e48389c17058c6d29527
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348358"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Updates installeren via Azure Portal

1. Ga naar uw StorSimple Device Manager en selecteer **Apparaten**. Selecteer in de lijst met apparaten die zijn verbonden met uw service, het apparaat dat u wilt bijwerken.

2. Onder **instellingen**, klikt u op **apparaatupdates**.  

3. Als er software-updates beschikbaar zijn, wordt er een bericht weergegeven. U kunt ook op **Scannen** klikken om naar updates te zoeken. Noteer de versie van de software die u uitvoert. 

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate3m1.png)

    U krijgt een melding wanneer de scan wordt gestart en voltooid is.
 
4. Nadat de updates zijn gescand, klikt u op **Updates downloaden**. Onder **nieuwe updates**, neem de releaseopmerkingen. Let ook op nadat de updates zijn gedownload, moet u de installatie bevestigen. Klik op **OK**.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate6m.png)

    U ontvangt een melding wanneer de upload wordt gestart en wanneer deze is voltooid.

5. Onder **apparaatupdates**, klikt u op **installeren**.

     ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate11m1.png)

6. Onder **nieuwe updates**, wordt u gewaarschuwd dat de update verstorend is. Als de virtuele matrix een apparaat met een enkel knooppunt is, wordt het apparaat opnieuw opgestart nadat het is bijgewerkt. Dit verstoort alle actieve IO. Klik op **OK** om de updates te installeren.

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate12m.png)

    U ontvangt een melding wanneer de installatietaak wordt gestart.

7.  Nadat de installatietaak voltooid is, klikt u op **taak weergeven** koppeling. Deze actie gaat u naar de **Updates installeren** blade. Hier kunt u gedetailleerde informatie over de taak weergeven. 

    ![apparaat bijwerken](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate16m1.png)

8. Als u begint met een virtuele matrix met softwareversie Update 1 (10.0.10296.0), kunt u nu 1.1 Update worden uitgevoerd en klaar bent. U kunt de resterende stappen overslaan. 

    Als u begint met een virtuele matrix met softwareversie Update 0.6 (10.0.10293.0), bent u nu bijgewerkt naar Update 1.0. Ziet u een ander bericht weergegeven dat aangeeft dat er updates beschikbaar zijn. Herhaal stap 4-7 te installeren Update 1.1.

    

