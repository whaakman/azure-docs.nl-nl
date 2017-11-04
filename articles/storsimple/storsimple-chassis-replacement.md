---
title: Vervang chassis op StorSimple 8000 series apparaat | Microsoft Docs
description: Beschrijft hoe verwijderen en het chassis voor uw StorSimple-primaire behuizing of EBOD behuizing vervangen.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 7bc23e64331ad18c604ffaa29476766827119cd4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Vervang het chassis op uw StorSimple-apparaat
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. De versie van dit artikel voor de nieuwe Azure portal, Ga naar [vervangt het chassis op uw StorSimple-apparaat](storsimple-8000-chassis-replacement.md). Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe verwijderen en een chassis in een serie StorSimple 8000 apparaat vervangen. Het StorSimple 8100-model is een apparaat met één behuizing (één chassis), terwijl de 8600 een dubbele behuizing-apparaat (twee chassis is). Voor een model 8600 zijn mogelijk twee chassis dat op het apparaat kan mislukken: het chassis voor de primaire behuizing of het chassis voor de EBOD behuizing.

In beide gevallen is de vervanging chassis dat wordt geleverd door Microsoft leeg. Er is geen stroom en koeling Modules (PCMs), controller-modules Solid-State harde schijven (SSD's), harde schijven (HDD's) of EBOD modules worden opgenomen.

> [!IMPORTANT]
> Voordat u verwijdert en vervangt het chassis, lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-the-chassis"></a>Verwijder het chassis
De volgende stappen uitvoeren om te verwijderen van het chassis op uw StorSimple-apparaat.

#### <a name="to-remove-a-chassis"></a>Verwijderen van een chassis
1. Zorg ervoor dat het StorSimple-apparaat wordt afgesloten en alle bronnen van de stroom is verbroken.
2. Verwijder alle netwerk- en SAS-kabels, indien van toepassing.
3. Verwijder de eenheid van het rek te plaatsen.
4. Verwijderen van de schijven en noteer de sleuven waarvan ze zijn verwijderd. Zie voor meer informatie [verwijderen van het schijfstation](storsimple-disk-drive-replacement.md#remove-the-disk-drive).
5. Op de behuizing EBOD (indien dit het chassis die niet zijn geslaagd is), de controller EBOD modules verwijderen. Zie voor meer informatie [verwijderen van een domeincontroller EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 
   
    Op de primaire behuizing (indien dit het chassis die niet zijn geslaagd is), verwijdert u de domeincontrollers en noteer de sleuven waarvan ze zijn verwijderd. Zie voor meer informatie [verwijderen van een domeincontroller](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Installeer het chassis
Voer de volgende stappen uit voor het installeren van het chassis op uw StorSimple-apparaat.

#### <a name="to-install-a-chassis"></a>Voor het installeren van een chassis
1. Koppel het chassis in het rek. Zie voor meer informatie [Rack koppelen uw StorSimple-8100-apparaat](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) of [Rack koppelen 8600 StorSimple-apparaat](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Nadat het chassis is gekoppeld in het rek, moet u de domeincontroller-modules installeren op dezelfde positie die ze eerder zijn geïnstalleerd in.
3. Installeer de stations in dezelfde posities en sleuven die ze eerder zijn geïnstalleerd in.
   
   > [!NOTE]
   > Het is raadzaam om eerst de SSD's in de sleuven installeren en installeer vervolgens de HDD's.
   > 
   > 
4. Uw apparaat aansluit op de juiste stroomvoorziening nadat het apparaat gekoppeld in het rek en de onderdelen zijn geïnstalleerd, en schakelt u het apparaat. Zie voor meer informatie [uw StorSimple 8100-apparaat bekabelen](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw StorSimple 8600-apparaat bekabelen](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).

