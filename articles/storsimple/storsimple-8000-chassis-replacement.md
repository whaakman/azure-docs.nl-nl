---
title: Vervang chassis op StorSimple 8000-apparaat | Microsoft Docs
description: Beschrijft hoe u verwijdert en vervangt u het chassis voor uw StorSimple primaire behuizing of EBOD behuizing.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 7dfc39f4d08c8a49d1564a0a5bd7e3ef4156e3fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61312442"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Vervang het chassis op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe om te verwijderen en een chassis in een StorSimple 8000-apparaat vervangen. Het StorSimple 8100-model is een één behuizing-apparaat (één chassis), terwijl de 8600 een dubbele behuizing-apparaat (twee chassis is). Zijn mogelijk twee chassis dat op het apparaat kan mislukken voor een model 8600: het chassis voor de primaire behuizing of het chassis voor de EBOD-behuizing.

De vervangende chassis dat wordt geleverd door Microsoft is in beide gevallen moet leeg zijn. Er is geen voeding en koeling Modules (PCMs), controller-modules, solid state harde schijven (SSD's), harde schijven (HDD's) of EBOD-modules worden opgenomen.

> [!IMPORTANT]
> Voordat u verwijdert en vervangt het chassis, lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-chassis"></a>Verwijderen van het chassis
Voer de volgende stappen uit als u wilt verwijderen van het chassis op uw StorSimple-apparaat.

#### <a name="to-remove-a-chassis"></a>Verwijderen van een chassis
1. Zorg ervoor dat het StorSimple-apparaat wordt afgesloten en niet met alle bronnen van de stroom verbonden.
2. Verwijder alle netwerk- en SAS-kabels, indien van toepassing.
3. Verwijder de eenheid van het rek te plaatsen.
4. Verwijder elk van de stations en houd er rekening mee de implementatiesleuven waarvan ze zijn verwijderd. Zie voor meer informatie, [verwijderen van het schijfstation](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. Op de EBOD-behuizing (indien dit het chassis dat is mislukt is), verwijderen van de modules EBOD-controller. Zie voor meer informatie, [een EBOD-controller verwijderen](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    Op de primaire behuizing (indien dit het chassis dat is mislukt is), verwijderen van de controllers en houd er rekening mee de implementatiesleuven waarvan ze zijn verwijderd. Zie voor meer informatie, [verwijderen van een domeincontroller](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Installeer het chassis
Voer de volgende stappen uit voor het installeren van het chassis op uw StorSimple-apparaat.

#### <a name="to-install-a-chassis"></a>Voor het installeren van een chassis
1. Koppel het chassis in het rek te plaatsen. Zie voor meer informatie, [Rack koppelen uw StorSimple 8100-apparaat](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) of [Rack koppelen uw StorSimple 8600-apparaat](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Nadat het chassis is gekoppeld in het rack, installeert u de modules netwerkcontroller in de dezelfde functies die ze eerder zijn geïnstalleerd in.
3. Installeer de stations in de dezelfde functies en -sleuven die ze eerder zijn geïnstalleerd in.
   
   > [!NOTE]
   > U wordt aangeraden dat u eerst de SSD's in de sleuven installeren en vervolgens de HDD's installeren.
  
4. Het apparaat is gekoppeld in het rek en de onderdelen zijn geïnstalleerd, uw apparaat aansluiten op de juiste energiebronnen en schakel het apparaat. Zie voor meer informatie, [uw StorSimple 8100-apparaat bekabelen](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw StorSimple 8600-apparaat bekabelen](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

