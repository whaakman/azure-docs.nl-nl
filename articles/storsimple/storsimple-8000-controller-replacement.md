---
title: Vervang de apparaatcontroller van een StorSimple 8000-reeks | Microsoft Docs
description: Wordt uitgelegd hoe om te verwijderen en vervangen door een of beide controller-modules op uw StorSimple 8000-apparaat.
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: dd2f6fcc9b2f5d716566e91e89487969613d1005
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61482791"
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Vervangen van een controllermodule op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe om te verwijderen en vervangen door een of beide controller-modules in een StorSimple-apparaat. Hierin worden ook de onderliggende logica voor de controller voor enkele en dubbele vervanging scenario's.

> [!NOTE]
> Voordat u een vervangende domeincontroller uitvoert, wordt u aangeraden de firmware van controller altijd bij te werken naar de nieuwste versie.
> 
> Om te voorkomen dat schade aan uw StorSimple-apparaat, niet verwijdert de controller totdat de LED's worden weergegeven als een van de volgende:
> 
> * Alle lichten zijn uitgeschakeld.
> * LED 3, ![pictogram groen vinkje](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), en ![rode x pictogram](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) knipperen, en LED 0 en 7 LED **ON**.


De volgende tabel bevat de ondersteunde controller vervanging scenario's.

| Aanvraag | Vervanging scenario | Procedures |
|:--- |:--- |:--- |
| 1 |Een controller bevindt zich in een foutstatus, de andere controller is in orde en actief is. |[Eén controller vervangen](#replace-a-single-controller), welke optie beschrijft het [logica achter een vervanging van één domeincontroller](#single-controller-replacement-logic), evenals de [vervanging stappen](#single-controller-replacement-steps). |
| 2 |Beide controllers zijn mislukt en moeten worden vervangen. Het chassis, schijven en schijfbehuizing zijn in orde. |[Dual-controller vervangen](#replace-both-controllers), welke optie beschrijft het [logica achter de vervanging van een dubbele domeincontroller](#dual-controller-replacement-logic), evenals de [vervanging stappen](#dual-controller-replacement-steps). |
| 3 |Domeincontrollers van hetzelfde apparaat of van verschillende apparaten worden gewisseld. Het chassis, schijven en schijfbehuizingen zijn in orde. |Een waarschuwingsbericht van sleuf incompatibel wordt weergegeven. |
| 4 |Een controller ontbreekt en de andere controller is mislukt. |[Dual-controller vervangen](#replace-both-controllers), welke optie beschrijft het [logica achter de vervanging van een dubbele domeincontroller](#dual-controller-replacement-logic), evenals de [vervanging stappen](#dual-controller-replacement-steps). |
| 5 |Een of beide controllers zijn mislukt. U kunt geen toegang tot het apparaat via de seriële console of Windows PowerShell voor externe toegang. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor een handmatige controller vervangingsprocedure. |
| 6 |De controllers hebben een andere buildversie, die kan worden veroorzaakt door:<ul><li>Domeincontrollers hebben een ander software-versie.</li><li>Domeincontrollers hebben verschillende firmwareversie.</li></ul> |Als de softwareversies controller verschillend zijn, wordt de vervangende logica detecteert dat en de versie van de software op de controller vervangen updates.<br><br>Als de domeincontroller firmware-versies verschillend zijn en de oude firmwareversie is **niet** automatisch kan worden geüpgraded, een waarschuwingsbericht wordt weergegeven in de Azure-portal. U moet naar updates zoeken en installeren van de firmware-updates.</br></br>Als de domeincontroller firmware-versies verschillend zijn en de oude firmwareversie automatisch kan worden geüpgraded is, de logica van de vervangende domeincontroller detecteert DPM dit en nadat de controller is gestart, de firmware wordt automatisch bijgewerkt. |

U moet een controllermodule verwijderen als deze is mislukt. Een of beide de controller modules kunnen mislukken, hetgeen kan leiden tot een Eén controller vervangen of een dubbele domeincontroller vervanging. Zie de volgende onderwerpen voor procedures vervanging en de logica achter deze:

* [Eén controller vervangen](#replace-a-single-controller)
* [Beide controllers vervangen](#replace-both-controllers)
* [Een controller verwijderen](#remove-a-controller)
* [Invoegen van een domeincontroller](#insert-a-controller)
* [De actieve controller op uw apparaat identificeren](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Voordat verwijderen en het vervangen van een domeincontroller, lees de veiligheidsinformatie in [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Eén controller vervangen
Wanneer een van de twee controllers op de Microsoft Azure StorSimple-apparaat is mislukt, functioneert of ontbreekt, moet u een één-controller vervangen.

### <a name="single-controller-replacement-logic"></a>Eén controller vervangen logica
In een vervanging van één domeincontroller, moet u eerst de mislukte controller verwijderen. (De resterende domeincontrollers in het apparaat is de actieve controller.) Wanneer u de vervangende domeincontroller invoegt, wordt de volgende acties uitgevoerd:

1. De vervangende domeincontroller wordt onmiddellijk gestart tijdens de communicatie met het StorSimple-apparaat.
2. Een momentopname van de virtuele harde schijf (VHD) voor de actieve controller is gekopieerd op de controller vervangen.
3. De momentopname wordt gewijzigd, zodat wanneer de vervangende domeincontroller wordt gestart vanaf deze VHD, deze worden herkend als een controller voor de stand-by.
4. Wanneer de wijzigingen voltooid zijn, wordt de vervangende domeincontroller wordt gestart als de stand-by-controller.
5. Wanneer beide controllers worden uitgevoerd, wordt het cluster online komt.

### <a name="single-controller-replacement-steps"></a>Eén controller vervangen stappen
Voltooi de volgende stappen uit als een van de domeincontrollers in uw Microsoft Azure StorSimple-apparaat mislukt. (De andere controller moet actief en wordt uitgevoerd. Als beide controllers mislukt of er problemen optreden, gaat u naar [dual-controller vervangen stappen](#dual-controller-replacement-steps).)

> [!NOTE]
> Het kan duren voordat 30: 45 minuten voor de controller opnieuw opstarten en volledig herstellen vanuit de Eén controller vervangingsprocedure. De totale tijd voor de gehele procedure, is met inbegrip van de kabels verbonden ongeveer 2 uur.


#### <a name="to-remove-a-single-failed-controller-module"></a>Een module één mislukte controller verwijderen
1. In de Azure-portal, gaat u naar de StorSimple Device Manager-service, klikt u op **apparaten**, en klik vervolgens op de naam van het apparaat dat u wilt bewaken.
2. Ga naar **Monitor > hardwarestatus**. De status van Controller 0 of Controller 1 moet rood, wat aangeeft dat het een fout.
   
   > [!NOTE]
   > De controller defect is in een vervanging van één domeincontroller is altijd een stand-by-controller.
   
3. Afbeelding 1 en de volgende tabel gebruiken om te vinden van de module controller defect is.
   
    ![Backplane van apparaat primaire behuizing-modules](./media/storsimple-controller-replacement/IC740994.png)
   
    **Afbeelding 1** terug van StorSimple-apparaat
   
   | Label | Description |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controller 0 |
   | 4 |Controller 1 |
4. Op de controller is mislukt, alle verbonden netwerkkabels uit de gegevenspoorten te verwijderen. Als u van een 8600-model gebruikmaakt, moet u ook de SAS-kabels die verbinding maken met de controller de EBOD-controller verwijderen.
5. Volg de stappen in [verwijderen van een domeincontroller](#remove-a-controller) te verwijderen van de controller defect is.
6. Installeer de factory te vervangen in dezelfde sleuf van waaruit de mislukte controller is verwijderd. Dit activeert de logica van de vervangende één domeincontroller. Zie voor meer informatie, [Eén controller vervangen logische](#single-controller-replacement-logic).
7. Terwijl de logica van één domeincontroller vervanging wordt uitgevoerd op de achtergrond, sluit de kabels. Let erop dat u verbinding maken met de kabels aan precies dezelfde manier dat ze zijn verbonden voordat de vervanging.
8. Nadat de controller opnieuw is opgestart, controleert u de **Controller status** en de **Cluster status** in Azure portal om te controleren dat de controller weer een goede status is en in standby-modus is.

> [!NOTE]
> Als u het apparaat via de seriële console bewaakt, ziet u mogelijk meerdere opnieuw is opgestart tijdens het herstellen van de controller van de vervangingsprocedure. Wanneer het menu van de seriële console wordt weergegeven, weet u dat de verwijzing wordt vervangen voltooid is. Als het menu binnen twee uur na het starten van de vervangende domeincontroller niet wordt weergegeven, neemt u [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
>
> Vanaf Update 4 gaan ook kunt u de cmdlet `Get-HCSControllerReplacementStatus` in de Windows PowerShell-interface van het apparaat voor het bewaken van de status van het proces voor het vervangen van controller.
> 

## <a name="replace-both-controllers"></a>Beide controllers vervangen
Wanneer beide controllers op de Microsoft Azure StorSimple-apparaat is mislukt, hoofddatacenter of ontbreken, moet u beide controllers vervangen. 

### <a name="dual-controller-replacement-logic"></a>Logica voor Dual-controller vervangen
In de vervanging van een dubbele domeincontroller, moet u eerst verwijderen beide controllers mislukte en voeg vervolgens vervangingen. Als de domeincontrollers twee vervanging wordt geplaatst, wordt de volgende acties uitgevoerd:

1. De controller vervanging in sleuf 0 controleert het volgende:
   
   1. Is het huidige versies van de firmware en software gebruikt?
   2. Er is een onderdeel van het cluster?
   3. De peer-controller uitgevoerd en is het geclusterde?
      
      Als geen van deze voorwaarden voldaan wordt, de domeincontroller zoekt naar de meest recente dagelijkse back-up (zich in de **nonDOMstorage** op station S). De controller kopieert de meest recente momentopname van de VHD van de back-up.
2. De controller in sleuf 0 maakt gebruik van de momentopname naar de afbeelding zelf.
3. In de tussentijd zorgen, wacht de controller in sleuf 1 voor controller 0 tot en met voltooien van de installatiekopieën en starten.
4. Na het starten van controller 0, detecteert controller 1 in het cluster die zijn gemaakt door de controller 0, die de logica van één domeincontroller vervanging wordt geactiveerd. Zie voor meer informatie, [Eén controller vervangen logische](#single-controller-replacement-logic).
5. Daarna beide controllers zal worden uitgevoerd en wordt het cluster online komt.

> [!IMPORTANT]
> Na de vervanging van een dubbele domeincontroller, nadat het StorSimple-apparaat is geconfigureerd, is het essentieel dat u pas een handmatige back-up van het apparaat. Dagelijkse apparaat configuratie back-ups worden niet geactiveerd totdat na 24 uur zijn verstreken. Werken met [Microsoft Support](storsimple-8000-contact-microsoft-support.md) om een handmatige back-up van uw apparaat.


### <a name="dual-controller-replacement-steps"></a>Stappen voor Dual-controller vervangen
Deze werkstroom is vereist wanneer beide van de domeincontrollers in uw Microsoft Azure StorSimple-apparaat is mislukt. Dit kan gebeuren in een datacenter waarin de koelsysteem niet meer werkt en als gevolg hiervan, mislukt de beide controllers binnen een korte periode. Afhankelijk van of het StorSimple-apparaat of is uitgeschakeld en of u gebruikmaakt van een 8600 of een 8100-model, is een andere reeks stappen vereist.

> [!IMPORTANT]
> Het kan 45 minuten duren voordat naar één uur voor de controller opnieuw opstarten en volledig herstellen vanuit een dubbele domeincontroller vervangingsprocedure. De totale tijd voor de gehele procedure, is met inbegrip van de kabels koppelen ongeveer 2,5 uur.

#### <a name="to-replace-both-controller-modules"></a>Beide modules controller vervangen
1. Als het apparaat is uitgeschakeld, wordt deze stap overslaan en doorgaan met de volgende stap. Als het apparaat is ingeschakeld, schakelt u het apparaat uit.
   
   1. Als u van een 8600-model gebruikmaakt, de primaire behuizing eerste uitschakelen en schakelt u vervolgens de EBOD-behuizing.
   2. Wacht totdat het apparaat volledig is afgesloten. Alle LED's aan de achterzijde van het apparaat is uitgeschakeld.
2. Verwijder de netwerkkabels die zijn verbonden met de gegevenspoorten. Als u van een 8600-model gebruikmaakt, moet u ook de SAS-kabels die verbinding maken met de primaire behuizing van de behuizing EBOD verwijderen.
3. Beide controllers verwijderen uit de StorSimple-apparaat. Zie voor meer informatie, [verwijderen van een domeincontroller](#remove-a-controller).
4. Plaatst u eerst de factory vervanging voor Controller 0 en Controller 1 plaats. Zie voor meer informatie, [invoegen van een domeincontroller](#insert-a-controller). Dit activeert de logica van de vervangende dubbele domeincontroller. Zie voor meer informatie, [dual-controller vervangen logische](#dual-controller-replacement-logic).
5. Terwijl de logica van de vervangende domeincontroller wordt uitgevoerd op de achtergrond, sluit de kabels. Let erop dat u verbinding maken met de kabels aan precies dezelfde manier dat ze zijn verbonden voordat de vervanging. Zie de gedetailleerde instructies voor het model in de kabel uw apparaat-sectie van [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
6. Schakel op het StorSimple-apparaat. Als u een model 8600:
   
   1. Zorg ervoor dat de behuizing EBOD eerst is ingeschakeld.
   2. Wacht totdat de behuizing EBOD wordt uitgevoerd.
   3. Schakel op de primaire behuizing.
   4. Nadat de eerste domeincontroller opnieuw is opgestart en in een foutloze toestand bevindt, wordt het systeem worden uitgevoerd.
      
      > [!NOTE]
      > Als u het apparaat via de seriële console bewaakt, ziet u mogelijk meerdere opnieuw is opgestart tijdens het herstellen van de controller van de vervangingsprocedure. Wanneer het menu van de seriële console wordt weergegeven, weet u dat de verwijzing wordt vervangen voltooid is. Als het menu binnen 2,5 uur van het starten van de vervangende domeincontroller niet wordt weergegeven, neemt u [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
     
## <a name="remove-a-controller"></a>Een controller verwijderen
Gebruik de volgende procedure een defecte controllermodule verwijderen uit uw StorSimple-apparaat.

> [!NOTE]
> De volgende illustraties worden voor controller 0. Voor de controller 1, zou deze worden omgekeerd.


#### <a name="to-remove-a-controller-module"></a>Een controllermodule verwijderen
1. De module vergrendeling tussen uw duim en wijsvinger ingewikkeld.
2. Zacht verondersteld uw duim en wijsvinger samen vrij te geven van de controller-vergrendeling.
   
    ![Controller vergrendeling vrijgeven](./media/storsimple-controller-replacement/IC741047.png)
   
    **Afbeelding 2** vrijgegeven controller vergrendeling
3. Gebruik de vergrendeling als een ingang naar de controller buiten het chassis dia.
   
    ![Sliding domeincontroller buiten het chassis](./media/storsimple-controller-replacement/IC741048.png)
   
    **Afbeelding 3** schuiven de controller buiten het chassis

## <a name="insert-a-controller"></a>Invoegen van een domeincontroller
Gebruik de volgende procedure een factory opgegeven controllermodule installeren na het verwijderen van een foutieve module van uw StorSimple-apparaat.

#### <a name="to-install-a-controller-module"></a>Een controllermodule installeren
1. Controleer of er een beschadiging van de interface-connectors is. Installeer de module niet als een van de connector pincodes zijn beschadigd of verbogen.
2. Schuif de controllermodule in het chassis terwijl de vergrendeling is volledig die zijn uitgebracht.
   
    ![Sliding controller in chassis](./media/storsimple-controller-replacement/IC741053.png)
   
    **Afbeelding 4** schuifregelaar domeincontroller in het chassis
3. Met de netwerkcontroller-module ingevoegd, gaan de vergrendeling wordt gesloten terwijl u verdergaat met het pushen van de controllermodule naar het chassis. De vergrendeling wordt uitgevoerd voor de controller het naar de juiste plaats.
   
    ![Vergrendeling van de controller afsluiten](./media/storsimple-controller-replacement/IC741054.png)
   
    **Afbeelding 5** sluiten van de controller-vergrendeling
4. U bent klaar wanneer vastklikt van de vergrendeling. De **OK** LED worden nu op.
   
   > [!NOTE]
   > Het kan tot vijf minuten voor de controller en de LED activeren duren.
  
5. Om te bevestigen dat de verwijzing wordt vervangen, lukt dit in de Azure-portal, gaat u naar uw apparaat en navigeer vervolgens naar **Monitor** > **hardwarestatus**, en zorg ervoor dat zowel controller 0 en controller 1 zijn in orde (de status is groen).

## <a name="identify-the-active-controller-on-your-device"></a>De actieve controller op uw apparaat identificeren
Er zijn veel situaties, zoals eerst apparaat registratie- of -controller vervangen, waarvoor u de actieve controller vinden op een StorSimple-apparaat. De actieve controller verwerkt alle de firmware- en netwerkresources schijfbewerkingen. U kunt een van de volgende methoden gebruiken voor het identificeren van de actieve controller:

* [De Azure portal gebruiken voor de actieve controller identificeren](#use-the-azure-portal-to-identify-the-active-controller)
* [Windows PowerShell voor StorSimple gebruiken voor het de actieve controller identificeren](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Controleer het fysieke apparaat voor het identificeren van de actieve controller](#check-the-physical-device-to-identify-the-active-controller)

Elk van deze procedures wordt hierna beschreven.

### <a name="use-the-azure-portal-to-identify-the-active-controller"></a>De Azure portal gebruiken voor de actieve controller identificeren
In de Azure-portal, gaat u naar uw apparaat en van daaruit naar **Monitor** > **hardwarestatus**, en schuif naar de **Controllers** sectie. Hier kunt u controleren welke domeincontroller actief is.

![Identificeren van de actieve controller in Azure portal](./media/storsimple-controller-replacement/IC752072.png)

**Afbeelding 6** Azure-portal geeft de actieve controller

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Windows PowerShell voor StorSimple gebruiken voor het de actieve controller identificeren
Wanneer u toegang uw apparaat via de seriële console tot, wordt een bannerbericht wordt weergegeven. Het bannerbericht aangegeven bevat basisinformatie over het apparaatgegevens zoals het model, naam, versie van de geïnstalleerde software en status van de controller die u probeert te openen. De volgende afbeelding toont een voorbeeld van een banner weergegeven:

![Seriële bannerbericht aangegeven](./media/storsimple-controller-replacement/IC741098.png)

**Afbeelding 7** Banner bericht weergeven controller 0 als actief

U kunt het bannerbericht aangegeven gebruiken om te bepalen of de domeincontroller die u met verbonden bent actieve of passieve.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Controleer het fysieke apparaat voor het identificeren van de actieve controller
Ga naar de blauwe LED boven de gegevens-5-poort op de achterkant van de primaire behuizing voor het identificeren van de actieve controller op uw apparaat.

Als deze LED knippert, de domeincontroller actief is en de andere controller is in standby-modus. Gebruik het volgende diagram en de volgende tabel als hulpmiddel.

![Apparaat primaire behuizing backplane met dataports](./media/storsimple-controller-replacement/IC741055.png)

**Afbeelding 8** achterzijde primaire behuizing met gegevenspoorten en controle-LED's

| Label | Description |
|:--- |:--- |
| 1-6 |DATA 0-5 netwerkpoorten |
| 7 |Blauwe LED |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

