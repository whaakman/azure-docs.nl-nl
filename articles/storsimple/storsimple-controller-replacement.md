---
title: Vervangen van een StorSimple-apparaat controller | Microsoft Docs
description: Legt uit hoe verwijdert en vervangt u een of beide controller modules op uw StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e25b52b7-60f5-47f3-bffc-6c157d57ab5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/03/2017
ms.author: alkohli
ms.openlocfilehash: 5dd5ffc7c08fcc9263b91ca5ac86de5163f91657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Vervangen van een module van de domeincontroller op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe verwijdert en vervangt u een of beide controller modules in een StorSimple-apparaat. Dit wordt ook de onderliggende logica voor de domeincontroller enkele en dubbele vervanging scenario's beschreven.

> [!NOTE]
> Voordat u een vervangende domeincontroller uitvoert, wordt u aangeraden de firmware van de domeincontroller altijd bij te werken naar de nieuwste versie.
> 
> Om te voorkomen dat schade toebrengen aan uw StorSimple-apparaat, niet verwijdert de controller totdat de LED's worden weergegeven als een van de volgende:
> 
> * Alle lichten zijn uitgeschakeld.
> * LED 3, ![pictogram groen vinkje](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), en ![rode x pictogram](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) knippert, en LED 0 en 7 LED zijn **ON**.
> 
> 

De volgende tabel bevat de ondersteunde controller vervanging scenario's.

| Aanvraag | Vervanging scenario | Toepasselijke procedure |
|:--- |:--- |:--- |
| 1 |Één domeincontroller is een mislukte status, de andere controller is in orde en actief is. |[Vervanging van de domeincontroller enkele](#replace-a-single-controller), waarin wordt beschreven de [logica achter een vervangende één domeincontroller](#single-controller-replacement-logic), evenals de [vervanging stappen](#single-controller-replacement-steps). |
| 2 |Beide domeincontrollers niet hebben doorstaan en moeten worden vervangen. De chassis, schijven en schijfbehuizing zijn in orde. |[Dubbele domeincontroller vervanging](#replace-both-controllers), waarin wordt beschreven de [logica achter een dubbele domeincontroller vervangende](#dual-controller-replacement-logic), evenals de [vervanging stappen](#dual-controller-replacement-steps). |
| 3 |Domeincontrollers van hetzelfde apparaat of van verschillende apparaten worden omgewisseld. De chassis, schijven en schijfbehuizingen zijn in orde. |Een waarschuwingsbericht van sleuf verschil wordt weergegeven. |
| 4 |Er ontbreekt een domeincontroller en de andere controller mislukt. |[Dubbele domeincontroller vervanging](#replace-both-controllers), waarin wordt beschreven de [logica achter een dubbele domeincontroller vervangende](#dual-controller-replacement-logic), evenals de [vervanging stappen](#dual-controller-replacement-steps). |
| 5 |Een of beide domeincontrollers zijn mislukt. U geen toegang tot het apparaat via de seriële console of Windows PowerShell op afstand. |[Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor een handmatige controller vervangingsprocedure. |
| 6 |De controllers hebben een verschillende buildversie die veroorzaakt worden kan:<ul><li>Domeincontrollers hebben een ander software-versie.</li><li>Domeincontrollers hebben een verschillende firmwareversie.</li></ul> |Als de domeincontroller softwareversies verschillen, wordt de logica vervanging detecteert die en de versie van de software op de controller vervangen updates.<br><br>Als de domeincontroller firmware-versies verschillend zijn en de oude firmwareversie is **niet** automatisch kan worden uitgebreid, een waarschuwing wordt weergegeven in de klassieke Azure portal. U moet controleren op updates en installeert de firmware-updates.</br></br>Als de domeincontroller firmware-versies verschillend zijn en de oude firmwareversie van de kan automatisch worden uitgebreid, de logica van de vervangende domeincontroller detecteert DPM dit en nadat de domeincontroller is gestart, de firmware wordt automatisch bijgewerkt. |

U moet een controllermodule verwijderen als deze is mislukt. Een of beide de controller modules kunnen mislukken, hetgeen kan leiden tot een enkele controller vervanging van of dubbele domeincontroller vervanging. Vervanging procedures en de logica achter ze, Zie de volgende:

* [Eén controller vervangen](#replace-a-single-controller)
* [Vervang beide domeincontrollers](#replace-both-controllers)
* [Verwijderen van een domeincontroller](#remove-a-controller)
* [Invoegen van een domeincontroller](#insert-a-controller)
* [De actieve controller op uw apparaat identificeren](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Voordat verwijderen en het vervangen van een domeincontroller, lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Vervangen van één controller
Wanneer een van de twee controllers op het Microsoft Azure StorSimple-apparaat is mislukt, is defect of ontbreekt, moet u één controller vervangen. 

### <a name="single-controller-replacement-logic"></a>Logica van één domeincontroller-vervanging
In een vervanging van één domeincontroller, moet u eerst de mislukte controller verwijderen. (De resterende domeincontrollers in het apparaat is de actieve controller.) Wanneer u de vervangende domeincontroller invoegt, wordt de volgende acties uitgevoerd:

1. De vervangende domeincontroller begint onmiddellijk communiceren met het StorSimple-apparaat.
2. Een momentopname van de virtuele harde schijf (VHD) voor de actieve controller is gekopieerd op de controller vervanging.
3. De momentopname wordt gewijzigd zodat wanneer de vervangende domeincontroller wordt gestart vanaf deze VHD, deze wordt herkend als een stand-by-controller.
4. Wanneer de wijzigingen voltooid zijn, wordt de vervangende domeincontroller wordt gestart als de stand-by-controller.
5. Wanneer beide domeincontrollers worden uitgevoerd, wordt het cluster online komt.

### <a name="single-controller-replacement-steps"></a>Stappen voor één domeincontroller-vervanging
De volgende stappen uitvoeren als een van de domeincontrollers in uw Microsoft Azure StorSimple-apparaat is mislukt. (De andere controller moet actief en wordt uitgevoerd. Als beide domeincontrollers mislukt of er problemen optreden, gaat u naar [dubbele domeincontroller vervanging stappen](#dual-controller-replacement-steps).)

> [!NOTE]
> Het kan 30 – 45 minuten duren voordat de domeincontroller opnieuw opstarten en volledig herstellen vanuit de procedure van één domeincontroller vervanging. De totale tijd voor de gehele procedure, is met inbegrip van de kabels verbonden ongeveer twee uur.
> 
> 

#### <a name="to-remove-a-single-failed-controller-module"></a>Een enkele mislukte controller-module verwijderen
1. Klik in de Azure classic portal, gaat u naar de StorSimple Manager-service op de **apparaten** tabblad en klik vervolgens op de naam van het apparaat dat u wilt bewaken.
2. Ga naar **onderhoud > hardwarestatus**. De status van Controller 0 of 1 van de domeincontroller moet rood, wat aangeeft dat een fout.
   
   > [!NOTE]
   > De mislukte controller in een vervanging van één domeincontroller is altijd een stand-by-controller.
   > 
   > 
3. Afbeelding 1 en de volgende tabel gebruiken om te vinden van de mislukte controllermodule.  
   
    ![Backplane apparaat primaire behuizing modules](./media/storsimple-controller-replacement/IC740994.png)
   
    **Afbeelding 1** Back van StorSimple-apparaat
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
4. Verwijderen de verbonden netwerkkabels vanaf de gegevenspoorten op de domeincontroller is mislukt. Als u een 8600 model gebruikt, moet u ook de SAS-kabels die verbinding maken met de domeincontroller de domeincontroller EBOD verwijderen.
5. Volg de stappen in [verwijderen van een domeincontroller](#remove-a-controller) verwijderen van de domeincontroller is mislukt. 
6. Installeer de factory-vervanging in dezelfde sleuf van waaruit de mislukte controller is verwijderd. Dit activeert de logica van één domeincontroller vervanging. Zie voor meer informatie [één domeincontroller vervanging logica](#single-controller-replacement-logic).
7. Terwijl de logica van één domeincontroller vervanging op de achtergrond verloopt, sluit u de kabels. Verbinding maken van de kabels los precies dezelfde manier dat ze zijn verbonden voordat de vervangende zorgt.
8. Nadat de domeincontroller opnieuw is opgestart, controleert u de **Controller status** en de **Cluster status** in de klassieke Azure portal om te controleren of de controller terug naar de status in orde is en zich in standby-modus.

> [!NOTE]
> Als u het apparaat via de seriële console bewaakt, ziet u mogelijk meerdere opnieuw wordt opgestart tijdens het herstellen van de domeincontroller van de vervangingsprocedure. Wanneer de seriële console-menu wordt weergegeven, weet u dat de vervanging voltooid is. Als u het menu binnen twee uur na het starten van de vervangende domeincontroller niet wordt weergegeven, neemt u [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).
>
> Vanaf Update 4, kunt u ook gebruiken de cmdlet `Get-HCSControllerReplacementStatus` in de Windows PowerShell-interface van het apparaat voor het bewaken van de status van het proces van de vervangende domeincontroller.
> 

## <a name="replace-both-controllers"></a>Vervang beide domeincontrollers
Wanneer beide domeincontrollers op het Microsoft Azure StorSimple-apparaat is mislukt, zijn defect of ontbreekt, moet u beide domeincontrollers te vervangen. 

### <a name="dual-controller-replacement-logic"></a>Dubbele domeincontroller vervanging logica
In een vervanging dubbele domeincontroller verwijdert u beide mislukte domeincontrollers en voeg vervolgens vervangingen. Als de twee vervanging controllers wordt geplaatst, wordt de volgende acties uitgevoerd:

1. De vervangende domeincontroller in sleuf 0 controleert het volgende:
   
   1. Is het huidige versies van de firmware en software gebruikt?
   2. Er is een onderdeel van het cluster?
   3. De peer-controller actief is en is het geclusterd?
      
      Als geen van deze voorwaarden voldaan wordt, de domeincontroller zoekt naar de meest recente dagelijkse back-up (zich in de **nonDOMstorage** op station S). De meest recente momentopname van de VHD de controller opgehaald uit de back-up.
2. De controller in sleuf 0 wordt de momentopname van de installatiekopie zelf.
3. Ondertussen, wacht de controller in sleuf 1 voor controller 0 voltooien van de installatiekopie en starten.
4. Nadat de controller 0 wordt gestart, detecteert de controller 1 het cluster gemaakt door de netwerkcontroller 0, die de logica van één domeincontroller vervanging activeert. Zie voor meer informatie [één domeincontroller vervanging logica](#single-controller-replacement-logic).
5. Daarna beide domeincontrollers wordt uitgevoerd en het cluster online komen.

> [!IMPORTANT]
> Na een vervangende dubbele domeincontroller nadat het StorSimple-apparaat is geconfigureerd, is het essentieel dat u rekening houden met een handmatige back-up van het apparaat. Dagelijkse apparaat configuratie back-ups worden niet geactiveerd totdat na 24 uur zijn verstreken. Werken met [Microsoft Support](storsimple-contact-microsoft-support.md) waarmee een handmatige back-up van uw apparaat.
> 
> 

### <a name="dual-controller-replacement-steps"></a>Dubbele domeincontroller vervanging stappen
Deze werkstroom is vereist wanneer beide van de domeincontrollers in uw Microsoft Azure StorSimple-apparaat is mislukt. Dit kan gebeuren in een datacenter waarin de koeling systeem werkt niet, en als gevolg hiervan beide domeincontrollers binnen een korte periode mislukken. Afhankelijk van of in- of uitschakelen op het StorSimple-apparaat is ingeschakeld en of u gebruikmaakt van een 8600 of een 8100-model, is een andere set stappen vereist.

> [!IMPORTANT]
> Het kan tot 1 uur voor de controller te starten en volledig herstellen vanuit een dubbele domeincontroller vervangingsprocedure 45 minuten duren. De totale tijd voor de gehele procedure, is met inbegrip van de kabels koppelen ongeveer 2,5 uur.
> 
> 

#### <a name="to-replace-both-controller-modules"></a>Beide modules controller vervangen
1. Als het apparaat is uitgeschakeld, wordt deze stap overslaan en doorgaan met de volgende stap. Als het apparaat is ingeschakeld, schakelt u het apparaat uit.
   
   1. Als u van een 8600-model gebruikmaakt, de primaire behuizing eerste uitschakelen en schakelt u de behuizing EBOD vervolgens.
   2. Wacht totdat het apparaat volledig is afgesloten. Alle LED's aan de achterzijde van het apparaat worden uitgeschakeld.
2. Verwijder de netwerkkabels die zijn verbonden met de gegevenspoorten. Als u een 8600 model gebruikt, moet u ook de SAS-kabels die verbinding maken met de primaire behuizing de behuizing EBOD verwijderen.
3. Beide domeincontrollers van het StorSimple-apparaat verwijderen. Zie voor meer informatie [verwijderen van een domeincontroller](#remove-a-controller).
4. Plaatst u eerst de factory vervanging voor Controller 0 en Controller 1 plaats. Zie voor meer informatie [invoegen van een domeincontroller](#insert-a-controller). Dit activeert de logica van de vervangende dubbele domeincontroller. Zie voor meer informatie [dubbele domeincontroller vervanging logica](#dual-controller-replacement-logic).
5. Terwijl de logica van de vervangende domeincontroller op de achtergrond verloopt, sluit u de kabels. Verbinding maken van de kabels los precies dezelfde manier dat ze zijn verbonden voordat de vervangende zorgt. Raadpleeg de gedetailleerde instructies voor het model in de kabel uw apparaat-sectie van [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
6. Schakel op het StorSimple-apparaat. Als u een 8600-model:
   
   1. Zorg ervoor dat de behuizing EBOD eerste is ingeschakeld.
   2. Wacht totdat de behuizing EBOD wordt uitgevoerd.
   3. Schakel op de primaire behuizing.
   4. Nadat de eerste domeincontroller opnieuw is opgestart en in een foutloze toestand bevindt, wordt het systeem worden uitgevoerd.
      
      > [!NOTE]
      > Als u het apparaat via de seriële console bewaakt, ziet u mogelijk meerdere opnieuw wordt opgestart tijdens het herstellen van de domeincontroller van de vervangingsprocedure. Wanneer de seriële console-menu wordt weergegeven, weet u dat de vervanging voltooid is. Als u het menu binnen 2,5 uur na het starten van de vervangende domeincontroller niet wordt weergegeven, neemt u [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).
      > 
      > 

## <a name="remove-a-controller"></a>Verwijderen van een domeincontroller
Gebruik de volgende procedure een defecte controllermodule verwijderen uit uw StorSimple-apparaat.

> [!NOTE]
> De volgende illustraties worden voor controller 0. Voor 1-controller, zou deze worden omgekeerd.
> 
> 

#### <a name="to-remove-a-controller-module"></a>Een controllermodule verwijderen
1. De module vergrendeling tussen uw miniatuur en wijsvinger te vatten.
2. Voorzichtig verondersteld uw miniatuur en wijsvinger samen voor vrijgave van de vergrendeling van de domeincontroller.
   
    ![Controller vergrendeling vrijgeven](./media/storsimple-controller-replacement/IC741047.png)
   
    **Afbeelding 2** vrijgeven controller vergrendeling
3. De vergrendeling als een ingang naar de domeincontroller buiten het chassis dia gebruiken.
   
    ![Verschuivend domeincontroller buiten het chassis](./media/storsimple-controller-replacement/IC741048.png)
   
    **Afbeelding 3** Verschuivend van de domeincontroller buiten het chassis

## <a name="insert-a-controller"></a>Invoegen van een domeincontroller
Gebruik de volgende procedure voor het installeren van een module factory opgegeven domeincontroller nadat u een defecte module verwijderd van uw StorSimple-apparaat.

#### <a name="to-install-a-controller-module"></a>Een module van de domeincontroller te installeren
1. Controleer of er een beschadiging van de interface-connectors is. Installeer de module niet als een van de connector pincodes zijn beschadigd of verbogen.
2. Schuif de module domeincontroller in het chassis terwijl de vergrendeling is volledig vrijgegeven. 
   
    ![Controller Verschuivend in chassis](./media/storsimple-controller-replacement/IC741053.png)
   
    **Afbeelding 4** schuifregelaar domeincontroller in het chassis
3. Met de domeincontroller-module ingevoegd, gaan de vergrendeling sluiten terwijl u de module domeincontroller in het chassis push. De vergrendeling wordt uitgevoerd om te leiden van de domeincontroller naar de juiste plaats.
   
    ![Controller vergrendeling sluiten](./media/storsimple-controller-replacement/IC741054.png)
   
    **Afbeelding 5** sluiten van de domeincontroller-vergrendeling
4. Wanneer de vergrendeling op de plaats waar uitlijnen voltooid. De **OK** LED worden nu op.  
   
   > [!NOTE]
   > Het kan maximaal 5 minuten duren voordat de controller en de LED activeren.
   > 
   > 
5. Om te bevestigen dat de vervanging geslaagd, in de klassieke Azure portal is, gaat u naar **apparaten** > **onderhoud** > **hardwarestatus**, en Zorg ervoor dat zowel controller 0 en controller 1 in orde (status groen is).

## <a name="identify-the-active-controller-on-your-device"></a>De actieve controller op uw apparaat identificeren
Er zijn velerlei situaties, zoals eerst apparaat registratie of controller vervangen, moet u de actieve controller op een StorSimple-apparaat niet vinden. De actieve controller verwerkt alle de firmware en netwerken schijfbewerkingen. U kunt een van de volgende methoden gebruiken voor het identificeren van de actieve controller:

* [Gebruik de klassieke Azure portal om te identificeren van de actieve controller](#use-the-azure-classic-portal-to-identify-the-active-controller)
* [Windows PowerShell voor StorSimple gebruiken voor het identificeren van de actieve controller](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Controleer het fysieke apparaat te identificeren van de actieve controller](#check-the-physical-device-to-identify-the-active-controller)

Elk van deze procedures is nu beschreven.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Gebruik de klassieke Azure portal om te identificeren van de actieve controller
Navigeer in de klassieke Azure portal naar **apparaten** > **onderhoud**, en schuif naar de **domeincontrollers** sectie. Hier kunt u controleren welke domeincontroller actief is.

![Identificeren van de actieve controller in de klassieke Azure-portal](./media/storsimple-controller-replacement/IC752072.png)

**Afbeelding 6** klassieke Azure-portal met de actieve controller

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Windows PowerShell voor StorSimple gebruiken voor het identificeren van de actieve controller
Wanneer u uw apparaat via de seriële console opent, wordt een bannerbericht wordt weergegeven. Het bericht banner bevat basisinformatie over het apparaatgegevens zoals het model, de naam, de versie van de geïnstalleerde software en de status van de domeincontroller die u wilt openen. De volgende afbeelding toont een voorbeeld van een bannerbericht:

![Seriële bannerbericht aangegeven](./media/storsimple-controller-replacement/IC741098.png)

**Afbeelding 7** Banner bericht tonen controller 0 als actief

U kunt het bannerbericht aangegeven gebruiken om te bepalen of de domeincontroller die u met verbonden bent een actieve of passieve.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Controleer het fysieke apparaat te identificeren van de actieve controller
Voor het identificeren van de actieve controller op uw apparaat, zoek de blauwe geleid boven de 5-poort op de achterkant van de primaire behuizing.

Als deze LED knippert, de domeincontroller actief is en de andere controller in standby-modus is. Gebruik de volgende diagram en de volgende tabel als hulpmiddel.

![Apparaat primaire behuizing backplane met dataports](./media/storsimple-controller-replacement/IC741055.png)

**Afbeelding 8** achterzijde primaire behuizing met gegevenspoorten en bewaking LED's

| Label | Beschrijving |
|:--- |:--- |
| 1-6 |DATA 0-5 netwerkpoorten |
| 7 |Blauw LED |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).

