---
title: Access control records beheren voor virtuele StorSimple-matrix | Microsoft Docs
description: Beschrijft hoe access control records (ACRs) om te bepalen welke hosts kunnen verbinding maken met een volume op de virtuele StorSimple-matrix te beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
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
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>Het StorSimple-Apparaatbeheer gebruiken voor het beheren van access control records voor virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

Access control-records (ACRs) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op de virtuele StorSimple-matrix (ook wel bekend als de StorSimple on-premises virtuele apparaat). ACRs zijn ingesteld op een bepaald volume en de iSCSI-gekwalificeerde namen bevatten (IQN's de gebruikershandleiding) van de hosts. Wanneer een host probeert verbinding maken met een volume, het apparaat de ACR die zijn gekoppeld aan dat volume voor de naam IQN controleert en als er een overeenkomst, klikt u vervolgens de verbinding tot stand is gebracht. De **Access control records** blade binnen de **configuratie** sectie van uw service Apparaatbeheer vindt u alle access control records met de bijbehorende IQN's de gebruikershandleiding van de hosts.

![Access control records beheren](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Deze zelfstudie wordt uitgelegd hoe de volgende algemene ACR-gerelateerde taken:

* Het IQN ophalen
* Een record voor toegangscontrole toevoegen
* Een record voor toegangscontrole bewerken
* Een record voor toegangscontrole verwijderen

> [!IMPORTANT]
> 
> * Wanneer u een ACR toewijst aan een volume, ervoor te zorgen dat het volume niet gelijktijdig toegankelijk is door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.
> * Wanneer u een ACR verwijdert van een volume, zorg dat de bijbehorende host geen toegang heeft tot het volume omdat de verwijdering tot een onderbreking van de alleen-lezen leiden kan.


## <a name="get-the-iqn"></a>Het IQN ophalen

De volgende stappen uitvoeren om te het IQN ophalen van een Windows-host waarop Windows Server 2012 wordt uitgevoerd.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Een ACR toevoegen

U gebruikt **Access control records** blade binnen de **configuratie** gedeelte van uw StorSimple-apparaat Manager service ACRs toevoegen. Normaal gesproken koppelt u een ACR aan één volume.

Ga voor informatie over het koppelen van een ACR met een volume naar [toevoegen van een volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Wanneer u een ACR toewijst aan een volume, ervoor te zorgen dat het volume niet gelijktijdig toegankelijk is door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd.


Voer de volgende stappen uit om toe te voegen een ACR.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie, klikt u op **Access control records**.
2. In de **Access control records** blade, klikt u op **toevoegen**.
3. In de **ACR toevoegen** blade het volgende doen:
   
    1. Geef een **naam** voor uw ACR op.
    
    2. Onder **iSCSI-initiatornaam**, geef de naam van het IQN van uw Windows-host. Als u het IQN van uw Windows Server-host, het volgende doen:
   
    3. Start de Microsoft iSCSI-initiator op uw Windows-host. In het venster van de iSCSI-Initiator-eigenschappen, op de **configuratie** tabblad Selecteer en kopieer de tekenreeks van de **initiatornaam** veld.
    Plak deze tekenreeks in de **IQN** veld in de **ACR toevoegen** blade.
   
    6. Klik op **toevoegen** de ACR toevoegen.  
   
        ![Access control records toevoegen](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. In de lijst in tabelvorm is bijgewerkt, zodat deze toevoeging.

## <a name="edit-an-acr"></a>Een ACR bewerken

U gebruikt de **Access control records** blade binnen de **configuratie** gedeelte van uw service Apparaatbeheer in de Azure portal ACRs bewerken.

> [!NOTE]
> U moet een ACR die momenteel in gebruik is niet wijzigen. Als u wilt bewerken een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.


De volgende stappen uitvoeren om te bewerken van een ACR.

#### <a name="to-edit-an-acr"></a>Een ACR bewerken

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie **Access control records**.
2. In de **Access control records** blade in de lijst in tabelvorm van de access control records, dubbelklikt u op de ACR die u wilt wijzigen.
3. In de **bewerken access control records** blade het volgende doen:
   
    1. Het IQN opgeven voor de ACR.
   
    2. Klik op **opslaan** boven aan de blade om op te slaan van het gewijzigde ACR. U ziet het volgende bevestigingsbericht weergegeven:
   
        ![Access control records bewerken](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen

U gebruikt de **configuratie** pagina in de Azure portal ACRs verwijderen.

> [!NOTE]
> 
> * U moet een ACR die momenteel in gebruik is niet verwijderen. Als u wilt verwijderen een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.
> * Wanneer u een ACR verwijdert van een volume, zorg dat de bijbehorende host geen toegang heeft tot het volume omdat de verwijdering tot een onderbreking van de alleen-lezen leiden kan.


Voer de volgende stappen uit als u wilt verwijderen van een record voor toegangscontrole.

#### <a name="to-delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie **Access control records**.

2. In de **Access control records** blade in de lijst in tabelvorm van de access control records, dubbelklikt u op de ACR die u wilt verwijderen.

3. Klik op de blade bewerken access control records **verwijderen**.
   
    ![ACRS verwijderen](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Wanneer u om bevestiging gevraagd, klikt u op **verwijderen** om door te gaan met het verwijderen. In de lijst in tabelvorm is bijgewerkt met de verwijdering.
   
   ![Waarschuwingsbericht](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [volumes toevoegen en configureren van ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

