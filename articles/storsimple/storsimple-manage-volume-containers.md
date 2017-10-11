---
title: Beheren van uw StorSimple-volumecontainers | Microsoft Docs
description: Legt uit hoe u kunt de pagina van de volume-containers voor de StorSimple Manager-service toevoegen, wijzigen of verwijderen van een volumecontainer.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>De StorSimple Manager-service gebruiken voor het beheren van StorSimple volumecontainers
## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe de StorSimple Manager-service maken en beheren van StorSimple volumecontainers gebruiken.

Een volumecontainer in een Microsoft Azure StorSimple-apparaat bevat een of meer volumes die storage-account, versleuteling en verbruik van bandbreedte-instellingen delen. Een apparaat kan meerdere volumecontainers voor alle volumes hebben. 

Een volumecontainer heeft de volgende kenmerken:

* **Volumes** – de lagen of lokaal vastgemaakt StorSimple-volumes die zijn opgenomen in de volumecontainer. Een volumecontainer mag maximaal 256 StorSimple-volumes bevatten.
* **Versleuteling** – een versleutelingssleutel die kan worden gedefinieerd voor elke volumecontainer. Deze sleutel wordt gebruikt voor het versleutelen van de gegevens die door uw StorSimple-apparaat wordt verzonden naar de cloud. Een defensie hoogwaardige AES-256-bitssleutel wordt gebruikt door de gebruiker ingevoerde sleutel. Uw gegevens wilt beveiligen, is het raadzaam dat u altijd versleuteling van cloudopslag inschakelen.
* **Storage-account** – het opslagaccount dat is gekoppeld aan uw serviceprovider voor cloud-opslag. Alle volumes die zich in een volumecontainer delen dit opslagaccount. U kunt een opslagaccount kiezen uit een bestaande lijst of maak een nieuw account als u de volumecontainer maken en geef vervolgens de referenties voor toegang voor dat account.
* **Cloud bandbreedte** – de bandbreedte die door het apparaat worden gebruikt wanneer de gegevens van het apparaat worden verzonden naar de cloud. U kunt een bandbreedteregeling afdwingen door een waarde tussen 1 en 1000 Mbps bij het definiëren van deze container. Als u het apparaat voor gebruik van alle beschikbare bandbreedte wilt, moet u dit veld ingesteld op onbeperkt. U kunt ook maken en toepassen van een bandbreedtesjabloon op basis van schema-bandbreedte toewijzen.

![Volume containers pagina](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Deze volgende procedures wordt uitgelegd hoe u de StorSimple **volumecontainers** pagina om de volgende algemene bewerkingen uitgevoerd:

* Een volumecontainer toevoegen 
* Een volumecontainer wijzigen 
* Een volumecontainer verwijderen 

## <a name="add-a-volume-container"></a>Een volumecontainer toevoegen
Voer de volgende stappen uit om een volumecontainer toevoegen.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Een volumecontainer wijzigen
Voer de volgende stappen uit voor het wijzigen van een volumecontainer.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Een volumecontainer verwijderen
Een volumecontainer heeft volumes in het. Worden kan verwijderd als de volumes die zijn opgenomen in het eerst worden verwijderd. Voer de volgende stappen uit voor het verwijderen van een volumecontainer.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple-volumes beheren](storsimple-manage-volumes.md). 
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

