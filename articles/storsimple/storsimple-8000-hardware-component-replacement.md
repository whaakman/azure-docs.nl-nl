---
title: StorSimple 8000-serie Hardwarevervanging onderdeel | Microsoft Docs
description: Hierin wordt beschreven hoe u veilig vervangt de PCMs, accu, controller-modules, EBOD domeincontrollers, schijven en chassis van een StorSimple-apparaat.
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
ms.date: 06/02/2017
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: e05a37122647d4979089f0ba00b1fc15f9b84b0f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433651"
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Vervangen door een hardware-onderdeel op de StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht
De hardware-onderdeel vervanging zelfstudies beschrijven de hardware-onderdelen van uw Microsoft Azure StorSimple 8000-apparaat en de stappen die nodig zijn om te verwijderen en deze te vervangen. In dit artikel beschrijft de pictogrammen veiligheid, bevat verwijzingen naar de gedetailleerde zelfstudies en geeft een lijst van de onderdelen die vervangen zijn.

> [!IMPORTANT]
> Voordat u probeert te verwijderen of vervangen van een StorSimple-onderdeel, zorg ervoor dat u de [veiligheid pictogram conventies](#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).


### <a name="safety-icon-conventions"></a>Veiligheid pictogram conventies
De volgende tabel beschrijft de veiligheid pictogrammen die worden gebruikt in deze zelfstudies. Let vooral op deze pictogrammen veiligheid terwijl u de stappen voor het Apparaatonderdelen verwijderen en vervangen.

| Pictogram | Tekst | Aanvullende informatie |
|:--- |:--- |:--- |
| ![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) |**GEVAAR!** |Geeft aan dat een gevaarlijke situaties die, als er geen vermeden, in overlijden of ernstige schade resulteert. Dit woord signaal is beperkt tot de meest uitzonderlijke situaties. |
| ![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) |**WAARSCHUWING!** |Geeft aan dat een gevaarlijke situaties die als niet voorkomen, tot overlijden of ernstige schade leiden kan. |
| ![Let op het pictogram](./media/storsimple-hardware-component-replacement/Caution.png) |**LET OP!** |Geeft aan dat een gevaarlijke situaties die als niet voorkomen, tot schade voor kleine of matige leiden kan. |
| ![U ziet dat het pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**LET OP:** |Geeft aan dat beschouwd als belangrijk, maar niet op risico productspecifieke informatie. |
| ![Elektrische schudt pictogram](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrische schudt gevaar** |Geeft aan dat hoge spanning. |
| ![Pictogram zware gewicht](./media/storsimple-hardware-component-replacement/Weight.png) |**Zware gewicht** | |
| ![Er is geen Gebruikerspictogram onderhouden delen](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Geen onderdelen voor het onderhouden van gebruiker** |Geen toegang tot tenzij goed zijn opgeleid. |
| ![Pictogram van de instructies lezen](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Lees eerst de instructies** | |
| ![Gevaar tippictogram](./media/storsimple-hardware-component-replacement/TipHazard.png) |**Tip gevaar** | |

### <a name="before-you-begin"></a>Voordat u begint
Maak uzelf bekend met de veiligheidsinformatie over uw apparaat en de veiligheid pictogrammen in deze zelfstudie gebruikt. Ga naar [veilig installeren en gebruiken van uw StorSimple-apparaat](storsimple-safety.md) voor volledige informatie. Lees de [voorzorgsmaatregelen](storsimple-safety.md#handling-precautions) voordat u uw StorSimple-apparaat verwerken.

Houd rekening met de volgende informatie voordat u probeert te vervangen door een component.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrische schudt pictogram](./media/storsimple-hardware-component-replacement/Electric.png) **waarschuwing!**

* Moet u uzelf goed met behulp van een elektrostatische ontlading of antistatische mat bij het verwerken van modules en -onderdelen van uw StorSimple-apparaat.
* Een circuit niet worden gebruikt. Gebruik de opgegeven ingangen en handleidingen tijdens de verwerking van onderdelen die mogelijk beschikbaar circuit hebt gemaakt.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![Let op het pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **kennisgeving:**

Wanneer u een module vervangt **nooit laat een lege bay aan de achterzijde van de behuizing**. Een vervangende of lege module verkrijgen voordat u verwijdert de probleem-onderdeel.

## <a name="hardware-component-replacement-procedures"></a>Hardware-onderdeel vervanging procedures
Uw apparaat StorSimple 8000-serie bestaat uit verschillende Plug-ins in de primaire en/of EBOD-behuizingen. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire behuizing en een EBOD-behuizing is.

De belangrijkste hardware-onderdelen in uw apparaat worden samengevat in de volgende tabellen. Klik op de koppeling in de **vervangingsprocedure** kolom aan de slag met de bijbehorende zelfstudie.

| Onderdelen | # Aanwezig | Module? | Vervangingsprocedure |
|:--- |:--- |:--- |:--- |
| Chassis |1 |Nee |[Vervang het chassis op uw StorSimple-apparaat](storsimple-8000-chassis-replacement.md) |
| Primaire domeincontrollers |2 |Ja |[Vervangen van een controllermodule op uw StorSimple-apparaat](storsimple-8000-controller-replacement.md) |
| 764W voeding en koeling Modules (PCMs) |2 |Ja |[Stroom- en koelmodule van StorSimple-apparaat vervangen](storsimple-8000-power-cooling-module-replacement.md) |
| Back-upbatterij |2 |Ja |[Noodaccumodule van StorSimple-apparaat vervangen](storsimple-8000-battery-replacement.md) |
| Schijfstations |12 |Ja |[Vervang een schijfstation op uw StorSimple-apparaat](storsimple-8000-disk-drive-replacement.md) |

**Tabel 1** hardwareonderdelen in de primaire behuizing

De primaire behuizing en de behuizing EBOD verschillen in de i/o-modules. Bovendien hebben de PCMs verschillende vermogen. De PCMs in de primaire behuizing zijn 764 W, die in de behuizing EBOD 580 zijn W. De PCMs in de primaire behuizing bevatten ook een back-upbatterij-module.

| Onderdelen | # Aanwezig | Module? | Vervangingsprocedure |
|:--- |:--- |:--- |:--- |
| Chassis |1 |Nee |[Vervang het chassis op uw StorSimple-apparaat](storsimple-8000-chassis-replacement.md) |
| EBOD-controllers |2 |Ja |[Op uw StorSimple-apparaat een EBOD-controller vervangen](storsimple-8000-ebod-controller-replacement.md) |
| 580 w bij voeding en koeling Modules (PCMs) |2 |Ja |[Stroom- en koelmodule van StorSimple-apparaat vervangen](storsimple-8000-power-cooling-module-replacement.md) |
| Schijfstations |12 |Ja |[Vervang een schijfstation op uw StorSimple-apparaat](storsimple-8000-disk-drive-replacement.md) |

**Tabel 2** hardwareonderdelen in de behuizing EBOD

De invoegtoepassing modules op het apparaat zijn gemarkeerd in de volgende voor- en achterkant diagrammen. U kunt deze diagrammen gebruiken om te bepalen van de locatie van de verschillende Plug-ins als vervanging vereist is. Het front diagram toont de schijfstations en de achterzijde diagrammen van de behuizing EBOD en het weergeven van de primaire behuizing de invoegtoepassing modules.

![Voorpaneel van apparaat met harde schijven](./media/storsimple-hardware-component-replacement/IC741028.png)

**Afbeelding 1** Front van het apparaat

| Label | Description |
|:--- |:--- |
| 0 - 11 |Harde schijven (totaal van 12) |

Zowel de primaire behuizing en de behuizing EBOD hebben station carrier modules. Het chassis ook nieuwste twaalf 3,5-inch harde schijven die zijn gerangschikt in een indeling 3 van 4.

![Backplane van apparaat primaire behuizing-modules](./media/storsimple-hardware-component-replacement/IC740994.png)

**Afbeelding 2** achterzijde de primaire behuizing

| Label | Description |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Controller 0 |
| 4 |Controller 1 |

![Backplane van apparaat EBOD behuizing Plug-ins](./media/storsimple-hardware-component-replacement/IC769599.png)

**Afbeelding 3** achterzijde de behuizing EBOD

| Label | Description |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD-Controller 0 |
| 4 |EBOD-Controller 1 |

## <a name="field-replaceable-units"></a>Veld replaceable eenheden
De volgende veld replaceable eenheden (FRU) zijn beschikbaar voor uw StorSimple-apparaat:

* Chassis (met inbegrip van het paneel geïntegreerde bewerkingen)
* 764 W AC PCM
* 580 W AC PCM
* Harde schijf met station provider-module
* Controllermodule
* EBOD-controller-module
* Back-upbatterij-module
* Rack spoor kit koppelen

Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om een van deze vervanging-eenheden.

## <a name="next-steps"></a>Volgende stappen
Bekijk alle [veiligheidsinformatie](storsimple-safety.md) voordat u probeert te vervangen door een StorSimple-hardware-onderdeel.

