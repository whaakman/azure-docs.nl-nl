---
title: bestand opnemen
description: bestand opnemen
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 06/08/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: e7f3f80c886f90a8bc3ae8c38e7d101c506439a6
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35250219"
---
Als u wilt een volumecontainer hebt verwijderd, moet u
 - Verwijder de volumes in de volumecontainer. Als de volumecontainer is volumes gekoppeld, deze volumes offline halen eerst. Volg de stappen in [offline zetten van een volume](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Nadat de volumes offline zijn, kunt u ze verwijderen. 
 - gekoppelde back-upbeleid te verwijderen en de cloud worden opgeslagen. Controleer of de volumecontainer is gekoppeld, back-upbeleid en cloudmomentopnamen. Als dit het geval is, klikt u vervolgens [verwijderen van het back-upbeleid](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Hiermee verwijdert u ook de cloudmomentopnamen. 
 
Wanneer de volumecontainer geen gekoppelde volumes, back-upbeleid en cloudmomentopnamen heeft, kunt u het kunt verwijderen. De volgende procedure voor het verwijderen van een volumecontainer uitvoeren.

#### <a name="to-delete-a-volume-container"></a>Een volumecontainer verwijderen
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. Selecteer en klik op het apparaat gaat u naar **instellingen > beheren > volumecontainers**.

    ![Volume containers blade](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Selecteer in de lijst in tabelvorm van volumecontainers de volumecontainer die u wilt verwijderen, klik met de rechtermuisknop **...**  en selecteer vervolgens **verwijderen**.

    ![Volumecontainer verwijderen](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Als een volumecontainer geen gekoppelde volumes, back-upbeleid en cloudmomentopnamen heeft, kan het worden verwijderd. Wanneer u wordt gevraagd om bevestiging, bekijken en schakel het selectievakje met vermelding van de gevolgen van het verwijderen van de volumecontainer. Klik op **verwijderen** vervolgens de volumecontainer te verwijderen.

    ![Verwijdering bevestigen](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

De lijst met volumecontainers is bijgewerkt met de container verwijderde volume.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


