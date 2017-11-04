---
title: StorSimple 8000 series Hardwarevervanging onderdeel | Microsoft Docs
description: Hierin wordt beschreven hoe u veilig vervangt de PCMs, accu, controller-modules, EBOD domeincontrollers, schijven en chassis van een StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.custom: 
ms.openlocfilehash: 6de50c5031db59176bdf17ecc69b934559220f6a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Vervangen van een hardware-onderdeel op uw StorSimple 8000 series apparaat

## <a name="overview"></a>Overzicht
De hardware-onderdeel vervanging zelfstudies beschrijven de hardware-onderdelen van uw Microsoft Azure StorSimple 8000 series apparaat en de stappen die nodig zijn om te verwijderen en ze vervangen. In dit artikel beschrijft de pictogrammen veiligheid, bevat verwijzingen naar de gedetailleerde zelfstudies en geeft een lijst van de onderdelen die vervangen zijn.

> [!IMPORTANT]
> Voordat u probeert te verwijderen of vervangen van een StorSimple-onderdeel, zorg ervoor dat u wordt aangeraden de [veiligheid pictogram conventies](#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).


### <a name="safety-icon-conventions"></a>Veiligheid pictogram conventies
De volgende tabel beschrijft de veiligheid pictogrammen in deze zelfstudies. Aandacht besteedt aan deze pictogrammen veiligheid terwijl u de stappen doorloopt voor het Apparaatonderdelen verwijderen en vervangen.

| Pictogram | Tekst | Aanvullende informatie |
|:--- |:--- |:--- |
| ![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) |**GEVAAR!** |Hiermee geeft u een gevaarlijke situatie die als niet wordt vermeden, in overlijden of ernstige schade resulteren. Dit woord signaal is beperkt tot de meest extreme situaties. |
| ![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) |**WAARSCHUWING!** |Hiermee geeft u een gevaarlijke situatie die als niet voorkomen, tot overlijden of ernstige schade leiden kan. |
| ![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Caution.png) |**WAARSCHUWING!** |Hiermee geeft u een gevaarlijke situatie die als niet voorkomen, in kleine of matige schade resulteren kan. |
| ![Pictogram van de kennisgeving](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**LET OP:** |Hiermee wordt aangegeven beschouwd als belangrijk, maar niet gevaar-gerelateerde informatie. |
| ![Elektrische gebruik-pictogram](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrische gebruik gevaar** |Hiermee wordt aangegeven hoge spanning. |
| ![Pictogram zware gewicht](./media/storsimple-hardware-component-replacement/Weight.png) |**Zware gewicht** | |
| ![Er is geen geschikte delen-pictogram van gebruiker](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Geen geschikte onderdelen van de gebruiker** |Geen toegang tot tenzij goed zijn opgeleid. |
| ![Pictogram voor lees-instructies](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Lees eerst de instructies** | |
| ![Gevaar tippictogram](./media/storsimple-hardware-component-replacement/TipHazard.png) |**Tip gevaar** | |

### <a name="before-you-begin"></a>Voordat u begint
Tijd om uzelf bekend met de veiligheidsinformatie over uw apparaat en de veiligheid pictogrammen in deze zelfstudie gebruikt. Ga naar [veilig installeren en gebruiken van uw StorSimple-apparaat](storsimple-safety.md) voor volledige informatie. Lees de [voorzorgsmaatregelen](storsimple-safety.md#handling-precautions) voordat u uw StorSimple-apparaat verwerkt.

Voordat u het onderdeel vervangt, houd rekening met de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrische gebruik pictogram](./media/storsimple-hardware-component-replacement/Electric.png) **waarschuwing!**

* Moet u uzelf goed met behulp van een elektrostatische zuivering of antistatische mat bij het verwerken van modules en -onderdelen van uw StorSimple-apparaat.
* Niet alle circuits aanraakt. De opgegeven ingangen en handleidingen gebruiken tijdens de verwerking van de onderdelen die mogelijk beschikbaar circuits hebt gemaakt.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![Let pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **aankondiging:**

Wanneer u een module vervangt **verlaten nooit een leeg bay aan de achterzijde van de behuizing**. Verkrijgen van een vervanging van of lege module voordat u het probleem deel verwijdert.

## <a name="hardware-component-replacement-procedures"></a>Hardware-onderdeel vervanging procedures
Uw StorSimple 8000 series apparaat bestaat uit meerdere invoegtoepassing modules in de primaire en/of EBOD behuizingen. De 8100 heeft één primaire behuizing, terwijl de 8600 een dubbele behuizing-apparaat met een primaire-behuizing en een EBOD behuizing is.

De belangrijkste hardwareonderdelen in uw apparaat worden samengevat in de volgende tabellen. Klik op de koppeling in de **vervangingsprocedure** kolom naar de bijbehorende zelfstudie.

| Onderdelen | # Aanwezig | Invoegtoepassingmodule? | Vervangingsprocedure |
|:--- |:--- |:--- |:--- |
| Chassis |1 |Nee |[Vervang het chassis op uw StorSimple-apparaat](storsimple-8000-chassis-replacement.md) |
| Primaire domeincontrollers |2 |Ja |[Vervangen van een module van de domeincontroller op uw StorSimple-apparaat](storsimple-8000-controller-replacement.md) |
| 764W stroom en koeling Modules (PCMs) |2 |Ja |[Vervangen van een stroom en koeling Module op uw StorSimple-apparaat](storsimple-8000-power-cooling-module-replacement.md) |
| Back-up batterij |2 |Ja |[Vervang de module voor back-up batterij op uw StorSimple-apparaat](storsimple-8000-battery-replacement.md) |
| Harde schijven |12 |Ja |[Vervangen van een harde schijf op uw StorSimple-apparaat](storsimple-8000-disk-drive-replacement.md) |

**Tabel 1** hardwareonderdelen in de primaire behuizing

De primaire ruimte en de behuizing EBOD verschillen in de i/o-modules. Bovendien hebben de PCMs verschillende vermogen. De PCMs in de primaire behuizing zijn 764 W, die in de behuizing EBOD 580 zijn W. De PCMs in de primaire behuizing bevatten ook een back-up batterij-module.

| Onderdelen | # Aanwezig | Invoegtoepassingmodule? | Vervangingsprocedure |
|:--- |:--- |:--- |:--- |
| Chassis |1 |Nee |[Vervang het chassis op uw StorSimple-apparaat](storsimple-8000-chassis-replacement.md) |
| EBOD domeincontrollers |2 |Ja |[Vervangen van een domeincontroller EBOD op uw StorSimple-apparaat](storsimple-8000-ebod-controller-replacement.md) |
| 580 w bij stroom en koeling Modules (PCMs) |2 |Ja |[Vervangen van een stroom en koeling Module op uw StorSimple-apparaat](storsimple-8000-power-cooling-module-replacement.md) |
| Harde schijven |12 |Ja |[Vervangen van een harde schijf op uw StorSimple-apparaat](storsimple-8000-disk-drive-replacement.md) |

**Tabel 2** hardwareonderdelen in de behuizing EBOD

De invoegtoepassing modules op het apparaat zijn gemarkeerd in de volgende voor- en achterste diagrammen. U kunt deze diagrammen gebruiken om te bepalen van de locatie van de verschillende invoegtoepassing modules of vervanging vereist is. Het front-diagram toont de schijfstations en de achterste diagrammen van de behuizing EBOD en het weergeven van de primaire behuizing de invoegtoepassing modules.

![Frontplane van apparaat met harde schijven](./media/storsimple-hardware-component-replacement/IC741028.png)

**Afbeelding 1** Front van het apparaat

| Label | Beschrijving |
|:--- |:--- |
| 0 - 11 |Harde schijven (totaal van 12) |

Zowel de primaire ruimte en de behuizing EBOD hebben station carrier modules. Het chassis ook nieuwste twaalf 3.5" schijfstations gerangschikt in een indeling 3 bij 4.

![Backplane apparaat primaire behuizing modules](./media/storsimple-hardware-component-replacement/IC740994.png)

**Afbeelding 2** achterzijde van de primaire behuizing

| Label | Beschrijving |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Controller 0 |
| 4 |Controller 1 |

![Backplane apparaat EBOD behuizing invoegtoepassing modules](./media/storsimple-hardware-component-replacement/IC769599.png)

**Afbeelding 3** achterzijde van de behuizing EBOD

| Label | Beschrijving |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD-Controller 0 |
| 4 |EBOD Controller 1 |

## <a name="field-replaceable-units"></a>Veld replaceable eenheden
De volgende veld replaceable eenheden (FRU) zijn beschikbaar voor uw StorSimple-apparaat:

* Chassis (met inbegrip van het paneel geïntegreerde operations)
* 764 W AC PCM
* 580 W AC PCM
* Harde schijf met station carrier-module
* Controllermodule
* EBOD controllermodule
* Back-up batterij module
* Spoor kit monteren

Neem [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) rangschikken van een van deze eenheden vervanging.

## <a name="next-steps"></a>Volgende stappen
Bekijk alle [veiligheidsinformatie](storsimple-safety.md) voordat u probeert te vervangen door een StorSimple-hardware-onderdeel.

