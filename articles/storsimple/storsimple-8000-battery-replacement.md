---
title: Vervang accu op Microsoft Azure StorSimple 8000-apparaat | Microsoft Docs
description: Beschrijft hoe u verwijderen, vervangen en onderhouden van de back-upbatterij-module op uw StorSimple-apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 4ebf3f28d40e0461d140a3fe74fb940720f26db6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60418926"
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Vervang de back-upbatterij-module op uw StorSimple-apparaat

## <a name="overview"></a>Overzicht
De primaire behuizing voeding en koeling-Module (PCM) op uw Microsoft Azure StorSimple-apparaat heeft een extra accu. Dit pakket biedt power zodat het StorSimple-apparaat kunt gegevens opslaan als er verlies van Netstroom zijn aangesloten op de primaire behuizing. Dit pack accu wordt aangeduid als de *back-upbatterij module*. De back-upbatterij module bestaat alleen voor de primaire behuizing in uw StorSimple-apparaat (de behuizing EBOD bevat een back-upbatterij-module).

In deze zelfstudie wordt het volgende uitgelegd:

* De back-upbatterij-module verwijderen
* Een nieuwe back-upbatterij-module installeren
* De module back-upbatterij onderhouden

> [!IMPORTANT]
> Voordat verwijderen en het vervangen van een back-upbatterij-module, lees de veiligheidsinformatie in de [Inleiding tot StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-backup-battery-module"></a>De back-upbatterij-module verwijderen
De back-upbatterij-module voor uw StorSimple-apparaat is een field-replaceable unit. Voordat deze wordt geïnstalleerd in de PCM, moet de batterij-module worden opgeslagen in de oorspronkelijke verpakking. Voer de volgende stappen uit als u wilt verwijderen van de back-upbatterij.

#### <a name="to-remove-the-backup-battery-module"></a>De back-upbatterij-module verwijderen
1. In de Azure-portal, gaat u naar de serviceblade van uw StorSimple Device Manager. Ga naar **apparaten** en selecteer vervolgens het apparaat in de lijst met apparaten. Navigeer naar **Monitor** > **hardwarestatus**. Onder **Shared Components**, kijken naar de status van de accu.
2. Identificeer de PCM waarin de batterij is mislukt. Afbeelding 1 ziet u de achterkant van het StorSimple-apparaat.
   
    ![Backplane van apparaat primaire behuizing-Modules](./media/storsimple-battery-replacement/IC740994.png)
   
    **Afbeelding 1** achterzijde PCM en controller modules met het primaire apparaat
   
   | Label | Description |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
   
    Zoals wordt weergegeven door het cijfer 3 in de afbeelding 2, de controle indicator geleid in PCM 0 die overeenkomt met **accu fouttolerantie** moeten worden belicht.
   
    ![Backplane van apparaat PCM controle-LED 's](./media/storsimple-battery-replacement/IC740992.png)
   
    **Afbeelding 2** terug van PCM met de bewakings-LED
   
   | Label | Description |
   |:--- |:--- |
   | 1 |Fout in Echtheidsvoorwaarde power |
   | 2 |Storing in de ventilator |
   | 3 |Accu fouttolerantie |
   | 4 |PCM OK |
   | 5 |DC-stroomstoring |
   | 6 |Accu in orde |
3. Als u wilt verwijderen van de PCM met een mislukte batterij, volg de stappen in [verwijderen van een PCM](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm).
4. Met de PCM verwijderd, lift ophalen om tot de batterij verwijderen en de ingang van de module accu omhoog draaien zoals aangegeven in de volgende afbeelding.
   
    ![Accu verwijderen in PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    **Afbeelding 3** de accu van de PCM verwijderen
5. Plaats de module in het veld-replaceable unit verpakking.
6. De defecte eenheid retourneren aan Microsoft voor de juiste service- en verwerken.

## <a name="install-a-new-backup-battery-module"></a>Een nieuwe back-upbatterij-module installeren
Voer de volgende stappen uit voor het installeren van de vervangende accu-module in de PCM in de primaire behuizing van uw StorSimple-apparaat.

#### <a name="to-install-the-battery-module"></a>De batterij-module installeren
1. Plaats de back-upbatterij-module in de juiste richting in de PCM.
2. Houd de ingang van de module accu helemaal tot aan de connector seat.
3. De PCM in de primaire behuizing vervangen door de richtlijnen in [vervangen door een stroom en koeling Module op uw StorSimple-apparaat](storsimple-8000-power-cooling-module-replacement.md).
4. Nadat de vervanging voltooid is, gaat u naar uw apparaat en ga vervolgens naar **Monitor** > **hardwarestatus** in Azure portal. Controleer of de status van de batterij om ervoor te zorgen dat de installatie geslaagd is. Groene status geeft aan dat de batterij in orde is.

## <a name="maintain-the-backup-battery-module"></a>De module back-upbatterij onderhouden
In uw StorSimple-apparaat, wordt in de module back-upbatterij power naar de controller biedt tijdens een power-verlies-gebeurtenis. Hierdoor kan het StorSimple-apparaat op te slaan van kritieke gegevens voorafgaand aan het afsluiten van op een gecontroleerde manier. Met twee volledig opgeladen batterijen in de PCMs, kan twee opeenvolgende verlies gebeurtenissen worden verwerkt door het systeem.

In de Azure-portal, de **hardwarestatus** onder de **Monitor** blade geeft aan of de batterij is defect of het einde van de levensduur nadert. De status van de accu wordt aangegeven door **accu in PCM 0** of **accu in PCM 1** onder **Shared Components**. Deze blade ziet u een **GEDEGRADEERD** staat voor het einde van de levensduur nadert, en **mislukt** voor het einde van de levenscyclus is bereikt.

> [!NOTE]
> Kan rapporteren dat de batterij **mislukt** gewoon moet in rekening gebracht.


Als de **GEDEGRADEERD** status wordt weergegeven, wordt aangeraden de volgende cursus van actie:

* Het systeem mogelijk ondervonden recente stroomuitval of de accu's kunnen momenteel periodiek onderhoud worden uitgevoerd. Bekijk het systeem 12 uur voordat u doorgaat.
  
  * Als de status nog steeds is **GEDEGRADEERD** na 12 uur van continue verbinding met het AC met de domeincontrollers en PCMs uitgevoerd inschakelen, klikt u vervolgens de batterij moet worden vervangen. Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor een module van de back-upbatterij vervangen.
  * Als de status OK na 12 uur, de batterij operationeel is en deze alleen een toeslag onderhoud nodig is.
* Als er een bijbehorende verlies van netstroom niet is en de PCM is ingeschakeld en aangesloten op netstroom, moet de batterij worden vervangen. [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) rangschikken van een module van de back-upbatterij vervangen.

> [!IMPORTANT]
> Verwijderen van de mislukte accu op basis van nationale of regionale voorschriften.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

