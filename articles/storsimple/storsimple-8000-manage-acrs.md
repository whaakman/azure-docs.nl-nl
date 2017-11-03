---
title: Access control records in StorSimple beheren | Microsoft Docs
description: Beschrijft hoe access control-records (ACRs) gebruiken om te bepalen welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat.
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
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: 9173e34f889ce1c082b20bb382cb6ca9a03dd797
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>De StorSimple Manager-service gebruiken om de access control records beheren

## <a name="overview"></a>Overzicht
Access control-records (ACRs) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat. ACRs zijn ingesteld op een bepaald volume en de iSCSI-gekwalificeerde namen bevatten (IQN's de gebruikershandleiding) van de hosts. Wanneer een host probeert verbinding maken met een volume bevindt, controleert het apparaat de ACR die zijn gekoppeld aan dat volume voor de naam IQN en als er een overeenkomst, klikt u vervolgens de verbinding tot stand is gebracht. Het toegangsbeheer registreert in de **configuratie** sectie van de blade van uw StorSimple Device Manager-service alle access control records met de bijbehorende IQN's de gebruikershandleiding van de hosts weergeven.

Deze zelfstudie wordt uitgelegd hoe de volgende algemene ACR-gerelateerde taken:

* Een record voor toegangscontrole toevoegen
* Een record voor toegangscontrole bewerken
* Een record voor toegangscontrole verwijderen

> [!IMPORTANT]
> * Wanneer u een ACR toewijst aan een volume, ervoor te zorgen dat het volume niet gelijktijdig toegankelijk is door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.
> * Wanneer u een ACR verwijdert van een volume, zorg dat de bijbehorende host geen toegang heeft tot het volume omdat de verwijdering tot een onderbreking van de alleen-lezen leiden kan.

## <a name="get-the-iqn"></a>Het IQN ophalen

De volgende stappen uitvoeren om te het IQN ophalen van een Windows-host waarop Windows Server 2012 wordt uitgevoerd.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Een record voor toegangscontrole toevoegen
U gebruikt de **configuratie** sectie in de blade van de service Manager voor StorSimple-apparaat ACRs toevoegen. U koppelt doorgaans één ACR met één volume.

Voer de volgende stappen uit om toe te voegen een ACR.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Ga naar uw StorSimple-apparaat Manager-service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.
2. In de **Access control records** blade, klikt u op **+ toevoegen ACR**.

    ![Klik op toevoegen ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. In de **ACR toevoegen** blade, moet u de volgende stappen uitvoeren:

    1. Geef een naam voor uw ACR.
    
    2. Geef de naam van het IQN van uw Windows Server-host onder **iSCSI-Initiator Name (IQN)**.

    3. Klik op **toevoegen** de ACR maken.

        ![Klik op toevoegen ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  De toegevoegde ACR wordt weergegeven in de lijst in tabelvorm van ACRs.

    ![Klik op toevoegen ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Een record voor toegangscontrole bewerken
U gebruikt de **configuratie** sectie in de blade van de service Manager voor StorSimple-apparaat ACRs bewerken.

> [!NOTE]
> Het is raadzaam dat u alleen deze ACRs die zich momenteel niet in gebruik. Als u wilt bewerken een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.

De volgende stappen uitvoeren om te bewerken van een ACR.

#### <a name="to-edit-an-access-control-record"></a>Een record voor toegangscontrole bewerken
1.  Ga naar uw StorSimple-apparaat Manager-service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klik in de lijst in tabelvorm access control records en selecteert u de ACR die u wilt wijzigen.

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr1.png)

3. In de **record voor toegangscontrole bewerken** blade, Geef een andere IQN overeenkomt met een andere host.

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr2.png)

4. Klik op **Opslaan**. Klik op **Ja** als u om bevestiging wordt gevraagd. 

    ![Access control records bewerken](./media/storsimple-8000-manage-acrs/editacr3.png)

5. U wordt gewaarschuwd wanneer de ACR wordt bijgewerkt. De in tabelvorm aanbieding ook bijgewerkt zodat de wijzigingen.

   
## <a name="delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen
U gebruikt de **configuratie** sectie in de blade van de service Manager voor StorSimple-apparaat ACRs verwijderen.

> [!NOTE]
> U kunt alleen deze ACRs die zich momenteel niet in gebruik verwijderen. Als u wilt verwijderen een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.

Voer de volgende stappen uit als u wilt verwijderen van een record voor toegangscontrole.

#### <a name="to-delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen
1.  Ga naar uw StorSimple-apparaat Manager-service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klik in de lijst in tabelvorm access control records en selecteert u de ACR die u wilt verwijderen.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Klik met de rechtermuisknop het contextmenu aanroepen en selecteer **verwijderen**.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Als u wordt gevraagd om bevestiging, lees de informatie en klik vervolgens op **verwijderen**.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. U wordt gewaarschuwd wanneer de verwijdering is voltooid. In de lijst in tabelvorm is bijgewerkt met de verwijdering.

    ![Ga naar de access control-records](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple-volumes beheren](storsimple-8000-manage-volumes-u2.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

