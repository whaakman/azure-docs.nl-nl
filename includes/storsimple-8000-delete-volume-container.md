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
ms.openlocfilehash: e683d17422321b780a1c01b3011292f2e2c631cb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156086"
---
Als u wilt verwijderen van een volumecontainer, moet u
 - Verwijder de volumes in de volumecontainer. Als de volumecontainer gekoppelde volumes heeft, zet de volumes offline eerst. Volg de stappen in [een volume offline halen](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Nadat de volumes offline zijn, kunt u ze verwijderen. 
 - gekoppelde back-upbeleidsregels verwijderen en cloud-momentopnamen. Controleer als de volumecontainer is gekoppeld back-upbeleid en cloudmomentopnamen. Als dit het geval is, klikt u vervolgens [verwijderen van de back-upbeleid](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Hiermee worden de cloudmomentopnamen ook verwijderd. 
 
Wanneer de volumecontainer geen gekoppelde volumes, back-upbeleid en cloudmomentopnamen heeft, kunt u deze kunt verwijderen. De volgende procedure als u wilt verwijderen van een volumecontainer uitvoeren.

#### <a name="to-delete-a-volume-container"></a>Verwijderen van een volumecontainer
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. Selecteer en klik op het apparaat en ga vervolgens naar **instellingen > beheren > volumecontainers**.

    ![Volume containers blade](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Selecteer in de lijst in tabelvorm van volumecontainers de volumecontainer die u wilt verwijderen, klik met de rechtermuisknop **...**  en selecteer vervolgens **verwijderen**.

    ![Volumecontainer](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Als een volumecontainer geen gekoppelde volumes, back-upbeleid en cloudmomentopnamen heeft, kan het worden verwijderd. Wanneer u hierom wordt gevraagd om bevestiging, controleren en schakel het selectievakje met vermelding van de impact van het verwijderen van de volumecontainer. Klik op **verwijderen** vervolgens verwijderen van de volumecontainer.

    ![Verwijdering bevestigen](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

De lijst met volumecontainers is bijgewerkt, zodat de container verwijderde volume.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


