---
title: Beheren van access control records in StorSimple | Microsoft Docs
description: Beschrijft hoe access control records (ACR's) gebruiken om te bepalen welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: 9173e34f889ce1c082b20bb382cb6ca9a03dd797
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597494"
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>De StorSimple Manager-service gebruiken voor het beheren van access control records

## <a name="overview"></a>Overzicht
Access control records (ACR's) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat. ACR's zijn ingesteld op een bepaald volume en de iSCSI-gekwalificeerde namen (IQN's de gebruikershandleiding) van de hosts bevatten. Wanneer een host probeert verbinding maken met een volume, wordt het apparaat controleert de ACR die zijn gekoppeld aan het volume voor de IQN-naam en als er een overeenkomst is, klikt u vervolgens de verbinding tot stand is gebracht. De access control records in de **configuratie** sectie van uw StorSimple Device Manager-serviceblade weergeven alle de access control records met de bijbehorende IQN's de gebruikershandleiding van de hosts.

Deze zelfstudie wordt uitgelegd dat de volgende algemene ACR-gerelateerde taken:

* Een Acces controlerecord toevoegen
* Een access controlerecord bewerken
* Verwijderen van een access controlerecord

> [!IMPORTANT]
> * Wanneer u een ACR toewijst aan een volume, Let erop dat het volume is niet gelijktijdig worden gebruikt door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.
> * Wanneer een ACR te verwijderen van een volume, zorg ervoor dat de bijbehorende host niet verkrijgen van toegang het volume tot is omdat de verwijdering tot een onderbreking van de lezen / schrijven leiden kan.

## <a name="get-the-iqn"></a>Het IQN ophalen

De volgende stappen uitvoeren om het IQN ophalen van een Windows-host waarop Windows Server 2012 wordt uitgevoerd.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Een Acces controlerecord toevoegen
U gebruikt de **configuratie** sectie in de serviceblade StorSimple Device Manager om toe te voegen ACR's. U wordt normaal gesproken een ACR koppelen met één volume.

De volgende stappen uitvoeren om een ACR toevoegen.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Ga naar uw StorSimple Device Manager-service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.
2. In de **Access control records** blade, klikt u op **+ ACR toevoegen**.

    ![Klik op ACR toevoegen](./media/storsimple-8000-manage-acrs/createacr1.png)

3. In de **ACR toevoegen** blade de volgende stappen uit:

    1. Geef een naam voor uw ACR.
    
    2. Geef de IQN-naam van uw Windows Server-host onder **iSCSI Initiator-Name (IQN)**.

    3. Klik op **toevoegen** de ACR maken.

        ![Klik op ACR toevoegen](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  De zojuist toegevoegde ACR wordt weergegeven in de lijst in tabelvorm met ACR's.

    ![Klik op ACR toevoegen](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Een access controlerecord bewerken
U gebruikt de **configuratie** sectie in de serviceblade StorSimple Device Manager ACR's bewerken.

> [!NOTE]
> Het is raadzaam dat u deze ACR's die zich momenteel niet in gebruik. Als u wilt bewerken een ACR die zijn gekoppeld aan een volume dat momenteel wordt gebruikt, moet u het volume eerst offline uitvoeren.

Voer de volgende stappen uit als u wilt bewerken, een ACR.

#### <a name="to-edit-an-access-control-record"></a>Een Acces controlerecord bewerken
1.  Ga naar uw StorSimple Device Manager-service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klik in de lijst in tabelvorm van de access control records, en selecteert u de ACR die u wilt wijzigen.

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr1.png)

3. In de **bewerken Acces controlerecord** blade, Geef een andere IQN overeenkomt met een andere host.

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr2.png)

4. Klik op **Opslaan**. Klik op **Ja** als u om bevestiging wordt gevraagd. 

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr3.png)

5. U krijgt een melding wanneer de ACR is bijgewerkt. De tabellaire aanbieding ook bijgewerkt om de wijziging weer te geven.

   
## <a name="delete-an-access-control-record"></a>Verwijderen van een access controlerecord
U gebruikt de **configuratie** sectie in de serviceblade StorSimple Device Manager ACR's verwijderen.

> [!NOTE]
> U kunt deze ACR's die zich momenteel niet in gebruik verwijderen. Als u wilt verwijderen een ACR die zijn gekoppeld aan een volume dat momenteel wordt gebruikt, moet u het volume eerst offline uitvoeren.

De volgende stappen uitvoeren om een Acces control record.

#### <a name="to-delete-an-access-control-record"></a>Een Acces controlerecord verwijderen
1.  Ga naar uw StorSimple Device Manager-service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klik in de lijst in tabelvorm van de access control records, en selecteert u de ACR die u wilt verwijderen.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Klik met de rechtermuisknop op het aanroepen van het contextmenu en selecteer **verwijderen**.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Wanneer u hierom wordt gevraagd om bevestiging, lees de informatie en klik vervolgens op **verwijderen**.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. U krijgt een melding wanneer de verwijdering is voltooid. De lijst in tabelvorm is bijgewerkt naar aanleiding van de verwijdering.

    ![Ga naar de access control records](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [beheer van StorSimple-volumes](storsimple-8000-manage-volumes-u2.md).
* Meer informatie over [met behulp van de StorSimple Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

