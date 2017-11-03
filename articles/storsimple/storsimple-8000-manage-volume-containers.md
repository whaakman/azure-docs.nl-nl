---
title: Beheren van uw StorSimple-volumecontainers op het apparaat van de serie StorSimple 8000 | Microsoft Docs
description: Legt uit hoe u kunt de StorSimple-Apparaatbeheer volume containers pagina toevoegen, wijzigen of verwijderen van een volumecontainer.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 0f8e00d6d07224f56625482f339e612e68914be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van StorSimple volumecontainers

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe u met de service Manager voor StorSimple-apparaat maken en beheren van StorSimple volumecontainers.

Een volumecontainer in een Microsoft Azure StorSimple-apparaat bevat een of meer volumes die storage-account, versleuteling en verbruik van bandbreedte-instellingen delen. Een apparaat kan meerdere volumecontainers voor alle volumes hebben. 

Een volumecontainer heeft de volgende kenmerken:

* **Volumes** – de lagen of lokaal vastgemaakt StorSimple-volumes die zijn opgenomen in de volumecontainer. 
* **Versleuteling** – een versleutelingssleutel die kan worden gedefinieerd voor elke volumecontainer. Deze sleutel wordt gebruikt voor het versleutelen van de gegevens die door uw StorSimple-apparaat wordt verzonden naar de cloud. Een defensie hoogwaardige AES-256-bitssleutel wordt gebruikt door de gebruiker ingevoerde sleutel. Uw gegevens wilt beveiligen, is het raadzaam dat u altijd versleuteling van cloudopslag inschakelen.
* **Storage-account** – de Azure storage-account dat wordt gebruikt voor het opslaan van de gegevens. Alle volumes die zich in een volumecontainer delen dit opslagaccount. U kunt een opslagaccount kiezen uit een bestaande lijst of maak een nieuw account als u de volumecontainer maken en geef vervolgens de referenties voor toegang voor dat account.
* **Cloud bandbreedte** – de bandbreedte die door het apparaat worden gebruikt wanneer de gegevens van het apparaat worden verzonden naar de cloud. U kunt een bandbreedteregeling afdwingen door een waarde tussen 1 en 1000 Mbps op te geven wanneer u deze container maken. Als u wilt dat het apparaat aan alle beschikbare bandbreedte in beslag nemen, stelt u dit veld op **onbeperkt**. U kunt ook maken en toepassen van een bandbreedtesjabloon op basis van schema-bandbreedte toewijzen.

De volgende procedures wordt uitgelegd hoe u de StorSimple **volumecontainers** blade om de volgende algemene bewerkingen te voltooien:

* Een volumecontainer toevoegen
* Een volumecontainer wijzigen
* Een volumecontainer verwijderen

## <a name="add-a-volume-container"></a>Een volumecontainer toevoegen
Voer de volgende stappen uit om een volumecontainer toevoegen.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Een volumecontainer wijzigen
Voer de volgende stappen uit voor het wijzigen van een volumecontainer.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Een volumecontainer verwijderen
Een volumecontainer heeft volumes in het. Worden kan verwijderd als de volumes die zijn opgenomen in het eerst worden verwijderd. Voer de volgende stappen uit voor het verwijderen van een volumecontainer.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple-volumes beheren](storsimple-8000-manage-volumes-u2.md). 
* Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

