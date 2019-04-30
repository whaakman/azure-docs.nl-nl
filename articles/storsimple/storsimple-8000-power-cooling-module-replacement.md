---
title: Vervang een PCM op uw StorSimple 8000-apparaat | Microsoft Docs
description: Wordt uitgelegd hoe u verwijdert en vervangt u de kracht en de koeling-Module (PCM) op uw StorSimple-apparaat
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
ms.openlocfilehash: 42561570e24aec5edd33248ef1738e53175e480e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60632415"
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Vervangen van een stroom en koeling Module op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
De stroom en koeling-Module (PCM) in uw Microsoft Azure StorSimple-apparaat bestaat uit een voeding en koeling fans die worden beheerd via de primaire en EBOD-behuizingen. Er is slechts één model van PCM die is gecertificeerd voor elke behuizing. De primaire behuizing is gecertificeerd voor een 764 W PCM en de EBOD-behuizing is gecertificeerd voor een 580 W PCM. Hoewel de PCMs voor de primaire behuizing en de behuizing EBOD verschillend zijn, is de vervangingsprocedure is vrijwel identiek.

In deze zelfstudie wordt het volgende uitgelegd:

* Een PCM verwijderen
* Een vervangende PCM installeren

> [!IMPORTANT]
> Voordat verwijderen en een PCM vervangen Lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).


## <a name="before-you-replace-a-pcm"></a>Voordat u een PCM vervangen
Houd rekening met de volgende belangrijke problemen voordat u uw PCM vervangen:

* Als de voeding van de PCM mislukt, laat u de defecte-module geïnstalleerd, maar het netsnoer verwijderen. De ventilator blijft power ontvangen van de behuizing en blijven leveren juiste koeling. Als de ventilator mislukt, wordt de PCM moet onmiddellijk worden vervangen.
* Voordat u verwijdert de PCM, verbreekt u de kracht van de PCM door het uitschakelen van de belangrijkste switch (indien aanwezig) of door het netsnoer fysiek te verwijderen. Dit biedt een waarschuwing weergegeven op uw systeem dat een afsluiten power dreigende is.
* Zorg ervoor dat de andere PCM functionele voor voortdurende Systeembewerking voordat de defecte PCM vervangen. Een defecte PCM moet worden vervangen door een volledig operationeel PCM zo snel mogelijk.
* PCM-module vervanging duurt slechts enkele minuten duren, maar deze moet worden voltooid binnen 10 minuten van het verwijderen van de PCM defect is om te voorkomen dat oververhitting.
* Houd er rekening mee dat de vervangende 764 W PCM modules die standaard in de fabriek geen de module back-upbatterij bevatten. U moet voor het verwijderen van de accu van uw PCM defect en plaatst u deze in de module vervangen voordat u de vervanging uitvoert. Zie voor meer informatie over het [verwijderen en voeg een back-upbatterij-module](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>Een PCM verwijderen
Volg deze instructies wanneer u klaar bent voor het verwijderen van een stroom en koeling-Module (PCM) van uw Microsoft Azure StorSimple-apparaat.

> [!NOTE]
> Voordat u uw PCM verwijdert, moet u controleren of u een juiste vervangende (764 W voor de primaire behuizing) of 580 W voor de behuizing EBOD hebben.

#### <a name="to-remove-a-pcm"></a>Een PCM verwijderen
1. Klik in de klassieke Azure portal, op **instellingen > Monitor > hardwarestatus**. Controleer de status van de PCM-onderdelen onder **gedeelde onderdelen** om te bepalen welke PCM is mislukt:
   
   * Als een voeding in PCM 0 is mislukt, wordt de status van **voeding in PCM 0** rood.
   * Als een voeding in PCM 1 is mislukt, wordt de status van **voeding in PCM 1** rood.
   * Als de ventilator in PCM 1 is mislukt, wordt de status van een van beide **koeling 0 voor PCM 0** of **koeling 1 voor PCM 0** rood.
2. Ga naar de PCM defect is op de achterkant van de primaire behuizing. Als u een model 8600 uitvoert, moet u de primaire behuizing identificeren door te kijken op het systeem eenheid id-nummer op het voorpaneel LED-scherm weergegeven. De standaard-eenheid-ID weergegeven op de primaire behuizing is **00**, terwijl de standaard-eenheid-ID weergegeven op de behuizing EBOD **01**. Het volgende diagram en de volgende tabel wordt uitgelegd het voorpaneel van de weergave LED.
   
    ![Systeem-ID op voorpaneel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Afbeelding 1** Front-deelvenster van het apparaat  
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Knop Dempen |
   | 2 |Inschakelen van het systeem |
   | 3 |Module-fout |
   | 4 |Logische fout |
   | 5 |Eenheid-ID weergeven |
3. De bewakings-LED aan de achterzijde van de primaire behuizing kan ook worden gebruikt voor het identificeren van de PCM defect. Zie het volgende diagram en tabel om te begrijpen hoe de LED's gebruiken om te vinden van de PCM defect. Bijvoorbeeld, als de LED die overeenkomt met de **ventilator mislukken** wordt belicht, de ventilator is mislukt. Op dezelfde manier als de LED die overeenkomt met **AC mislukken** wordt belicht, de voeding is mislukt. 
   
    ![Backplane van apparaat PCM bewakings-LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Afbeelding 2** terug van PCM met LED's
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Fout in Echtheidsvoorwaarde power |
   | 2 |Storing in de ventilator |
   | 3 |Accu fouttolerantie |
   | 4 |PCM OK |
   | 5 |DC-stroomstoring |
   | 6 |Accu in orde |
4. Raadpleeg het volgende diagram van de achtergrond van het StorSimple-apparaat te vinden van de mislukte PCM-module. PCM 0 is aan de linkerkant en PCM 1 is aan de rechterkant. De volgende tabel wordt uitgelegd dat de modules.
   
     ![Backplane van apparaat primaire behuizing-modules](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Afbeelding 3** achterzijde apparaten met Plug-ins 
   
   | Label | Description |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
5. De PCM defect uitschakelen en het netsnoer van de levering verbreken. U kunt nu de PCM verwijderen.
6. De vergrendeling en de kant van de schuifknop PCM tussen uw duim en wijsvinger ingewikkeld en verondersteld dat ze samen om te openen van de greep.
   
    ![De PCM-ingang openen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Afbeelding 4** openen van de PCM-ingang
7. De ingang houvastOpening en verwijder de PCM.
   
    ![PCM-apparaat verwijderen](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Afbeelding 5** de PCM verwijderen

## <a name="install-a-replacement-pcm"></a>Een vervangende PCM installeren
Volg deze instructies voor het installeren van een PCM in uw StorSimple-apparaat. Zorg ervoor dat u de back-upbatterij-module voor de installatie van de vervanging PCM (van toepassing op alleen 764 W PCMs) hebt geplaatst. Zie voor meer informatie over het [verwijderen en voeg een back-upbatterij-module](storsimple-8000-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Voor het installeren van een PCM
1. Controleer of u de juiste vervanging PCM voor deze bijlage. De primaire behuizing moet een 764 W PCM en de behuizing EBOD moet een 580 W PCM. U moet niet proberen om de 580 W PCM in de primaire behuizing of de 764 W PCM in de behuizing EBOD te gebruiken. De volgende afbeelding ziet u waar om deze informatie op het label dat wordt aangebracht op de PCM te identificeren.
   
    ![Apparaat PCM Label](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Afbeelding 6** PCM-label
2. Controleer op beschadiging van de behuizing, met name aandacht voor de connectors. 
   
   > [!NOTE]
   > **Installeer de module niet als een connector pincodes verbogen zijn.**
   > 
   > 
3. Met de PCM-ingang op positie open, schuift u de module in de behuizing.
   
    ![Installeren van het apparaat PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Afbeelding 7** de PCM installeren
4. De PCM-ingang handmatig af te sluiten. U moet één klik van u horen als de vergrendeling ingang praat.
   
   > [!NOTE]
   > Om ervoor te zorgen dat de pincodes connector hebt ingeschakeld, kunt u voorzichtig tug op de greep zonder de vergrendeling vrij te geven. Als de PCM schuift, betekent dit dat de vergrendeling is afgesloten voordat de connectors die zich bezighoudt.
   
5. De power-kabels aansluiten op de voeding en op de PCM.
6. Beveilig de stam vrijstelling bales.
7. De PCM inschakelen.
8. Controleren of de vervanging geslaagd is: in de Azure-portal van uw StorSimple Device Manager-service, gaat u naar uw apparaat en van daaruit naar **instellingen > Monitor > hardwarestatus**. Onder de **gedeelde onderdelen**, de status van de PCM moeten groen.
   
   > [!NOTE]
   > Het duurt een paar minuten voor de vervanging PCM volledig worden geïnitialiseerd.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

