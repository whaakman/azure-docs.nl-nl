---
title: Uw StorSimple 8000 series apparaat in of uit te schakelen | Microsoft Docs
description: Legt uit hoe u een nieuwe StorSimple-apparaat inschakelen, inschakelen op een apparaat dat is afgesloten of stroomvoorziening en een actieve apparaat uitschakelen.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 95fd00608be9cfafb4c703c32ec3ed4713855ca5
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Op of het apparaat van de serie StorSimple 8000 uitschakelen

## <a name="overview"></a>Overzicht
Afsluiten van een Microsoft Azure StorSimple-apparaat is niet vereist als onderdeel van de werking van het normale systeem. U wilt, schakelt u een nieuw apparaat of een apparaat dat opnieuw moest worden afgesloten. In het algemeen is afsluiten vereist in gevallen waarin u moet mislukte hardware vervangen, fysiek verplaatsen een eenheid of een apparaat buiten dienst te nemen. Deze zelfstudie wordt de vereiste procedure voor het inschakelen van en afsluiten van het StorSimple-apparaat in verschillende scenario's beschreven.

## <a name="turn-on-a-new-device"></a>Een nieuw apparaat inschakelen
De stappen voor het inschakelen van een StorSimple-apparaat voor de eerste keer is afhankelijk van of het apparaat een 8100 is of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire-behuizing en een EBOD behuizing is. De gedetailleerde stappen voor beide modellen worden in de volgende secties besproken.

* [Nieuw apparaat met alleen primaire behuizing](#new-device-with-primary-enclosure-only)
* [Nieuw apparaat met EBOD behuizing](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nieuw apparaat met alleen primaire behuizing
Het model StorSimple 8100 is een apparaat met één behuizing. Uw apparaat bevat redundante stroom en koeling Modules (PCMs). Zowel PCMs moet worden geïnstalleerd en is verbonden met verschillende energiebeheerschema bronnen om hoge beschikbaarheid te garanderen.

De volgende stappen om uw apparaat voor power bekabelen uitvoeren.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Voor het apparaat voltooien en instructies bekabeling, gaat u naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md). Zorg ervoor dat u de instructies exact.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Nieuw apparaat met EBOD behuizing
Het model StorSimple 8600 heeft zowel een primaire-behuizing en een EBOD behuizing. Hiervoor moet de eenheden op elkaar worden bekabeld voor seriële gekoppelde SCSI (SAS)-verbinding en geen stroom.

Bij het instellen van dit apparaat voor de eerste keer, voer de stappen voor het SAS-kabels eerst en voltooi de stappen voor het power bekabeling.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Voor het apparaat voltooien en instructies bekabeling, gaat u naar [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md). Zorg ervoor dat u de instructies exact.

## <a name="turn-on-a-device-after-shutdown"></a>Een apparaat inschakelen na afsluiten
De stappen voor het inschakelen van een StorSimple-apparaat nadat deze is afgesloten zijn verschillend, afhankelijk van of het apparaat een 8100 is of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire-behuizing en een EBOD behuizing is.

* [Apparaten met alleen primaire behuizing](#device-with-primary-enclosure-only)
* [Apparaat met EBOD behuizing](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Apparaten met alleen primaire behuizing
Gebruik de volgende procedure om in te schakelen op een StorSimple-apparaat met een primaire-behuizing en geen behuizing EBOD na een afsluiten.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Een apparaat met een primaire behuizing alleen inschakelen
1. Zorg ervoor dat de kracht switches op beide stroom en koeling Modules (PCMs) op de positie OFF zijn. Als de switches niet op OFF positie, ze naar de positie OFF spiegelen en wacht totdat de lichten om uit te gaan.
2. Schakel op het apparaat door het spiegelen van de energie-switches op beide PCMs naar de ON-positie. Het apparaat moet inschakelen.
3. Controleer het volgende om te controleren of het apparaat volledig op:
   
   1. De OK LED's op beide PCM-modules zijn groen.
   2. De status LED's op beide domeincontrollers zijn groen.
   3. De blauwe LED op een van de controllers knippert, wat aangeeft dat de domeincontroller actief is.
      
      Uw apparaat is niet in orde als een van deze voorwaarden niet wordt voldaan. Neem [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Apparaat met EBOD behuizing
Gebruik de volgende procedure om in te schakelen op een StorSimple-apparaat met een primaire-behuizing en een behuizing EBOD na een afsluiten. Elke stap precies zoals beschreven in volgorde uitvoert. Niet doet, kan leiden tot verlies van gegevens.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Een apparaat met een primaire en een behuizing EBOD inschakelen
1. Zorg ervoor dat de behuizing EBOD is verbonden met de primaire behuizing. Zie voor meer informatie [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
2. Zorg ervoor dat de positie uit de stroom en koeling Modules (PCMs) op de EBOD en de primaire-behuizingen zijn. Als de switches niet op OFF positie, ze naar de positie OFF spiegelen en wacht totdat de lichten om uit te gaan.
3. De behuizing EBOD eerst inschakelen door het spiegelen van de energie-switches op beide PCMs naar de ON-positie. De PCM LED's moeten groene. Een groen EBOD controller LED op deze eenheid geeft aan dat de behuizing EBOD op.
4. De primaire behuizing inschakelen door het spiegelen van de energie-switches op beide PCMs naar de ON-positie. Het hele systeem worden nu op.
5. Controleer of de SAS-LED's groen, waardoor de verbinding tussen de behuizing EBOD en de primaire behuizing is goed.

## <a name="turn-on-a-device-after-a-power-loss"></a>Een apparaat na een stroomstoring inschakelen
Een stroomstoring of onderbreking kan een StorSimple-apparaat afsluiten. De stroomstoring kan gebeuren op een van de voedingen of beide voedingen. De herstelstappen zijn verschillend afhankelijk van of het apparaat een 8100 of een 8600-model is. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire-behuizing en een EBOD behuizing is. Deze sectie beschrijft de herstelprocedure voor elk scenario.

* [Apparaten met alleen primaire behuizing](#8100)
* [Apparaat met EBOD behuizing](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Apparaten met alleen primaire behuizing<a name="8100">
Het systeem kan de normale werking blijven als er stroomstoring op een van de stroom wordt voorzien. Echter, hoge beschikbaarheid van het apparaat, zodat herstellen power de voeding zo snel mogelijk.

Als er een stroomstoring of een stroomstoring op beide voedingen, wordt het systeem zich in een normaal en gecontroleerde wijze afgesloten. Het systeem wordt automatisch ingeschakeld wanneer de stroom wordt hersteld.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Apparaat met EBOD behuizing<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Stroomstoring op één power supply
Het systeem kan de normale werking blijven als er stroomstoring op een van de voedingen op de primaire behuizing of de EBOD behuizing. Echter, hoge beschikbaarheid van het apparaat, zodat herstel power de voeding zo snel mogelijk.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Stroomstoring op beide voedingen op primaire en EBOD behuizingen
Als er een stroomstoring of een stroomstoring op beide voedingen, wordt de behuizing EBOD wordt onmiddellijk afgesloten en wordt de primaire behuizing op een geordende en gecontroleerde manier wordt afgesloten. Wanneer power wordt hersteld, wordt het apparaat wordt automatisch gestart.

Als de macht handmatig is uitgeschakeld, klikt u vervolgens de volgende stappen nemen om power herstellen naar het systeem.

1. Schakel de EBOD behuizing.
2. Nadat de behuizing EBOD ingeschakeld is, schakelt u de primaire behuizing.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Stroomstoring op beide voedingen op EBOD behuizing
Bij het instellen van de kabels, moet u ervoor zorgen dat de EBOD nooit alleen is verbonden met een afzonderlijke PDU. Als de EBOD en primaire behuizing op hetzelfde moment uitvallen, wordt het systeem herstellen.

Als alleen de behuizing EBOD op beide voedingen mislukt, het systeem wordt niet automatisch worden hersteld. De volgende stappen nemen om inschakelen op het systeem en herstel hem status in orde:

1. Als de primaire behuizing is ingeschakeld, gaat u zowel stroom en koeling Modules (PCMs) uit.
2. Wacht enkele minuten duren voordat het systeem af te sluiten.
3. Schakel de EBOD behuizing.
4. Nadat de behuizing EBOD ingeschakeld is, schakelt u de primaire behuizing.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Een apparaat inschakelen nadat de primaire en EBOD behuizing verbinding wordt verbroken
Als de verbinding verbroken tussen de stand-by-controller en de bijbehorende EBOD-controller wordt, blijft het apparaat werken. Als de verbinding tussen de actieve controller van systeem en de bijbehorende EBOD-controller verbroken wordt, failover moet worden uitgevoerd en het apparaat moet blijven werken normaal.

Wanneer beide kabels seriële gekoppelde SCSI (SAS) zijn verwijderd of de verbinding tussen de behuizing EBOD en de primaire behuizing is verbroken, werkt het apparaat niet meer. Op dit punt wordt de volgende stappen uitvoeren.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Het apparaat inschakelen nadat de verbinding wordt verbroken
1. Toegang tot de achterzijde van het apparaat.
2. Als de SAS-kabel-verbinding tussen de behuizing EBOD en de primaire behuizing verbroken is, wordt alle SAS lane LED's op de behuizing EBOD zijn uitgeschakeld.
3. Zowel stroom en koeling Modules (PCMs) sluit de behuizing EBOD en de primaire behuizing.
4. Wacht totdat de lichten op de achterkant van zowel de bijlagen uitschakelen.
5. Plaats de SAS-kabels en zorg ervoor dat er een goede verbinding tussen de behuizing EBOD en de primaire behuizing.
6. De behuizing EBOD eerst inschakelen door het spiegelen van beide PCM-switches op de positie op.
7. Zorg ervoor dat de behuizing EBOD op door te controleren dat de groene LED is ingesteld op ON.
8. Schakel op de primaire behuizing.
9. Zorg ervoor dat de primaire behuizing op door te controleren dat de domeincontroller groen LED is ingesteld op ON.
10. Controleer of de EBOD behuizing verbinding met de primaire behuizing goed door te controleren dat de SAS-baan LED's (vier per EBOD-controller) zijn alle ON.

> [!IMPORTANT]
> Als de SAS-kabels beschadigd zijn of de verbinding tussen de behuizing EBOD en de primaire behuizing niet goed, is wanneer u het systeem inschakelen, gaat deze in de herstelmodus overgaat. Neem [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als dit gebeurt.


## <a name="turn-off-a-running-device"></a>Een actief apparaat uitschakelen
Een actieve StorSimple-apparaat moet mogelijk worden afgesloten als hij wordt verplaatst, buiten de service, of heeft een slecht werkende onderdeel dat moet worden vervangen. De stappen zijn verschillend, afhankelijk van of het StorSimple-apparaat een 8100 is of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire-behuizing en een EBOD behuizing is. Deze sectie worden de stappen voor het afsluiten van een apparaat uitgevoerd.

* [Apparaat met primaire behuizing](#8100a)
* [Apparaat met EBOD behuizing](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Apparaat met primaire behuizing<a name="8100a">
Om het apparaat in een geordende en gecontroleerde manier afsluiten, kunt u dit doen via de Azure-portal of via de Windows PowerShell voor StorSimple. 

> [!IMPORTANT]
> Niet worden afgesloten een apparaat uitgevoerd met behulp van de knop op de achterkant van het apparaat.
> 
> Voordat het apparaat wordt afgesloten, zorg ervoor dat alle apparaatonderdelen goed. Navigeer in de Azure-portal naar **apparaten** > **Monitor** > **Hardware health**, en controleer of de status van alle onderdelen van groen. Dit geldt alleen voor een goede systeem. Als het systeem wordt afgesloten omlaag naar een niet-functionerende onderdeel vervangen, moet u ziet een mislukte (rode) of gedegradeerd (geel) status voor het betreffende onderdeel in de **hardwarestatus**.
> 
> 

Wanneer u de Windows PowerShell voor StorSimple of Azure portal, volg de stappen in [een StorSimple-apparaat afsluiten](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Apparaat met EBOD behuizing<a name="8600a">
> [!IMPORTANT]
> Voordat u de primaire ruimte en de behuizing EBOD afsluit, zorg ervoor dat alle apparaatonderdelen goed. Navigeer in de Azure-portal naar **apparaten** > **Monitor** > **Hardware health**, en controleer of alle onderdelen in orde zijn.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Een actief apparaat met EBOD behuizing afsluiten
1. Volg de stappen die zijn vermeld in [een StorSimple-apparaat afsluiten](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) voor de primaire behuizing.
2. Nadat de primaire behuizing wordt afgesloten, sluit u de EBOD door spiegelen uit zowel stroom en koeling Module (PCM).
3. Om te controleren of de EBOD is afgesloten, Controleer of alle lichten op de achterkant van de behuizing EBOD uitschakelen.

> [!NOTE]
> De SAS-kabels die worden gebruikt voor de behuizing EBOD verbinding met de primaire behuizing mogen niet worden verwijderd pas nadat het systeem wordt afgesloten.

## <a name="next-steps"></a>Volgende stappen
[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als u problemen bij het afsluiten een StorSimple-apparaat of inschakelen.

