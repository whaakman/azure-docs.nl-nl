---
title: Deactiveren en verwijderen van een Microsoft Azure StorSimple virtuele matrix | Microsoft Docs
description: Beschrijving van het StorSimple-apparaat verwijderen uit de service met deze eerst te deactiveren en vervolgens te verwijderen.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 8dea36f92b034f8c6cdb6875634848d37f4c6606
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Deactiveren en verwijderen van een virtueel StorSimple-matrix

## <a name="overview"></a>Overzicht

Wanneer u een virtueel StorSimple-matrix deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende Apparaatbeheer StorSimple-service. Deze zelfstudie wordt uitgelegd hoe:

* Een apparaat deactiveren 
* Een gedeactiveerde apparaat verwijderen

De informatie in dit artikel is alleen van toepassing op virtuele StorSimple-matrices. Voor informatie over 8000-serie, gaat u naar het [deactiveren of verwijderen van een apparaat](storsimple-deactivate-and-delete-device.md).

## <a name="when-to-deactivate"></a>Wanneer voor het deactiveren?

Deactivering is een permanente bewerking en kan niet ongedaan worden gemaakt. U kunt een gedeactiveerde apparaat kan niet opnieuw registreren met de service Manager voor StorSimple-apparaat. Mogelijk moet u deactiveren en verwijderen van een virtueel StorSimple-matrix in de volgende scenario's:

* **De geplande failover** : het apparaat online is en u van plan bent uw apparaat failover. Als u van plan bent om te werken naar een grotere apparaat, kunt u wellicht een failover van uw apparaat. Nadat de Gegevenseigendom wordt overgedragen en de failover voltooid is, wordt het bronapparaat automatisch verwijderd.
* **Niet-geplande failover** : het apparaat offline is en u moet het apparaat een failover. Dit scenario kan optreden tijdens een noodgeval wanneer er een storing in het datacenter en het primaire apparaat niet beschikbaar is. U van plan bent het apparaat een tweede apparaat failover. Nadat de Gegevenseigendom wordt overgedragen en de failover voltooid is, wordt het bronapparaat automatisch verwijderd.
* **Buiten gebruik stellen** : U wilt dat het apparaat buiten gebruik stellen. Hiervoor moet u eerst het apparaat te deactiveren en vervolgens te verwijderen. Wanneer u een apparaat deactiveert, u niet langer toegang tot alle gegevens die lokaal wordt opgeslagen. U kunt alleen toegang tot en het herstellen van de gegevens die zijn opgeslagen in de cloud. Als u van plan bent de gegevens van het apparaat om na te behouden deactivering, moet u een cloudmomentopname van al uw gegevens nemen voordat u een apparaat deactiveren. Deze cloudmomentopname met de kunt u alle gegevens in een later stadium te herstellen.

## <a name="deactivate-a-device"></a>Een apparaat deactiveren

U kunt uw apparaat, moet u de volgende stappen uitvoeren.

#### <a name="to-deactivate-the-device"></a>Het apparaat deactiveren

1. In uw service, gaat u naar **Management > apparaten**. In de **apparaten** blade, klikt u op en selecteer het apparaat dat u wilt deactiveren.
   
    ![Selecteer het apparaat te deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. In uw **apparaat dashboard** blade, klikt u op **... Meer** en selecteer in de lijst **deactiveren**.
   
    ![Klikt u op deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. In de **deactiveren** blade, typ de naam van het apparaat en klik vervolgens op **deactiveren**. 
   
    ![Bevestig deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Het proces deactiveren wordt gestart en duurt een paar minuten.
   
    ![Deactiveren wordt uitgevoerd](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Na de deactivering, de lijst met apparaten wordt vernieuwd.
   
    ![Volledige deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Nu kunt u dit apparaat verwijderen.

## <a name="delete-the-device"></a>Het apparaat verwijderen

Een apparaat moet eerst worden gedeactiveerd om het te verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet langer dan de verwijderde apparaatinschrijvingsbeheerder beheren. De gegevens die zijn gekoppeld aan het apparaat blijft echter in de cloud. Deze gegevens worden vervolgens kosten toenemen.

Als u wilt verwijderen van het apparaat, moet u de volgende stappen uitvoeren.

#### <a name="to-delete-the-device"></a>Het apparaat verwijderen

1. In uw StorSimple-Apparaatbeheer gaat u naar **Management > apparaten**. In de **apparaten** blade, selecteer een gedeactiveerde apparaat dat u wilt verwijderen.
2. In de **apparaat dashboard** blade, klikt u op **... Meer** en klik vervolgens op **verwijderen**.
   
   ![Selecteer het apparaat verwijderen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. In de **verwijderen** blade, typ de naam van uw apparaat om te bevestigen en klik vervolgens op **verwijderen**. Als u het apparaat verwijdert, verwijdert niet de cloudgegevens die zijn gekoppeld aan het apparaat. 
   
   ![De verwijdering bevestigen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. De verwijdering wordt gestart en duurt een paar minuten.
   
   ![Bezig met verwijderen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Nadat het apparaat wordt verwijderd, kunt u de bijgewerkte lijst met apparaten weergeven.

## <a name="next-steps"></a>Volgende stappen

* Ga voor meer informatie over de failover naar [Failover en herstel na noodgevallen van uw virtuele StorSimple-matrix](storsimple-virtual-array-failover-dr.md).

* Voor meer informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw virtuele StorSimple-matrix](storsimple-virtual-array-manager-service-administration.md). 

