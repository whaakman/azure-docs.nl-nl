---
title: Deactiveren en verwijderen van een StorSimple 8000 series apparaat | Microsoft Docs
description: Beschrijving van het StorSimple-apparaat verwijderen uit de service met deze eerst te deactiveren en vervolgens te verwijderen.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 3c00867a29cf8343a57e74e2aabe3971ae6837af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Deactiveren en verwijderen van een StorSimple-apparaat

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe deactiveren en verwijderen van een virtueel StorSimple-apparaat is verbonden met een StorSimple-apparaat Manager-service. De instructies in dit artikel geldt alleen voor apparaten van StorSimple 8000 serie met inbegrip van de StorSimple-Cloud-apparaten. Als u van een virtueel StorSimple-matrix gebruikmaakt, ga dan naar [deactiveren en verwijderen van een virtueel StorSimple-matrix](storsimple-virtual-array-deactivate-and-delete-device.md).

Deactivering verbreekt de verbinding tussen het apparaat en de bijbehorende Apparaatbeheer StorSimple-service. U kunt desgewenst een StorSimple-apparaat buiten dienst duren (bijvoorbeeld, als u uw apparaat upgraden of vervangen of als u niet langer StorSimple gebruikt). Als dit het geval is, moet u het apparaat deactiveren voordat u het kunt verwijderen.

Wanneer u een apparaat deactiveert, is alle gegevens die lokaal zijn opgeslagen op het apparaat niet meer toegankelijk. Alleen de gegevens die zijn gekoppeld aan het apparaat dat is opgeslagen in de cloud kunnen worden hersteld.

> [!WARNING]
> Deactivering is een permanente bewerking en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd met de service Manager voor StorSimple-apparaat, tenzij deze is opnieuw ingesteld op de standaardwaarden.
>
> De fabrieksinstellingen van proces verwijdert alle gegevens die lokaal zijn opgeslagen op uw apparaat. Daarom moet u een cloudmomentopname van al uw gegevens nemen voordat u een apparaat deactiveren. Deze cloudmomentopname met de kunt u alle gegevens in een later stadium te herstellen.

Na het lezen van deze zelfstudie, kunt u zich kunt:

* Een apparaat deactiveren en verwijderen van de gegevens.
* Een apparaat deactiveren en de gegevens behouden.

> [!NOTE]
> Voordat u een fysieke StorSimple-apparaat of de cloud apparaat deactiveert, stoppen of verwijderen van clients en hosts die afhankelijk van dat apparaat zijn.


## <a name="deactivate-and-delete-data"></a>Deactiveren en verwijderen van gegevens

Als u geïnteresseerd bent in het apparaat volledig verwijderen en niet wilt bewaren van gegevens op het apparaat, klikt u vervolgens de volgende stappen uitvoeren.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Het apparaat deactiveren en verwijderen van de gegevens

1. Voordat u een apparaat uitschakelt, moet u het volume verwijderen containers (en de volumes) die zijn gekoppeld aan het apparaat. Nadat u de gekoppelde back-ups hebt verwijderd, kunt u volumecontainers verwijderen.

    > [!NOTE]
    > Voordat u een fysieke StorSimple-apparaat of de cloud apparaat deactiveert, zorg ervoor dat de gegevens van de container verwijderde volume daadwerkelijk wordt verwijderd van het apparaat. U kunt de cloud verbruik grafieken bewaken en wanneer er gebruik van de cloud verwijderen vanwege de back-ups die u hebt verwijderd, klikt u vervolgens kunt u verdergaan met het apparaat deactiveren. Als u het apparaat deactiveren voordat deze vervolgkeuzelijst optreedt, worden de gegevens in het opslagaccount is overkomt en kosten toenemen.

2. Als volgt te werk om het apparaat te deactiveren:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het apparaat dat u uitschakelen wilt, klik met de rechtermuisknop en klik vervolgens op **deactiveren**.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. In de **deactiveren** blade, typ de apparaatnaam om te bevestigen en klik vervolgens op **deactiveren**. Het proces deactiveren wordt gestart en duurt een paar minuten.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Na de deactivering, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet langer dan de verwijderde apparaatinschrijvingsbeheerder beheren. Gebruik de volgende stappen uit om te verwijderen van het apparaat:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het gedeactiveerde apparaat dat u verwijderen wilt, klik met de rechtermuisknop en klik vervolgens op **verwijderen**.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. In de **verwijderen** blade, typ de apparaatnaam om te bevestigen en klik vervolgens op **verwijderen**. De verwijdering duurt enkele minuten.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Nadat de verwijdering met succes voltooid is, wordt u gewaarschuwd. De lijst met apparaten kan ook worden bijgewerkt naar aanleiding van de verwijdering.

## <a name="deactivate-and-retain-data"></a>Deactiveren en behoud van gegevens

Als u bent geïnteresseerd in het verwijderen van het apparaat, maar u wilt dat de gegevens wilt behouden, voltooit u de volgende stappen:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Een apparaat deactiveren en de gegevens behouden
1. Het apparaat deactiveren. Blijven de volumecontainers en de momentopnamen van het apparaat.
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het apparaat dat u uitschakelen wilt, klik met de rechtermuisknop en klik vervolgens op **deactiveren**.

         ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. In de **deactiveren** blade, typ de apparaatnaam om te bevestigen en klik vervolgens op **deactiveren**. Het proces deactiveren wordt gestart en duurt een paar minuten.

         ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. U kunt nu de volumecontainers en de bijbehorende momentopnamen failover. Voor procedures gaat u naar [Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-8000-device-failover-disaster-recovery.md).
3. Na deactiveren en failover, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet langer dan de verwijderde apparaatinschrijvingsbeheerder beheren. Als u wilt verwijderen van het apparaat, moet u de volgende stappen uitvoeren:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het gedeactiveerde apparaat dat u verwijderen wilt, klik met de rechtermuisknop en klik vervolgens op **verwijderen**.

       ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. In de **verwijderen** blade, typ de apparaatnaam om te bevestigen en klik vervolgens op **verwijderen**. De verwijdering duurt enkele minuten.

       ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Nadat de verwijdering met succes voltooid is, wordt u gewaarschuwd. De lijst met apparaten kan ook worden bijgewerkt naar aanleiding van de verwijdering.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Deactiveren en verwijderen van een cloud-apparaat

Voor een StorSimple Cloud-apparaat deactiveren vanuit de portal deallocates en verwijdert u de virtuele machine en de resources die zijn gemaakt tijdens het inrichten. Wanneer het cloudapparaat is gedeactiveerd, kan het niet meer worden hersteld naar de oorspronkelijke staat.

![Cloud van de StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Deactivering van resultaten in de volgende acties:

* Het StorSimple-Cloud-apparaat wordt verwijderd uit de service.
* De virtuele machine voor het StorSimple-Cloud-apparaat wordt verwijderd.
* De besturingssysteemschijf en gegevensschijven die zijn gemaakt voor het toestel StorSimple-Cloud worden verwijderd.
* De gehoste service en virtuele netwerken die zijn gemaakt tijdens het inrichten, blijven behouden. Als u deze entiteiten niet gebruikt, moet u deze handmatig verwijderen.
* Cloudmomentopnamen die zijn gemaakt door het toestel StorSimple-Cloud worden bewaard.

Nadat het toestel cloud is gedeactiveerd, kunt u deze verwijderen uit de lijst met apparaten. Selecteer de gedeactiveerde apparaat, klik met de rechtermuisknop en klik vervolgens op **verwijderen**. StorSimple waarschuwt u wanneer het apparaat is verwijderd en de lijst met apparaten updates.

## <a name="next-steps"></a>Volgende stappen

* Als u wilt herstellen het gedeactiveerde apparaat naar de standaardwaarden, gaat u naar [opnieuw instellen van het apparaat fabrieksinstellingen](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Voor technische ondersteuning [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

