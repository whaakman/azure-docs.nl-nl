---
title: Deactiveren en verwijderen van een Microsoft Azure StorSimple Virtual Array | Microsoft Docs
description: Beschrijft hoe u StorSimple-apparaat verwijdert uit de service door het eerst deactiveren en vervolgens te verwijderen.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: bb1a56d204a46f89213f20e317494120f0ea565e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60580535"
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Deactiveren en verwijderen van een StorSimple Virtual Array

## <a name="overview"></a>Overzicht

Wanneer u een StorSimple Virtual Array deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende StorSimple Device Manager-service. In deze zelfstudie wordt het volgende uitgelegd:

* Een apparaat deactiveren 
* Een gedeactiveerd apparaat verwijderen

De informatie in dit artikel geldt alleen voor virtuele StorSimple-matrices. Voor informatie over de 8000-serie, gaat u naar het [deactiveren of verwijderen van een apparaat](storsimple-deactivate-and-delete-device.md).

## <a name="when-to-deactivate"></a>Wanneer deactiveren?

Deactivering is een permanente bewerking en kan niet ongedaan worden gemaakt. U kunt een gedeactiveerd apparaat kan niet opnieuw registreren met de StorSimple Device Manager-service. U moet mogelijk deactiveren en verwijderen van een virtuele StorSimple-matrix in de volgende scenario's:

* **De geplande failover** : Uw apparaat online is en u van plan bent een failover van uw apparaat. Als u van plan bent om te upgraden naar een grotere apparaat, moet u mogelijk failover van uw apparaat. Nadat de Gegevenseigendom wordt overgedragen en de failover voltooid is, wordt het bronapparaat automatisch verwijderd.
* **Niet-geplande failover** : Uw apparaat offline is en u moet het apparaat een failover. In dit scenario kan zich voordoen tijdens een noodgeval wanneer er een storing in het datacenter en uw primair apparaat niet beschikbaar is. U van plan bent via het apparaat een tweede apparaat mislukt. Nadat de Gegevenseigendom wordt overgedragen en de failover voltooid is, wordt het bronapparaat automatisch verwijderd.
* **Uit bedrijf nemen** : Wilt u het apparaat buiten gebruik stellen. Hiervoor moet u het apparaat eerst deactiveren en deze te verwijderen. Wanneer u een apparaat deactiveert, u kunt geen toegang meer tot alle gegevens die lokaal wordt opgeslagen. U kunt alleen openen en herstellen van de gegevens die zijn opgeslagen in de cloud. Als u van plan bent het om apparaatgegevens te behouden na deactivering, moet u een cloud-momentopname van al uw gegevens uitvoeren voordat u een apparaat deactiveren. Deze cloudmomentopname kunt u alle gegevens op een later tijdstip te herstellen.

## <a name="deactivate-a-device"></a>Een apparaat deactiveren

U kunt uw apparaat, moet u de volgende stappen uitvoeren.

#### <a name="to-deactivate-the-device"></a>Het apparaat deactiveren

1. In uw service, gaat u naar **Management > apparaten**. In de **apparaten** blade, klikt u op en selecteer het apparaat dat u wilt uitschakelen.
   
    ![Selecteer het apparaat te deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. In uw **apparaatdashboard** blade, klikt u op **... Meer** en selecteer in de lijst **deactiveren**.
   
    ![Klikt u op deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. In de **deactiveren** blade, typ de naam van het apparaat en klik vervolgens op **deactiveren**. 
   
    ![Controleer of deactiveren](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Het deactiveren-proces wordt gestart en duurt een paar minuten om te voltooien.
   
    ![Deactiveren wordt uitgevoerd](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Na de deactivering, de lijst met apparaten wordt vernieuwd.
   
    ![Deactiveren voltooid](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Nu kunt u dit apparaat verwijderen.

## <a name="delete-the-device"></a>Het apparaat verwijderen

Een apparaat moet eerst worden gedeactiveerd om het te verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer vervolgens het verwijderde apparaat beheren. De gegevens die zijn gekoppeld aan het apparaat blijft echter in de cloud. Deze gegevens worden vervolgens berekend naast kosten in rekening gebracht.

Als u wilt verwijderen van het apparaat, moet u de volgende stappen uitvoeren.

#### <a name="to-delete-the-device"></a>Het apparaat verwijderen

1. In uw StorSimple Device Manager gaat u naar **Management > apparaten**. In de **apparaten** blade, selecteert u een gedeactiveerd apparaat die u wilt verwijderen.
2. In de **apparaatdashboard** blade, klikt u op **... Meer** en klik vervolgens op **verwijderen**.
   
   ![Selecteer het apparaat verwijderen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. In de **verwijderen** blade, typ de naam van uw apparaat om te bevestigen en klik vervolgens op **verwijderen**. Als u het apparaat verwijdert, worden de cloudgegevens die is gekoppeld aan het apparaat niet verwijderd. 
   
   ![De verwijdering bevestigen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. De verwijdering wordt gestart en duurt een paar minuten om te voltooien.
   
   ![Bezig met verwijderen](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Nadat het apparaat wordt verwijderd, kunt u de bijgewerkte lijst met apparaten weergeven.

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over hoe u failover wilt uitvoeren, gaat u naar [Failover en herstel na noodgevallen van uw StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).

* Voor meer informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md). 

