---
title: Access control records voor StorSimple Virtual Array beheren | Microsoft Docs
description: Beschrijft hoe u voor het beheren van access control records (ACR's) om te bepalen welke hosts kunnen verbinding maken met een volume op de StorSimple Virtual Array.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ce65aa4efba735305208f7a6d761bc2814d1b8f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38718883"
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>Gebruik StorSimple Device Manager voor het beheren van access control records voor StorSimple Virtual Array

## <a name="overview"></a>Overzicht

Access control records (ACR's) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op de StorSimple Virtual Array (ook wel bekend als de StorSimple on-premises virtuele apparaat). ACR's zijn ingesteld op een bepaald volume en de iSCSI-gekwalificeerde namen (IQN's de gebruikershandleiding) van de hosts bevatten. Wanneer een host probeert verbinding maken met een volume, controleert het apparaat de ACR die zijn gekoppeld aan het volume voor de IQN-naam, en als er een overeenkomst is, klikt u vervolgens de verbinding tot stand is gebracht. De **Access control records** blade in de **configuratie** sectie van uw Device Manager-service vindt u alle de access control records met de bijbehorende IQN's de gebruikershandleiding van de hosts.

![Access control records beheren](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Deze zelfstudie wordt uitgelegd dat de volgende algemene ACR-gerelateerde taken:

* Het IQN ophalen
* Een Acces controlerecord toevoegen
* Een access controlerecord bewerken
* Verwijderen van een access controlerecord

> [!IMPORTANT]
> 
> * Wanneer u een ACR toewijst aan een volume, Let erop dat het volume is niet gelijktijdig worden gebruikt door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.
> * Wanneer een ACR te verwijderen van een volume, zorg ervoor dat de bijbehorende host niet verkrijgen van toegang het volume tot is omdat de verwijdering tot een onderbreking van de lezen / schrijven leiden kan.


## <a name="get-the-iqn"></a>Het IQN ophalen

De volgende stappen uitvoeren om het IQN ophalen van een Windows-host waarop Windows Server 2012 wordt uitgevoerd.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Een ACR toevoegen

U gebruikt **Access control records** blade in de **configuratie** sectie van uw StorSimple Device Manager-service om toe te voegen ACR's. Normaal gesproken koppelt u een ACR aan één volume.

Voor informatie over hoe u een ACR met een volume koppelen, gaat u naar [toevoegen van een volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Wanneer u een ACR toewijst aan een volume, Let erop dat het volume is niet gelijktijdig worden gebruikt door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.


De volgende stappen uitvoeren om een ACR toevoegen.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.
2. In de **Access control records** blade, klikt u op **toevoegen**.
3. In de **ACR toevoegen** blade, doet u het volgende:
   
    1. Geef een **naam** voor uw ACR op.
    
    2. Onder **iSCSI-initiatornaam**, geeft u de IQN-naam van uw Windows-host. Als u wilt het IQN ophalen van uw Windows Server-host, het volgende doen:
   
    3. Start de Microsoft iSCSI-initiator op uw Windows-host. In het venster van de iSCSI-Initiator-eigenschappen, op de **configuratie** tabblad, selecteert en kopieert u de tekenreeks van de **initiatornaam** veld.
    Plak de volgende tekenreeks in de **IQN** veld in de **ACR toevoegen** blade.
   
    6. Klik op **toevoegen** om toe te voegen van de ACR.  
   
        ![Access control records toevoegen](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. De lijst in tabelvorm is bijgewerkt naar aanleiding van deze toevoeging.

## <a name="edit-an-acr"></a>Een ACR bewerken

U gebruikt de **Access control records** blade in de **configuratie** sectie van uw Device Manager-service in Azure portal ACR's bewerken.

> [!NOTE]
> U moet een ACR die momenteel in gebruik niet wijzigen. Als u wilt bewerken een ACR die zijn gekoppeld aan een volume dat momenteel wordt gebruikt, moet u het volume eerst offline uitvoeren.


Voer de volgende stappen uit als u wilt bewerken, een ACR.

#### <a name="to-edit-an-acr"></a>Een ACR bewerken

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie **Access control records**.
2. In de **Access control records** blade in de lijst in tabelvorm van de access control records, dubbelklikt u op de ACR die u wilt wijzigen.
3. In de **bewerken access control records** blade, doet u het volgende:
   
    1. Het IQN voor de ACR opgeven.
   
    2. Klik op **opslaan** aan de bovenkant van de blade om op te slaan van de gewijzigde ACR. U ziet het volgende bevestigingsbericht wordt weergegeven:
   
        ![Access control records bewerken](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Verwijderen van een access controlerecord

U gebruikt de **configuratie** pagina in de Azure portal ACR's verwijderen.

> [!NOTE]
> 
> * U moet een ACR die momenteel in gebruik niet verwijderen. Als u wilt verwijderen een ACR die zijn gekoppeld aan een volume dat momenteel wordt gebruikt, moet u het volume eerst offline uitvoeren.
> * Wanneer een ACR te verwijderen van een volume, zorg ervoor dat de bijbehorende host niet verkrijgen van toegang het volume tot is omdat de verwijdering tot een onderbreking van de lezen / schrijven leiden kan.


De volgende stappen uitvoeren om een Acces control record.

#### <a name="to-delete-an-access-control-record"></a>Een Acces controlerecord verwijderen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie **Access control records**.

2. In de **Access control records** blade in de lijst in tabelvorm van de access control records, dubbelklikt u op de ACR die u wilt verwijderen.

3. Klik op de blade bewerken access control records **verwijderen**.
   
    ![ACR's verwijderen](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **verwijderen** om door te gaan met het verwijderen. De lijst in tabelvorm is bijgewerkt naar aanleiding van de verwijdering.
   
   ![Waarschuwingsbericht](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [volumes toevoegen en configureren van ACR's](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

