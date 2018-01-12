---
title: Vervang accu op Microsoft Azure StorSimple 8000 series apparaat | Microsoft Docs
description: Beschrijft hoe verwijderen, vervangen en onderhouden van de module voor back-up batterij op uw StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: f8071cde67017ff031418f0d97da15a618c4969b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Vervang de module voor back-up batterij op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht
De primaire behuizing stroom en koeling Module (PCM) op uw Microsoft Azure StorSimple-apparaat heeft een extra batterij pack. Dit pakket biedt power zodat het StorSimple-apparaat kunt gegevens opslaan als er verlies van Netstroom zijn aangesloten op de primaire behuizing. Dit pack accu wordt aangeduid als de *back-up batterij module*. De module back-up batterij bestaat alleen voor de primaire behuizing in uw StorSimple-apparaat (de behuizing EBOD bevat geen een back-up batterij module).

Deze zelfstudie wordt uitgelegd hoe:

* De back-up batterij-module verwijderen
* Een nieuwe back-up batterij-module installeren
* De module back-up batterij onderhouden

> [!IMPORTANT]
> Voordat verwijderen en het vervangen van een back-up batterij-module, lees de veiligheidsinformatie in de [Inleiding tot StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-backup-battery-module"></a>De back-up batterij-module verwijderen
De back-up batterij-module voor uw StorSimple-apparaat is een field-replaceable unit. Voordat deze wordt geïnstalleerd in de PCM, moet de batterij-module worden opgeslagen in de originele verpakking. De volgende stappen uitvoeren om te verwijderen van de back-up batterij.

#### <a name="to-remove-the-backup-battery-module"></a>De back-up batterij-module verwijderen
1. Ga naar de blade van uw StorSimple-apparaat Manager-service in de Azure-portal. Ga naar **apparaten** en selecteer vervolgens het apparaat in de lijst met apparaten. Navigeer naar **Monitor** > **Hardware health**. Onder **gedeelde onderdelen**, de status van de accu bekijken.
2. Identificeer de PCM waarin de batterij is mislukt. Afbeelding 1 ziet u de achterkant van het StorSimple-apparaat.
   
    ![Backplane apparaat primaire behuizing modules](./media/storsimple-battery-replacement/IC740994.png)
   
    **Afbeelding 1** achterzijde van het primaire apparaat van PCM en controller modules
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
   
    Door de waarde 3 in de afbeelding 2 ziet de bewaking indicator geleid op PCM 0 die overeenkomt met **accu veroorzaakt** moet worden belicht.
   
    ![Backplane van apparaat PCM-bewaking indicatielampjes](./media/storsimple-battery-replacement/IC740992.png)
   
    **Afbeelding 2** Back van PCM met de bewaking LED
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Fout in Echtheidsvoorwaarde power |
   | 2 |Ventilator mislukt |
   | 3 |Fout met betrekking tot accu |
   | 4 |PCM OK |
   | 5 |DC-stroomstoring |
   | 6 |Accu in orde |
3. Volg de stappen in voor het verwijderen van de PCM met een mislukte batterij [verwijderen van een PCM](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm).
4. Met de PCM verwijderd lift-pull-tot de batterij verwijderen en de ingang van de module accu omhoog draaien zoals aangegeven in de volgende afbeelding.
   
    ![Verwijderen van de batterij van PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    **Afbeelding 3** verwijderen van de batterij van de PCM
5. De module in het veld replaceable unit verpakking plaatsen.
6. De defecte eenheid terug naar Microsoft voor de juiste onderhoud en verwerken.

## <a name="install-a-new-backup-battery-module"></a>Een nieuwe back-up batterij-module installeren
Voer de volgende stappen uit voor het installeren van de vervangende accu module in de PCM in de primaire behuizing van uw StorSimple-apparaat.

#### <a name="to-install-the-battery-module"></a>De batterij-module installeren
1. De back-up batterij-module in de juiste richting in de PCM plaatsen.
2. Houd de ingang van de module accu helemaal aan de connector.
3. De PCM in de primaire behuizing vervangen door de richtlijnen in [vervangen van een stroom en koeling Module op uw StorSimple-apparaat](storsimple-8000-power-cooling-module-replacement.md).
4. Nadat de vervanging voltooid is, gaat u naar het apparaat en Ga naar **Monitor** > **Hardware health** in de Azure portal. Controleer de status van de batterij om ervoor te zorgen dat de installatie geslaagd is. Groene status geeft aan of de batterij in orde is.

## <a name="maintain-the-backup-battery-module"></a>De module back-up batterij onderhouden
In uw StorSimple-apparaat, wordt in de module back-up batterij power naar de controller biedt tijdens een power verlies-gebeurtenis. Kunt u het StorSimple-apparaat op te slaan van kritieke gegevens voorafgaand aan de op een gecontroleerde manier wordt afgesloten. Het systeem kan twee opeenvolgende verlies gebeurtenissen verwerken met twee volledig opgeladen batterijen in de PCMs.

In de Azure portal, de **Hardware health** onder de **Monitor** blade geeft aan of de batterij is defect of de einde van de levensduur nadert. De status van de accu wordt aangegeven door **accu in PCM 0** of **accu in PCM 1** onder **gedeelde onderdelen**. Deze blade ziet een **GEDEGRADEERD** staat zijn voor het einde van de levensduur nadert, en **mislukt** voor einde van de levenscyclus bereikt.

> [!NOTE]
> De batterij kan rapporteren **mislukt** gewoon moet kunnen worden gebracht.


Als de **GEDEGRADEERD** status wordt weergegeven, wordt aangeraden de volgende loop van de actie:

* Het systeem kan zich recente stroomuitval of de batterijen momenteel periodiek onderhoud kunnen worden uitgevoerd. Houd rekening met het systeem voor 12 uur voordat u doorgaat.
  
  * Als de status nog steeds is **GEDEGRADEERD** na 12 uur van continue verbinding met de AC met de domeincontrollers en PCMs uitgevoerd inschakelen, klikt u vervolgens de batterij moet worden vervangen. Neem [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor een module van de back-up batterij vervanging.
  * Als de status na 12 uur OK wordt, de batterij operationeel is en deze alleen kosten met zich mee onderhoud nodig.
* Als er een bijbehorende verlies van netstroom niet is en de PCM is ingeschakeld en aangesloten op netstroom, moet de batterij worden vervangen. [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) rangschikken van een module van de back-up batterij vervanging.

> [!IMPORTANT]
> Verwijderen van de mislukte accu volgens nationale en regionale voorschriften.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

