---
title: Vervangen van een schijf op een apparaat StorSimple 8000-reeks | Microsoft Docs
description: Wordt uitgelegd hoe u een schijf op een primaire behuizing StorSimple of een behuizing EBOD vervangt.
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
ms.date: 8/25/2017
ms.author: alkohli
ms.openlocfilehash: 3d6ef22e4df36996d68194589f43ea0f57def22c
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673919"
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a>Vervang een schijfstation op de StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe u kunt verwijderen en vervangen door een niet-functionerende of mislukte harde schijf op een Microsoft Azure StorSimple-apparaat. Ter vervanging van een schijf, moet u naar:

* De antitamper vergrendeling ontworpen
* De schijf verwijderen
* De vervangende schijf installeren

> [!IMPORTANT]
> Voordat verwijderen en het vervangen van een schijf, lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).
 

## <a name="disengage-the-antitamper-lock"></a>De antitamper vergrendeling ontworpen
Deze procedure wordt uitgelegd hoe de antitamper vergrendelingen op uw StorSimple-apparaat kunnen worden ingeschakeld of uitgeschakeld wanneer u de schijfstations vervangen. De antitamper vergrendelingen zijn aangebracht in het station carrier verwerkt en ze toegankelijk zijn via een kleine lensopening in de sectie vergrendeling van de greep. Schijven worden geleverd met de vergrendelingen ingesteld op de vergrendeling.

#### <a name="to-unlock-the-antitamper-lock"></a>Voor het ontgrendelen van de antitamper vergrendeling
1. In de ruimte in de kunnen worden verwerkt en in een socket, invoegen zorgvuldig de LOCK (een "tamperproof" T10 draai die Microsoft heeft verstrekt). 
   
   Als de antitamper vergrendeling is ingeschakeld, is de rode indicator is zichtbaar in de ruimte.
  
    ![Vergrendelde schijfstation](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Afbeelding 1** tegen onrechtmatige wijzigingen vergrendeling is ingeschakeld
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Indicator scherpstelling |
   | 2 |Antitamper vergrendelen |
2. De sleutel in de richting van een linksom draaien totdat de rode indicator niet weergegeven in de opening boven de sleutel wordt.
3. Verwijder de sleutel.
   
    ![Niet-vergrendelde schijfstation](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Afbeelding 2** ontgrendeld harde schijf
4. De schijf kan nu worden verwijderd.

Volg de stappen in omgekeerde volgorde contact opnemen met de vergrendeling.

## <a name="remove-the-disk-drive"></a>De schijf verwijderen
Een RAID 10-achtige configuratie van opslagruimten biedt ondersteuning voor uw StorSimple-apparaat. Dit betekent dat het normaal gesproken kan werken met een niet-werkende schijf, SSD (solid-state drive), of harde schijf drive (HDD).

> [!IMPORTANT]
> * Als uw systeem meer dan één niet-werkende schijf heeft, verwijder niet meer dan één SSD of HDD uit het systeem op elk gewenst moment in-time. In dat geval kan leiden tot verlies van gegevens.
> * Zorg ervoor dat u een vervangende SSD plaatsen in een sleuf die eerder een SSD. Op dezelfde manier een vervangende harde schijf in een site die eerder deel uitmaakt van een harde schijf plaatsen.
> * In de Azure-portal sleuven, krijgen een nummer tussen 0 en 11. Dus als de portal ziet u dat een schijf in sleuf 2 is mislukt, klikt u op het apparaat, zoek naar de niet-werkende schijf in de derde sleuf uit links bovenin.
> 
> 

Schijven kunnen worden verwijderd en vervangen terwijl het systeem wordt uitgevoerd.

#### <a name="to-remove-a-drive"></a>Om een station te verwijderen
1. Voor het identificeren van de niet-werkende schijf, in de Azure-portal, gaat u naar uw apparaat **instellingen > hardwarestatus**. Omdat een schijf kan niet in de primaire behuizing en/of in een EBOD-behuizing (als u van een model 8600 gebruikmaakt), kijk naar de status van de schijven onder **gedeelde onderdelen** en klikt u onder **gedeelde EBOD-onderdelen**. Een niet-werkende schijf in een van beide behuizing wordt weergegeven met een rode status.
2. Zoek de schijven vooraan in de primaire behuizing of de EBOD-behuizing. 
3. Als de schijf ontgrendeld is, gaat u verder met de volgende stap. Als de schijf is vergrendeld, ontgrendelt u deze via de volgende procedure in [werking stellen de antitamper vergrendeling](#disengage-the-antitamper-lock).
4. Druk op de zwarte vergrendeling op het station provider-module en pull-de ingang van de provider station uit en opgeslagen uit het voorste deel van het chassis.
   
    ![Vrijgeven van schijf-ingang](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Afbeelding 3** vrijgeven van de greep station
5. Wanneer de provider station ingang volledig is uitgebreid, schuift u de provider station buiten het chassis. 
   
    ![Sliding schijf op basis van schijf](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Afbeelding 4** Verschuivend van de schijf uit de provider

## <a name="install-the-replacement-disk-drive"></a>De vervangende schijf installeren
Nadat een station is mislukt in uw StorSimple-apparaat en u deze hebt verwijderd, voert u deze procedure voor het vervangen door een nieuw station.

#### <a name="to-insert-a-drive"></a>Een station invoegen
1. Zorg ervoor dat de provider station ingang volledig is uitgebreid, zoals wordt weergegeven in de volgende afbeelding.
   
    ![Harde schijf met de ingang uitgebreid](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Afbeelding 5** station met de ingang uitgebreid
2. De provider station dia helemaal naar het chassis.
   
    ![Schijf naar schijf carrier schuiven](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Afbeelding 6** schuiven de station-provider in het chassis
3. Met de provider voor station ingevoegd, sluit u de ingang van de provider station terwijl u verdergaat met het pushen van de vervoerder station naar het chassis, totdat de ingang van de provider station in een vergrendelde positie uitlijnen.
4. Gebruik de LOCK die is geleverd door Microsoft (tamperproof Torx draai) voor het beveiligen van de provider-ingang in plaats door in te schakelen de installatie van de vergrendeling per kwartaal inschakelen met de klok mee.
5. Controleer of de vervanging is geslaagd en het station operationeel is. Toegang tot de Azure portal en Ga naar **apparaatinstellingen** > **hardwarestatus**. Onder **gedeelde onderdelen** of **gedeelde EBOD-onderdelen**, de stationsstatus moet groen, waarmee wordt aangegeven dat deze in orde is.

   
   > [!NOTE]
   > Het duurt enkele uren voor de status groen inschakelen na de vervanging.
  
## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

