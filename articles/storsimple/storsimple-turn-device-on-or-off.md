---
title: De StorSimple 8000-apparaat in of uit te schakelen | Microsoft Docs
description: Wordt uitgelegd hoe u een nieuwe StorSimple-apparaat inschakelen, inschakelen op een apparaat dat is afgesloten of verloren power en een actief apparaat uitschakelen.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670963"
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Schakel in of uit de StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht
Afsluiten van een Microsoft Azure StorSimple-apparaat is niet vereist als onderdeel van de werking van het normale systeem. U moet echter mogelijk inschakelen op een nieuw apparaat of een apparaat dat opnieuw moest worden afgesloten. Over het algemeen is een afsluiten vereist in gevallen waarin u moet mislukte hardware vervangen, fysiek verplaatsen een eenheid of duren voordat een apparaat buiten gebruik gesteld. Deze zelfstudie beschrijft de vereiste procedure voor het inschakelen van of afsluiten van uw StorSimple-apparaat in verschillende scenario's.

## <a name="turn-on-a-new-device"></a>Een nieuw apparaat inschakelen
De stappen voor het inschakelen van een StorSimple-apparaat voor de eerste keer is afhankelijk van of het apparaat is een 8100 of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing-apparaat met een primaire behuizing en een EBOD-behuizing is. De gedetailleerde stappen voor beide modellen worden in de volgende secties besproken.

* [Nieuw apparaat met alleen primaire behuizing](#new-device-with-primary-enclosure-only)
* [Nieuw apparaat met EBOD behuizing](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nieuw apparaat met alleen primaire behuizing
Het StorSimple 8100-model is een apparaat met één behuizing. Uw apparaat bevat redundante voeding en koeling Modules (PCMs). Zowel PCMs moet worden geïnstalleerd en die zijn verbonden met verschillende energiebronnen om hoge beschikbaarheid te garanderen.

De volgende stappen uitvoeren om uw apparaat voor power bekabelen.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Voor apparaatconfiguratie voltooien en instructies bekabeling, gaat u naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md). Zorg ervoor dat u de instructies precies volgen.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Nieuw apparaat met EBOD behuizing
Het StorSimple 8600-model heeft zowel een primaire behuizing en een EBOD-behuizing. Hiervoor moet de eenheden op elkaar worden bekabeld voor SAS Serial Attached SCSI ()-connectiviteit en power.

Bij het instellen van dit apparaat voor de eerste keer, voer de stappen voor het SAS-bekabeling eerst en voltooit u de stappen voor het power-bekabeling.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Voor apparaatconfiguratie voltooien en instructies bekabeling, gaat u naar [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md). Zorg ervoor dat u de instructies precies volgen.

## <a name="turn-on-a-device-after-shutdown"></a>Inschakelen op een apparaat dat na afsluiten
De stappen voor het inschakelen van een StorSimple-apparaat nadat deze is uitgeschakeld zijn verschillend, afhankelijk van of het apparaat is een 8100 of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing-apparaat met een primaire behuizing en een EBOD-behuizing is.

* [Apparaat met alleen primaire behuizing](#device-with-primary-enclosure-only)
* [Apparaat met EBOD behuizing](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Apparaat met alleen primaire behuizing
Gebruik de volgende procedure om in te schakelen op een StorSimple-apparaat met een primaire behuizing en geen behuizing EBOD na een afsluiten.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Om in te schakelen op een apparaat met een primaire behuizing alleen
1. Zorg ervoor dat de kracht switches op beide stroom en koeling Modules (PCMs) op de positie OFF zijn. Als de switches niet op OFF positie, ze naar de positie OFF spiegelen en wacht tot de verlichting om uit te gaan.
2. Schakel op het apparaat door de power-switches op beide PCMs op de positie van spiegelen. Het apparaat moet inschakelen.
3. Controleer het volgende om te controleren of het apparaat volledig is ingeschakeld:
   
   1. De OK-LED's op zowel PCM-modules zijn groen.
   2. De status-LED's op beide controllers zijn groen.
   3. De blauwe LED op een van de controllers is knipperende, wat aangeeft dat de domeincontroller actief is.
      
      Uw apparaat is niet in orde als niet aan een van deze voorwaarden wordt voldaan. Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Apparaat met EBOD behuizing
Gebruik de volgende procedure om in te schakelen op een StorSimple-apparaat met een primaire behuizing en een behuizing EBOD na een afsluiten. Elke stap precies zoals beschreven in volgorde uitvoert. Dit niet doet, kan leiden tot verlies van gegevens.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Om in te schakelen op een apparaat met een primaire en een behuizing EBOD
1. Zorg ervoor dat de behuizing EBOD is verbonden met de primaire behuizing. Zie voor meer informatie, [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
2. Zorg ervoor dat de stroom en koeling Modules (PCMs) op de EBOD en de primaire-behuizingen in de positie OFF zijn. Als de switches niet op OFF positie, ze naar de positie OFF spiegelen en wacht tot de verlichting om uit te gaan.
3. De behuizing EBOD eerst inschakelen door de power-switches op beide PCMs op de positie van spiegelen. De PCM-LED's moeten groen. Een groene EBOD-controller LED op deze eenheid geeft aan dat de behuizing EBOD op.
4. Schakel op de primaire behuizing door de power-switches op beide PCMs op de positie van spiegelen. Het hele systeem worden nu op.
5. Controleer of de SAS-LED's zijn groen, die garandeert dat de verbinding tussen de behuizing EBOD en de primaire behuizing goed is.

## <a name="turn-on-a-device-after-a-power-loss"></a>Inschakelen op een apparaat na een stroomstoring
Een stroomstoring of onderbreking kunt afsluiten een StorSimple-apparaat. De stroomstoring kan gebeuren op een van de stroom wordt voorzien of beide voedingen. De herstelstappen zijn verschillend, afhankelijk van of het apparaat een model 8100 of 8600 is. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing-apparaat met een primaire behuizing en een EBOD-behuizing is. Deze sectie beschrijft de herstelprocedure voor elk scenario.

* [Apparaat met alleen primaire behuizing](#8100)
* [Apparaat met EBOD behuizing](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Apparaat met alleen primaire behuizing <a name="8100">
Het systeem kan de normale werking blijven als er plotseling uitvalt op een van de stroom wordt voorzien. Voor hoge beschikbaarheid van het apparaat, herstel echter power de voeding zo snel mogelijk.

Als er een stroomstoring of een stroomstoring op beide voedingen, wordt het systeem zich in een geordend en gecontroleerde manier afsluiten. Het systeem wordt automatisch ingeschakeld wanneer de stroom wordt hersteld.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Apparaat met EBOD behuizing <a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Een stroom plotseling uitvalt opgeven
Het systeem kan de normale werking blijven als er plotseling uitvalt op een van de voedingen op de primaire behuizing of de EBOD-behuizing. Echter voor hoge beschikbaarheid van het apparaat, zet power de voeding zo snel mogelijk.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Beide voedingen op primaire en EBOD-behuizingen plotseling uitvalt
Als er een stroomstoring of een stroomstoring op beide voedingen, de behuizing EBOD wordt onmiddellijk afgesloten en de primaire behuizing op een wijze geordend en gecontroleerde wordt afgesloten. Wanneer power wordt hersteld, wordt het apparaat wordt automatisch gestart.

Als de stroom handmatig wordt uitgeschakeld, klikt u vervolgens de volgende stappen om te zetten power op het systeem.

1. Schakel de EBOD-behuizing.
2. Nadat de EBOD-behuizing ingeschakeld is, schakelt u op de primaire behuizing.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Beide voedingen op EBOD behuizing plotseling uitvalt
Bij het instellen van de kabels, moet u ervoor zorgen dat de EBOD nooit alleen is verbonden met een afzonderlijke PDU. Als de EBOD en primaire behuizing niet op hetzelfde moment, wordt het systeem wordt hersteld.

Als er slechts de behuizing EBOD op beide voedingen mislukt, wordt het systeem wordt niet automatisch hersteld. De volgende stappen om te schakelen op het systeem en het herstellen naar een goede status:

1. Als de primaire behuizing is ingeschakeld, schakelen zowel voeding en koeling Modules (PCMs) uit.
2. Wacht een paar minuten voor het systeem om af te sluiten.
3. Schakel de EBOD-behuizing.
4. Nadat de EBOD-behuizing ingeschakeld is, schakelt u op de primaire behuizing.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Een apparaat inschakelen nadat de primaire en EBOD behuizing verbinding is verbroken
Als de verbinding verbroken tussen de stand-by-controller en de bijbehorende EBOD-controller wordt, blijft het apparaat werken. Als de verbinding tussen de actieve controller van systeem en de bijbehorende EBOD-controller verbroken is, failover moet worden uitgevoerd en het apparaat moet blijven werken die normaal werken.

Wanneer beide SAS Serial Attached SCSI ()-kabels zijn verwijderd of de verbinding tussen de behuizing EBOD en de primaire behuizing is verbroken, wordt het apparaat niet meer werken. Op dit moment de volgende stappen uitvoeren.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Als u wilt inschakelen op het apparaat na de verbinding is verbroken
1. Toegang tot de achterkant van het apparaat.
2. Als de SAS-kabel-verbinding tussen de behuizing EBOD en de primaire behuizing verbroken is, worden alle SAS lane LED's op de behuizing EBOD uitschakelen.
3. Zowel voeding en koeling Modules (PCMs) sluit de behuizing EBOD en de primaire behuizing.
4. Wacht totdat de verlichting op de achterkant van zowel de bijlagen uitschakelen.
5. U kunt de SAS-kabels en zorg ervoor dat er een goede verbinding tussen de behuizing EBOD en de primaire behuizing.
6. De behuizing EBOD eerst inschakelen door het spiegelen van beide PCM-switches op de positie op.
7. Zorg ervoor dat de behuizing EBOD op door te controleren dat de groene LED is ingesteld op ON.
8. Schakel op de primaire behuizing.
9. Zorg ervoor dat de primaire behuizing op door te controleren dat de controller groen-LED is ingesteld op ON.
10. Controleer of de EBOD behuizing verbinding met de primaire behuizing is goed door te controleren dat de SAS-baan LED's (vier per EBOD-controller) zijn alle op.

> [!IMPORTANT]
> Als de SAS-kabels beschadigd zijn of de verbinding tussen de behuizing EBOD en de primaire behuizing niet goed, is wanneer u op het systeem inschakelen, gaat deze in de herstelmodus. Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als dit het geval is.


## <a name="turn-off-a-running-device"></a>Een actief apparaat uitschakelen
Een actieve StorSimple-apparaat moet mogelijk worden afgesloten als deze wordt verplaatst, genomen buiten gebruik gesteld, of heeft een niet-functionerende onderdeel dat moet worden vervangen. De stappen zijn verschillend, afhankelijk van het StorSimple-apparaat is of een 8100 of een 8600-model. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing-apparaat met een primaire behuizing en een EBOD-behuizing is. In deze sectie worden de stappen om een actief apparaat af te sluiten.

* [Apparaat met primaire behuizing](#8100a)
* [Apparaat met EBOD behuizing](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Apparaat met primaire behuizing <a name="8100a">
Als u wilt op een wijze geordend en gecontroleerd op het apparaat wordt afgesloten, kunt u dit doen via Azure portal of via de Windows PowerShell voor StorSimple. 

> [!IMPORTANT]
> Niet een actief apparaat uitschakelen met behulp van de / uit-knop op de achterkant van het apparaat.
> 
> Voordat het apparaat wordt afgesloten, ervoor zorgen dat alle onderdelen van het apparaat in orde zijn. In de Azure-portal, gaat u naar **apparaten** > **Monitor** > **hardwarestatus**, en controleer of de status van alle onderdelen is groen. Dit geldt alleen voor een systeem in orde. Als het systeem wordt afgesloten omlaag naar een niet-functionerende onderdeel vervangen, moet u ziet een mislukte (rode) of gedegradeerd (geel) de status voor het betreffende onderdeel in de **hardwarestatus**.
> 
> 

Nadat u toegang de Windows PowerShell voor StorSimple of de Azure-portal tot, volgt u de stappen in [afgesloten een StorSimple-apparaat](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Apparaat met EBOD behuizing <a name="8600a">
> [!IMPORTANT]
> Voordat u de primaire behuizing en de behuizing EBOD afsluit, zorg ervoor dat alle onderdelen van het apparaat in orde. In de Azure-portal, gaat u naar **apparaten** > **Monitor** > **hardwarestatus**, en controleer of alle onderdelen in orde zijn.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Een actief apparaat met EBOD behuizing afsluiten
1. Volg alle stappen die worden vermeld in [afgesloten een StorSimple-apparaat](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) voor de primaire behuizing.
2. Nadat de primaire behuizing is afgesloten, sluit u de EBOD door spiegelen uit zowel voeding en koeling-Module (PCM).
3. Als u wilt controleren of de EBOD is afgesloten, Controleer of alle verlichting op de achterkant van de behuizing EBOD uitschakelen.

> [!NOTE]
> De SAS-kabels die worden gebruikt voor de behuizing EBOD verbinden met de primaire behuizing mogen niet worden verwijderd tot nadat het systeem wordt afgesloten.

## <a name="next-steps"></a>Volgende stappen
[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als er problemen optreden bij het inschakelen van of een StorSimple-apparaat wordt afgesloten.

