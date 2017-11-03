---
title: Vervangen van een domeincontroller StorSimple 8600 EBOD | Microsoft Docs
description: Legt uit hoe verwijdert en vervangt u een of beide EBOD domeincontrollers op een StorSimple 8600-apparaat.
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
ms.openlocfilehash: 45699c267d1009c4884dd164fd3f2950d6d5f555
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Vervangen van een domeincontroller EBOD op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe ter vervanging van een defecte EBOD controllermodule op uw Microsoft Azure StorSimple-apparaat. Ter vervanging van een module van de domeincontroller EBOD, moet u:

* De defecte EBOD controller verwijderen
* Een nieuwe EBOD controller installeren

Houd rekening met de volgende informatie voordat u begint:

* Lege EBOD modules moeten worden ingevoegd in alle ongebruikte sleuven. De behuizing cool niet goed als een site wordt geopend.
* De controller EBOD hot verwisselbare is en kan worden verwijderd of vervangen. Verwijder een mislukte module niet totdat u een vervangende hebt. Wanneer u het proces voor het vervangen start, moet u deze voltooit binnen 10 minuten.

> [!IMPORTANT]
> Voordat u probeert te verwijderen of vervangen van een StorSimple-onderdeel, zorg ervoor dat u wordt aangeraden de [veiligheid pictogram conventies](storsimple-safety.md#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Een controller EBOD verwijderen
Zorg dat de andere EBOD controller-module actief en wordt uitgevoerd is voordat de mislukte EBOD controllermodule in uw StorSimple-apparaat kan worden vervangen. De volgende procedure en tabel wordt uitgelegd hoe de controller EBOD module verwijderen.

#### <a name="to-remove-an-ebod-module"></a>Een module EBOD verwijderen
1. Open de Azure-portal.
2. Ga naar het apparaat en navigeer naar **instellingen** > **Hardware health**, en controleer of de status van de LED voor de actieve EBOD controllermodule groen is en de LED voor de mislukte EBOD controller-module is rood.
3. Zoek de mislukte EBOD controllermodule aan het einde van het apparaat.
4. Verwijder de kabels die verbinding maken met de module van de domeincontroller EBOD de domeincontroller voordat u de module EBOD buiten het systeem.
5. Noteer de precieze SAS-poort van de module die EBOD domeincontroller die is verbonden met de domeincontroller. U moet het systeem herstellen naar deze configuratie nadat u de module EBOD vervangen.
   
   > [!NOTE]
   > Normaal gesproken is dit poort A, die wordt aangeduid als **worden gehost in** in het volgende diagram.
   
    ![Backplane van EBOD-controller](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Afbeelding 1** EBOD van Back-module
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Fout met betrekking tot LED |
   | 2 |Power LED |
   | 3 |SAS-connectors |
   | 4 |SAS LED 's |
   | 5 |Seriële poorten factory alleen voor gebruik |
   | 6 |Poort (Host in) |
   | 7 |Poort B (Host out) |
   | 8 |Poort C (alleen Factory gebruik) |

## <a name="install-a-new-ebod-controller"></a>Een nieuwe EBOD controller installeren
De volgende procedure en tabel wordt uitgelegd hoe u een domeincontroller EBOD module in uw StorSimple-apparaat installeert.

#### <a name="to-install-an-ebod-controller"></a>Een controller EBOD installeren
1. Controleer het apparaat EBOD voor schade, met name op de interface-connector. Installeer de nieuwe EBOD controller niet als een pincodes verbogen zijn.
2. Schuif de module met de vergrendelingen in de open positie in de behuizing totdat de vergrendelingen benaderen.
   
    ![EBOD controller installeren](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Afbeelding 2** de EBOD controller-module installeren
3. Sluit de vergrendeling. U moet een klik horen als het slot stelt.
   
    ![Vrijgeven van EBOD vergrendeling](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Afbeelding 3** sluiten van de module EBOD vergrendeling
4. Sluit de kabels. Gebruik de exacte configuratie die aanwezig zijn voordat de vervangende was. Zie het volgende diagram en de volgende tabel voor meer informatie over het aansluiten van de kabels.
   
    ![Uw apparaat 4U voor power bekabelen](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Afbeelding 4**. Opnieuw verbinding te maken kabels
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Primaire behuizing |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Controller 0 |
   | 5 |Controller 1 |
   | 6 |EBOD-controller 0 |
   | 7 |EBOD controller 1 |
   | 8 |EBOD behuizing |
   | 9 |Power Distribution Units |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

