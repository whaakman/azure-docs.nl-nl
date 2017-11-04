---
title: Vervang een PCM op uw StorSimple 8000 series apparaat | Microsoft Docs
description: Legt uit hoe verwijdert en vervangt u de stroom en koeling Module (PCM) op uw StorSimple-apparaat
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
ms.openlocfilehash: 7d181e6e434c998573dbea4b541cfacf7a28ee66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Vervangen van een stroom en koeling Module op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
De stroom en koeling Module (PCM) in uw Microsoft Azure StorSimple-apparaat bestaat uit een voeding en koeling van ventilatoren die worden beheerd via de primaire en EBOD behuizingen. Er is slechts één model van PCM die is gecertificeerd voor elke behuizing. De primaire behuizing is gecertificeerd voor een 764 W PCM en de behuizing EBOD is gecertificeerd voor een 580 W PCM. Hoewel de PCMs voor de primaire behuizing en de behuizing EBOD verschillend zijn, is de vervangingsprocedure is vrijwel identiek.

Deze zelfstudie wordt uitgelegd hoe:

* Een PCM verwijderen
* Een vervangende PCM installeren

> [!IMPORTANT]
> Voordat verwijderen en een PCM vervangen Lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).


## <a name="before-you-replace-a-pcm"></a>Voordat u een PCM vervangen
Let op de volgende belangrijke problemen voordat u uw PCM vervangt:

* Als de voeding van de PCM mislukt, laat de defecte module geïnstalleerd, maar het netsnoer verwijderen. De ventilator blijven power ontvangen van de behuizing en doorgaan met het juiste koeling bieden. Als de ventilator mislukt, wordt de PCM moet onmiddellijk worden vervangen.
* Voordat u de PCM verwijdert, Verbreek de verbinding met de kracht van de PCM door het uitschakelen van de belangrijkste switch (indien aanwezig) of door het netsnoer fysiek verwijderen. Dit biedt een waarschuwing op uw systeem afsluiten power dreigt.
* Zorg ervoor dat de andere PCM functionele voor voortdurende systeembewerkingen voordat de defecte PCM worden vervangen. Een defecte PCM moet worden vervangen door een volledig operationeel PCM zo snel mogelijk.
* Vervanging van PCM-module duurt slechts enkele minuten duren, maar deze moet worden voltooid binnen 10 minuten van het verwijderen van de mislukte PCM om te voorkomen dat oververhitting.
* Houd er rekening mee vervanging 764 W PCM modules verzonden van de fabriek hebben niet de back-up batterij-module. U moet de batterij verwijderen uit uw defecte PCM en plaatst u deze in de module vervanging voordat de vervangende worden uitgevoerd. Zie voor meer informatie hoe [verwijderen en voeg een back-up batterij module](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>Een PCM verwijderen
Volg deze instructies wanneer u klaar bent voor het verwijderen van een stroom en koeling Module (PCM) van uw Microsoft Azure StorSimple-apparaat.

> [!NOTE]
> Voordat u uw PCM verwijdert, moet u controleren of u een juiste vervangende (764 W voor de primaire behuizing) of 580 W voor de behuizing EBOD hebben.

#### <a name="to-remove-a-pcm"></a>Een PCM verwijderen
1. Klik in de klassieke Azure-portal op **instellingen > Monitor > Hardware health**. Controleer de status van de PCM-componenten onder **gedeelde onderdelen** geven aan welke PCM is mislukt:
   
   * Als een voeding in PCM 0 is mislukt, wordt de status van **Power Supply in PCM 0** rood.
   * Als een voeding in PCM-1 is mislukt, wordt de status van **Power Supply in PCM 1** rood.
   * Als de ventilator in PCM-1 is mislukt, wordt de status van een **koeling van 0 voor PCM 0** of **koeling 1 voor PCM 0** rood.
2. Zoek de mislukte PCM op de achterkant van de primaire behuizing. Als u een model 8600 uitvoert, moet u de primaire behuizing vinden door het systeem eenheid id-nummer op het voorpaneel LED-scherm weergegeven. De standaardwaarde eenheid-ID weergegeven op de primaire behuizing is **00**, terwijl de standaardwaarde eenheid-ID weergegeven op de behuizing EBOD **01**. Het volgende diagram en de volgende tabel wordt uitgelegd het voorpaneel van de weergave LED.
   
    ![Systeem-ID op voorpaneel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Afbeelding 1** voorzijde deelvenster van het apparaat  
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Knop Dempen |
   | 2 |Inschakelen van het systeem |
   | 3 |Module-fout |
   | 4 |Logische-fout |
   | 5 |Eenheid-ID weergeven |
3. De bewaking LED aan de achterzijde van de primaire behuizing kan ook worden gebruikt voor het identificeren van de defecte PCM. Zie het volgende diagram en tabel om te begrijpen hoe de LED's gebruiken om te vinden van de defecte PCM. Bijvoorbeeld, als de LED die overeenkomt met de **ventilator mislukken** is belicht, de ventilator is mislukt. Op dezelfde manier als de LED die overeenkomt met **AC mislukken** is belicht, de voeding is mislukt. 
   
    ![Backplane van apparaat PCM bewaking LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Afbeelding 2** Back van PCM met LED
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Fout in Echtheidsvoorwaarde power |
   | 2 |Ventilator mislukt |
   | 3 |Fout met betrekking tot accu |
   | 4 |PCM OK |
   | 5 |DC-stroomstoring |
   | 6 |Accu in orde |
4. Raadpleeg het volgende diagram van de achterkant van het StorSimple-apparaat Zoek de mislukte PCM-module. PCM 0 is aan de linkerkant en PCM-1 is aan de rechterkant. De volgende tabel wordt uitgelegd dat de modules.
   
     ![Backplane apparaat primaire behuizing modules](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Afbeelding 3** achterzijde van het apparaat met de invoegtoepassing modules 
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
5. Schakel de defecte PCM en Verbreek de verbinding met het netsnoer levering. U kunt nu de PCM verwijderen.
6. De vergrendeling en de kant van de greep PCM tussen uw miniatuur en wijsvinger te vatten en verondersteld ze samen als u wilt openen van de greep.
   
    ![De PCM-ingang openen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Afbeelding 4** de PCM-ingang openen
7. De ingang houvastOpening en verwijder de PCM.
   
    ![PCM-apparaat verwijderen](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Afbeelding 5** de PCM verwijderen

## <a name="install-a-replacement-pcm"></a>Een vervangende PCM installeren
Volg deze instructies voor het installeren van een PCM in uw StorSimple-apparaat. Zorg ervoor dat u de back-up batterij module voordat u installeert de vervangende PCM (geldt voor alleen 764 W PCMs) hebt geplaatst. Zie voor meer informatie hoe [verwijderen en voeg een back-up batterij module](storsimple-8000-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Voor het installeren van een PCM
1. Controleer of u de juiste vervangende PCM voor deze bijlage. De primaire behuizing moet een 764 W PCM en de behuizing EBOD moet een 580 W PCM. U moet niet proberen om de 580 W PCM in de primaire behuizing of de 764 W PCM in de behuizing EBOD te gebruiken. De volgende afbeelding toont waar u deze informatie op het label dat wordt aangebracht op de PCM identificeren.
   
    ![Apparaat PCM-Label](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Afbeelding 6** PCM-label
2. Controleer op beschadiging van de behuizing, met bijzondere aandacht voor de connectors. 
   
   > [!NOTE]
   > **Installeer de module niet als een verbindingslijn pincodes verbogen zijn.**
   > 
   > 
3. Met de PCM-ingang op positie open de module de schuifregelaar in de behuizing.
   
    ![Installatie van PCM-apparaat](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Afbeelding 7** de PCM installeren
4. De PCM-ingang handmatig af te sluiten. U moet een klik horen, zoals de vergrendeling ingang stelt.
   
   > [!NOTE]
   > Om ervoor te zorgen dat de pincodes connector hebt ingeschakeld, kunt u voorzichtig tug op de greep zonder de vergrendeling vrij te geven. Als de PCM dia uit, betekent dit dat de vergrendeling is gesloten voordat de connectors die betrokken zijn.
   
5. De power-kabels aansluiten op de stroombron en op de PCM.
6. Beveilig de stam vrijstelling bales.
7. De PCM inschakelen.
8. Controleren of de vervanging geslaagd is: in de Azure portal van uw StorSimple-apparaat Manager-service, gaat u naar het apparaat en klik vervolgens op **instellingen > Monitor > Hardware health**. Onder de **gedeelde onderdelen**, de status van de PCM moeten groene.
   
   > [!NOTE]
   > Duurt enkele minuten duren voordat de vervangende PCM volledig geïnitialiseerd.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

