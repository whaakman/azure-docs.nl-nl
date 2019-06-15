---
title: Een StorSimple 8600 EBOD-controller vervangen | Microsoft Docs
description: Wordt uitgelegd hoe om te verwijderen en vervangen door een of beide EBOD-controllers op een StorSimple 8600-apparaat.
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
ms.openlocfilehash: b05d1f36d1e74b3d915e216676859654fbcbacf3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60578660"
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Op uw StorSimple-apparaat een EBOD-controller vervangen

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe u vervangen door een defecte EBOD-controller-module op uw Microsoft Azure StorSimple-apparaat. Als u wilt een module van EBOD-controller vervangen, moet u naar:

* De defecte EBOD-controller verwijderen
* Een nieuwe EBOD-controller installeren

Houd rekening met de volgende informatie voordat u begint:

* Lege EBOD-modules moeten worden ingevoegd in alle niet-gebruikte sleuven. De behuizing cool niet goed als een site wordt geopend.
* De EBOD-controller is hot swappable en kan worden verwijderd of vervangen. Verwijder een mislukte module niet totdat u een vervangende hebt. Wanneer u het proces voor het vervangen start, moet u deze voltooien binnen 10 minuten.

> [!IMPORTANT]
> Voordat u probeert te verwijderen of vervangen van een StorSimple-onderdeel, zorg ervoor dat u de [veiligheid pictogram conventies](storsimple-safety.md#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Verwijderen van een EBOD-controller
Voordat u de mislukte EBOD-controller-module in uw StorSimple-apparaat vervangt, ervoor zorgen dat de andere EBOD-controller-module actief en wordt uitgevoerd is. De volgende procedure en tabel wordt uitgelegd hoe u de module EBOD-controller verwijderen.

#### <a name="to-remove-an-ebod-module"></a>Een EBOD-module verwijderen
1. Open Azure Portal.
2. Ga naar uw apparaat en navigeer naar **instellingen** > **hardwarestatus**, en controleer of de status van de LED voor de actieve EBOD-controller-module is groen en de LED voor de mislukte EBOD-controller module is rood.
3. Zoek de mislukte EBOD-controller-module aan het einde van het apparaat.
4. Verwijder de kabels die verbinding maken met de EBOD-controller-module met de domeincontroller alvorens de EBOD-module uit het systeem.
5. Noteer de exacte SAS-poort van de EBOD-controller-module die is verbonden met de domeincontroller. U moet het systeem om deze configuratie te herstellen nadat u de module EBOD vervangen.
   
   > [!NOTE]
   > Normaal gesproken worden deze poort A, waarmee wordt aangeduid als **hosten** in het volgende diagram.
   
    ![Backplane van EBOD-controller](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Afbeelding 1** terug van EBOD-module
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Fout met betrekking tot LED |
   | 2 |LED voor stroom |
   | 3 |SAS-connectors |
   | 4 |SAS-LED 's |
   | 5 |Seriële poorten voor alleen factory gebruiken |
   | 6 |Poort (Host in) |
   | 7 |Poort B (Host out) |
   | 8 |Poort C (alleen Factory gebruik) |

## <a name="install-a-new-ebod-controller"></a>Een nieuwe EBOD-controller installeren
De volgende procedure en tabel wordt uitgelegd hoe u een EBOD-controller-module in uw StorSimple-apparaat installeert.

#### <a name="to-install-an-ebod-controller"></a>Voor het installeren van een EBOD-controller
1. Controleer de EBOD-apparaat voor schade, met name voor de interface-connector. Installeer de nieuwe EBOD-controller niet als een pincodes verbogen zijn.
2. Met de vergrendelingen in de geopende positie, schuift u de module in de behuizing totdat de vergrendelingen betrekken.
   
    ![EBOD-controller installeren](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Afbeelding 2** de EBOD-controller-module installeren
3. Sluit de vergrendeling. U moet één klik van u horen als de vergrendeling maken.
   
    ![Vrijgeven van EBOD-vergrendeling](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Afbeelding 3** sluiten van de vergrendeling van EBOD-module
4. Sluit de kabels. Gebruik de exacte configuratie die voordat u de vervanging is. Zie het volgende diagram en de volgende tabel voor meer informatie over het verbinden van de kabels.
   
    ![Uw apparaat 4U voor power bekabelen](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Afbeelding 4**. Opnieuw verbinden kabels
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Primaire behuizing |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Controller 0 |
   | 5 |Controller 1 |
   | 6 |EBOD-controller 0 |
   | 7 |EBOD-controller 1 |
   | 8 |EBOD behuizing |
   | 9 |Power Distribution Units |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

