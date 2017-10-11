---
title: Vervangen van een harde schijf op een StorSimple-apparaat | Microsoft Docs
description: Legt uit hoe u een harde schijf op een primaire behuizing StorSimple of een behuizing EBOD vervangen.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 0659ab9d304dbfcce72e8c3c79edad68e70b9630
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a>Vervangen van een harde schijf op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe u kunt verwijderen en vervangen door een slecht werkende of mislukte harde schijf op een Microsoft Azure StorSimple-apparaat. Ter vervanging van een harde schijf, moet u:

* Werking stellen van de antitamper vergrendeling
* De schijf verwijderen
* De vervangende schijf installeren

> [!IMPORTANT]
> Voordat verwijderen en een schijf vervangen Lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="disengage-the-antitamper-lock"></a>Werking stellen van de antitamper vergrendeling
Deze procedure wordt uitgelegd hoe de antitamper vergrendelingen op uw StorSimple-apparaat kunnen worden in- of uitgeschakeld wanneer u de schijfstations vervangt. De antitamper vergrendelingen zijn aangebracht in het station carrier verwerkt en ze toegankelijk zijn via een kleine opening in de sectie vergrendeling van de greep. Schijven worden geleverd met vergrendelingen zijn ingesteld op de vergrendeling.

#### <a name="to-unlock-the-antitamper-lock"></a>Voor het ontgrendelen van de antitamper vergrendeling
1. In de ruimte in de greep en in een socket, invoegen zorgvuldig de LOCK (een 'tamperproof' T10 draai die Microsoft opgegeven). 
   
   > [!NOTE]
   > Als de antitamper vergrendeling is geactiveerd, is de rode indicator is zichtbaar in de ruimte.
   > 
   > 
   
    ![Vergrendelde schijfstation](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Afbeelding 1** antivirusprogramma draagbare vergrendeling is ingeschakeld
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Indicator opening |
   | 2 |Antitamper vergrendelen |
2. De sleutel in een Linksomdraaiend richting draaien totdat de rode indicator niet zichtbaar in de opening boven de sleutel is.
3. Verwijder de sleutel.
   
    ![Niet-vergrendelde schijfstation](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Afbeelding 2** ontgrendeld harde schijf
4. De harde schijf kan nu worden verwijderd.

Volg de stappen in omgekeerde volgorde de vergrendeling bezighouden.

## <a name="remove-the-disk-drive"></a>De schijf verwijderen
Uw StorSimple-apparaat ondersteunt een RAID 10-achtige spaties opslagconfiguratie. Dit betekent dat het normaal gesproken met een mislukte schijf SSD-schijf (SSD werken kan) of harde schijf drive (HDD). 

> [!IMPORTANT]
> * Als uw systeem meer dan één niet-werkende schijf heeft, verwijder niet meer dan één SSD of harde schijf uit het systeem op elk punt in tijd. Dit kan leiden tot verlies van gegevens.
> * Zorg ervoor dat u een vervangende SSD plaatsen in een sleuf dat eerder een SSD bevatte. Plaats een vervangende harde schijf op dezelfde manier in een sleuf dat eerder een harde schijf bevatte.
> * Sleuven zijn in de klassieke Azure portal, genummerd van 0 – 11. Daarom als de portal ziet u een schijf in de sleuf 2 is mislukt, op het apparaat, zoekt de defecte schijf in de derde sleuf vanaf de bovenkant links.
> 
> 

Schijven worden verwijderd en vervangen terwijl het systeem werkt.

#### <a name="to-remove-a-drive"></a>Een station verwijderen
1. Om te identificeren van de defecte schijf, in de klassieke Azure portal, gaat u naar **apparaten** > **onderhoud** > **hardwarestatus**. Omdat een schijf kan niet in de primaire behuizing en/of een behuizing EBOD (als u van een model 8600 gebruikmaakt), kijk naar de status van de schijven onder **gedeelde onderdelen** en klikt u onder **EBOD behuizing gedeelde onderdelen**. Een niet-werkende schijf in de behuizing worden weergegeven met een rode status.
2. Zoek de stations vooraan in de primaire behuizing of de EBOD behuizing. 
3. Als de schijf ontgrendeld is, gaat u verder met de volgende stap. Als de schijf is vergrendeld, ontgrendelt u het via de volgende procedure in [werking stellen van de vergrendeling antitamper](#disengage-the-antitamper-lock).
4. Druk op de zwarte vergrendeling op de schijf carrier module en pull-de ingang van station carrier af en opslag van het begin van het chassis. 
   
    ![Schijfstation ingang vrijgeven](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Afbeelding 3** vrijgave van de ingang station
5. Wanneer de ingang van de provider station volledig is uitgebreid, schuif de drager station buiten het chassis. 
   
    ![Schijf buiten het schijfstation Verschuivend](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Afbeelding 4** Verschuivend het schijfstation buiten de provider

## <a name="install-the-replacement-disk-drive"></a>De vervangende schijf installeren
Wanneer een station in uw StorSimple-apparaat is mislukt en u deze hebt verwijderd, volgt u deze procedure wilt vervangen door een nieuwe schijf.

#### <a name="to-insert-a-drive"></a>Invoegen van een station
1. Zorg ervoor dat de ingang van de provider station volledig is uitgebreid, zoals wordt weergegeven in de volgende afbeelding.
   
    ![Harde schijf met de ingang uitgebreid](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Afbeelding 5** station met de ingang uitgebreid
2. De provider station de schuifregelaar helemaal in het chassis. 
   
    ![Schijf naar schijf carrier Verschuivend](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Afbeelding 6** Verschuivend de station-provider in het chassis
3. Met de drager station is geplaatst, sluit u de ingang van station carrier terwijl u de provider station in het chassis push totdat de schijf carrier ingang in een vergrendelde positie uitlijnen.
4. Gebruik de LOCK die werd geleverd door Microsoft (tamperproof Torx draai) de ingang carrier op hun plaats door in te schakelen de installatie van de vergrendeling inschakelen van een kwartaal rechtsom.
5. Controleer of de vervanging is geslaagd en door toegang tot de klassieke Azure portal en het navigeren naar het station functioneert **onderhoud** > **hardwarestatus**. Onder **gedeelde onderdelen** of **EBOD behuizing gedeelde onderdelen**, de stationsstatus moet groen, die aangeeft of dit in orde is.
   
   > [!NOTE]
   > Het duurt enkele uren groen inschakelen na de vervanging van de schijfstatus van de.
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).

