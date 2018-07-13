---
title: Beheren van uw StorSimple-volumecontainers op de StorSimple 8000-apparaat | Microsoft Docs
description: Wordt uitgelegd hoe u kunt de pagina met StorSimple Device Manager service volume containers toevoegen, wijzigen of verwijderen van een volumecontainer.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 0f8e00d6d07224f56625482f339e612e68914be2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38606586"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a>Gebruik de service StorSimple Device Manager voor het beheer van StorSimple volumecontainers

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe u van de service StorSimple Device Manager maken en beheren van containers voor StorSimple-volume.

Een volumecontainer in een Microsoft Azure StorSimple-apparaat bevat een of meer volumes die delen van storage-account, versleuteling en instellingen voor het verbruik van bandbreedte. Een apparaat kan meerdere volumecontainers voor alle volumes hebben. 

Een volumecontainer heeft de volgende kenmerken:

* **Volumes** : de lagen of lokaal vastgemaakt StorSimple-volumes die zijn opgenomen in de volumecontainer. 
* **Versleuteling** – een versleutelingssleutel die kan worden gedefinieerd voor elke volumecontainer. Deze sleutel wordt gebruikt voor het versleutelen van de gegevens die door uw StorSimple-apparaat wordt verzonden naar de cloud. Een zijn AES-256-bitssleutel wordt gebruikt met de sleutel gebruiker heeft ingevoerd. Als u wilt uw gegevens worden beveiligd, wordt u aangeraden dat u altijd versleuteling van cloudopslag inschakelen.
* **Storage-account** : het Azure storage-account dat wordt gebruikt voor het opslaan van de gegevens. Alle volumes die zich bevinden in een volumecontainer delen dit storage-account. U kunt een storage-account kiezen uit een bestaande lijst of maak een nieuw account wanneer u de volumecontainer maken en geef vervolgens de referenties voor toegang voor dat account.
* **Cloud-bandbreedte** : de bandbreedte die door het apparaat worden gebruikt wanneer de gegevens van het apparaat wordt verzonden naar de cloud. U kunt een besturingselement voor de bandbreedte afdwingen door een waarde tussen 1 en 1000 Mbps op te geven wanneer u deze container maakt. Als u wilt dat het apparaat alle beschikbare bandbreedte gebruiken, stelt u dit veld aan **onbeperkt**. U kunt ook maken en pas een bandbreedtesjabloon voor het toewijzen van bandbreedte op basis van planning toe.

De volgende procedures wordt uitgelegd hoe u de StorSimple **volumecontainers** blade om de volgende algemene bewerkingen te voltooien:

* Een volumecontainer toevoegen
* Een volumecontainer aanpassen
* Een volumecontainer

## <a name="add-a-volume-container"></a>Een volumecontainer toevoegen
De volgende stappen uitvoeren om een volumecontainer toevoegen.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Een volumecontainer aanpassen
Voer de volgende stappen uit voor het wijzigen van een volumecontainer.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Een volumecontainer
Een volumecontainer heeft volumes in het. Het kan alleen worden verwijderd als alle volumes die zijn opgenomen in het eerst worden verwijderd. Voer de volgende stappen uit als u wilt verwijderen van een volumecontainer.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [beheer van StorSimple-volumes](storsimple-8000-manage-volumes-u2.md). 
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

