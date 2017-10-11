---
title: Access control records in StorSimple beheren | Microsoft Docs
description: Beschrijft hoe access control-records (ACRs) gebruiken om te bepalen welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a87624b5706c1d9b8c2b9926e5580996a89ce984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>De StorSimple Manager-service gebruiken om de access control records beheren
## <a name="overview"></a>Overzicht
Access control-records (ACRs) kunnen u opgeven welke hosts kunnen verbinding maken met een volume op het StorSimple-apparaat. ACRs zijn ingesteld op een bepaald volume en de iSCSI-gekwalificeerde namen bevatten (IQN's de gebruikershandleiding) van de hosts. Wanneer een host probeert verbinding maken met een volume bevindt, controleert het apparaat de ACR die zijn gekoppeld aan dat volume voor de naam IQN en als er een overeenkomst, klikt u vervolgens de verbinding tot stand is gebracht. Het toegangsbeheer sectie registreert op de **configureren** pagina wordt weergegeven voor alle access control records met de bijbehorende IQN's de gebruikershandleiding van de hosts.

Deze zelfstudie wordt uitgelegd hoe de volgende algemene ACR-gerelateerde taken:

* Een record voor toegangscontrole toevoegen 
* Een record voor toegangscontrole bewerken 
* Een record voor toegangscontrole verwijderen 

> [!IMPORTANT]
> * Wanneer u een ACR toewijst aan een volume, ervoor te zorgen dat het volume niet gelijktijdig toegankelijk is door meer dan één niet-geclusterde host omdat dit kan het volume beschadigd. 
> * Wanneer u een ACR verwijdert van een volume, zorg dat de bijbehorende host geen toegang heeft tot het volume omdat de verwijdering tot een onderbreking van de alleen-lezen leiden kan.
> 
> 

## <a name="add-an-access-control-record"></a>Een record voor toegangscontrole toevoegen
Gebruik van de StorSimple Manager-service **configureren** pagina ACRs toevoegen. U koppelt doorgaans één ACR met één volume.

Voer de volgende stappen uit om toe te voegen een ACR.

#### <a name="to-add-an-access-control-record"></a>Een record voor toegangscontrole toevoegen
1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik op de **configureren** tabblad.
2. In de lijst in tabelvorm onder **Access control records**, supply een **naam** voor uw ACR.
3. Geef de naam van het IQN van uw Windows-host onder **iSCSI-initiatornaam**. Als u het IQN van uw Windows Server-host, het volgende doen:
   
   * Start de Microsoft iSCSI-initiator op uw Windows-host.
   * Selecteer en kopieer in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Configuratie** de tekenreeks in het veld **Naam van initiator**.
   * Plak deze tekenreeks in de **iSCSI-initiatornaam** op de tabel ACRs in de klassieke Azure portal.
4. Klik op **opslaan** de zojuist gemaakte ACR opslaan. In de lijst in tabelvorm worden bijgewerkt naar aanleiding van deze toevoeging.

## <a name="edit-an-access-control-record"></a>Een record voor toegangscontrole bewerken
U gebruikt de **configureren** pagina in de klassieke Azure portal ACRs bewerken. 

> [!NOTE]
> U kunt alleen deze ACRs die zich momenteel niet in gebruik wijzigen. Als u wilt bewerken een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.
> 
> 

De volgende stappen uitvoeren om te bewerken van een ACR.

#### <a name="to-edit-an-access-control-record"></a>Een record voor toegangscontrole bewerken
1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik op de **configureren** tabblad.
2. In de lijst in tabelvorm van de access control-records de muisaanwijzer op de ACR die u wilt wijzigen.
3. Geef een nieuwe naam en/of IQN voor de ACR.
4. Klik op **opslaan** de gewijzigde ACR opslaan. In de lijst in tabelvorm worden bijgewerkt om deze wijziging weer te geven.

## <a name="delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen
U gebruikt de **configureren** pagina in de klassieke Azure portal ACRs verwijderen. 

> [!NOTE]
> U kunt alleen deze ACRs die zich momenteel niet in gebruik verwijderen. Als u wilt verwijderen een ACR die zijn gekoppeld aan een volume dat wordt gebruikt, moet u het volume eerst offline halen.
> 
> 

Voer de volgende stappen uit als u wilt verwijderen van een record voor toegangscontrole.

#### <a name="to-delete-an-access-control-record"></a>Een record voor toegangscontrole verwijderen
1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik op de **configureren** tabblad.
2. In de lijst in tabelvorm access control records (ACRs), de muisaanwijzer op de ACR die u wilt verwijderen.
3. Een verwijderpictogram (**x**) wordt weergegeven in de rechterkolom extreme voor de ACR die u selecteert. Klik op de **x** pictogram de ACR verwijderen.
4. Wanneer u om bevestiging gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. In de lijst in tabelvorm worden bijgewerkt naar aanleiding van de verwijdering.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple-volumes beheren](storsimple-manage-volumes.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

