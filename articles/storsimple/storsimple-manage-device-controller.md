---
title: Apparaatcontrollers StorSimple beheren | Microsoft Docs
description: Informatie over het stoppen, opnieuw opstarten, afsluiten of opnieuw instellen van uw StorSimple-apparaat-domeincontrollers.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2016
ms.author: alkohli
ms.openlocfilehash: 67dbb0c4066002256efbab6061157c641527e441
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Beheren van uw StorSimple-apparaat-domeincontrollers
## <a name="overview"></a>Overzicht
Deze zelfstudie beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd op uw StorSimple-apparaat-domeincontrollers. De domeincontrollers in uw StorSimple-apparaat zijn redundante (peer)-domeincontrollers in een actief / passief-configuratie. Op een bepaald moment slechts één domeincontroller is actief en alle bewerkingen van de schijf en netwerk wordt verwerkt. De andere controller is in de passieve modus. Als de actieve controller mislukt, actief de passieve controller automatisch.

Deze zelfstudie bevat stapsgewijze instructies voor het beheren van apparaten met behulp van de:

* **Domeincontrollers** sectie van de **onderhoud** pagina in de StorSimple Manager-service
* Windows PowerShell voor StorSimple.

Het is raadzaam dat u de apparaatcontrollers via de StorSimple Manager-service beheren. Als een actie kan alleen worden uitgevoerd met behulp van Windows PowerShell voor StorSimple, maakt de zelfstudie een opmerking ervan.

Na het lezen van deze zelfstudie, kunt u zich kunt:

* Opnieuw opstarten of afsluiten van een StorSimple-apparaat-controller
* Een StorSimple-apparaat afsluiten
* Uw StorSimple-apparaat opnieuw instellen op de standaardwaarden

## <a name="restart-or-shut-down-a-single-controller"></a>Opnieuw opstarten of afsluiten van een één-controller
Een domeincontroller opnieuw opstarten of afsluiten is niet vereist als onderdeel van de werking van het normale systeem. Afsluitbewerkingen voor een domeincontroller enkel apparaat gelden alleen in gevallen waarin een hardware-onderdeel van het apparaat moet worden vervangen. Een controller mogelijk ook opnieuw opstarten in een situatie waarin prestaties is van invloed op een overmatige geheugengebruik of een niet-functionerende controller. U moet mogelijk ook opnieuw starten van een domeincontroller na de vervanging van een geslaagde domeincontroller als u wilt inschakelen en het testen van de controller vervangen.

Opnieuw starten van een apparaat is niet verbonden initiators, ervan uitgaande dat de passieve domeincontroller beschikbaar is, wordt verplaatst. Als een passieve controller niet is beschikbaar of ingeschakeld, uitgeschakeld en opnieuw starten van de actieve controller kan leiden tot de onderbreking van de service en uitvaltijd.

> [!IMPORTANT]
> * **Een actieve domeincontroller moet nooit fysiek worden verwijderd omdat dit tot verlies van redundantie en een verhoogd risico op uitval leiden zou.**
> * De volgende procedure geldt alleen voor het fysieke StorSimple-apparaat. Zie voor meer informatie over het starten, stoppen en opnieuw opstarten van het virtuele apparaat [werken met het virtuele apparaat](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Opnieuw opstarten of afsluiten van een domeincontroller enkel apparaat via de klassieke Azure portal van de StorSimple Manager-service of de Windows PowerShell voor StorSimple

Als u wilt uw apparaatcontrollers beheren vanuit de klassieke Azure portal, moet u de volgende stappen uitvoeren.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Opnieuw opstarten of afsluiten van een domeincontroller in de klassieke portal
1. Navigeer naar **apparaten > onderhoud**.
2. Ga naar **hardwarestatus** en controleer of de status van beide domeincontrollers op uw apparaat **goed**.
   
    ![Controleer of de StorSimple-apparaat domeincontrollers zijn in orde](./media/storsimple-manage-device-controller/IC766017.png)
3. Van de onderkant van de **onderhoud** pagina, klikt u op **domeincontrollers beheren**.
   
    ![Apparaatcontrollers StorSimple beheren](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Als u niet kunt zien **domeincontrollers beheren**, moet u om updates te installeren. Zie voor meer informatie [uw StorSimple-apparaat bijwerken](storsimple-update-device.md).
   > 
   > 
4. In de **instellingen van de domeincontroller wijzigen** dialoogvenster de volgende handelingen uit:
   
   1. Van de **Select Controller** vervolgkeuzelijst, selecteert u de domeincontroller die u wilt beheren. De opties zijn Controller 0 en Controller 1. Deze domeincontrollers worden ook aangeduid als actief of passief.
      
      > [!NOTE]
      > Een domeincontroller kan niet worden beheerd als het is niet beschikbaar of ingeschakeld, uitgeschakeld en deze niet wordt weergegeven in de vervolgkeuzelijst.
      > 
      > 
   2. Van de **actie selecteren** vervolgkeuzelijst Kies **herstart controller** of **controller afgesloten**.
      
       ![Start passieve domeincontroller voor StorSimple-apparaat](./media/storsimple-manage-device-controller/IC766020.png)
   3. Klik op het vinkje ![Vinkje](./media/storsimple-manage-device-controller/IC740895.png).

Dit wordt opnieuw opstarten of afsluiten van de domeincontroller. De onderstaande tabel ziet u de details van wat er, afhankelijk van de selecties die u hebt aangebracht gebeurt in de **instellingen van de domeincontroller wijzigen** in het dialoogvenster.  

| Selectie # | Als u wilt... | Dit gebeurt. |
| --- | --- | --- |
| 1. |Start de passieve domeincontroller. |Een taak wordt gemaakt om de domeincontroller opnieuw te starten en u ontvangt een melding nadat de taak is gemaakt. Er wordt nu de domeincontroller opnieuw opstarten. U kunt het proces opnieuw opstarten controleren door het openen van **Service > Dashboard > weergeven bewerkingslogboeken** en vervolgens door de parameters die specifiek zijn voor uw service te filteren. |
| 2. |Start opnieuw op de actieve controller. |Ziet u de volgende waarschuwing: 'als u de actieve controller opnieuw start, het apparaat wordt failover naar de passieve controller. Wilt u doorgaan?' </br>Als u ervoor kiest om door te gaan met deze bewerking, de volgende stappen worden identiek zijn aan die wordt gebruikt om de passieve controller opnieuw starten (Zie selectie 1). |
| 3. |Sluit de passieve controller. |U ziet het volgende bericht: 'nadat afsluiten voltooid is, u moet de knop push op uw domeincontroller te worden ingeschakeld. Weet u zeker dat u wilt deze controller afsluiten?' </br>Als u ervoor kiest om door te gaan met deze bewerking, de volgende stappen worden identiek zijn aan die wordt gebruikt om de passieve controller opnieuw starten (Zie selectie 1). |
| 4. |Sluit de actieve controller. |U ziet het volgende bericht: 'nadat afsluiten voltooid is, u moet de knop push op uw domeincontroller te worden ingeschakeld. Weet u zeker dat u wilt deze controller afsluiten?' </br>Als u ervoor kiest om door te gaan met deze bewerking, de volgende stappen worden identiek zijn aan die wordt gebruikt om de passieve controller opnieuw starten (Zie selectie 1). |

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Windows PowerShell voor StorSimple
Voer de volgende stappen uit als u wilt afsluiten of opnieuw opstarten van één controller op uw StorSimple-apparaat via de klassieke Azure portal.

1. Toegang tot het apparaat met behulp van de seriële console of een Telnet-sessie vanaf een externe computer. Verbinding maken met de Controller 0 of 1 van de domeincontroller met de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
3. In het bannerbericht aangegeven, noteer de domeincontroller die u met (Controller 0 of 1 voor Controller verbonden bent) en de actieve of de passieve controller (standby).
   
   * Als u wilt afsluiten één controller bij de opdrachtprompt, typt u:
     
       `Stop-HcsController`
     
       Hiermee wordt de domeincontroller die u met verbonden bent afgesloten. Als u de actieve controller stopt, is klikt u vervolgens het failover op de passieve controller voordat deze wordt afgesloten.
   * Typ het volgende om een domeincontroller, bij de prompt opnieuw te starten:
     
       `Restart-HcsController`
     
       Hiermee wordt de domeincontroller die u met verbonden bent opnieuw opgestart. Als u de actieve controller opnieuw opstart, wordt het schakelt over naar de passieve controller voordat opnieuw wordt opgestart.

## <a name="shut-down-a-storsimple-device"></a>Een StorSimple-apparaat afsluiten
Deze sectie wordt uitgelegd hoe u een actieve of een mislukte StorSimple-apparaat vanaf een externe computer afsluiten. Een apparaat is na het afsluiten van de apparaatcontrollers uitgeschakeld. Het afsluiten van een apparaat wordt uitgevoerd zodra het apparaat fysiek wordt verplaatst of buiten de service wordt uitgevoerd.

> [!IMPORTANT]
> Voordat u het apparaat wordt afgesloten, controleert u de status van de Apparaatonderdelen. Navigeer naar **apparaten > onderhoud > hardwarestatus** en controleer of de LED-status van alle onderdelen groen is. Alleen een gezonde apparaat moeten groene status. Als uw apparaat is wordt afgesloten om een niet-functionerende onderdeel vervangen, ziet u een mislukte (rode) of een gedegradeerde (geel) status voor de betreffende onderdelen.
> 
> 

#### <a name="to-shut-down-a-storsimple-device"></a>Een StorSimple-apparaat afsluiten
1. Gebruik de [opnieuw opstarten of afsluiten van een domeincontroller](#restart-or-shut-down-a-single-controller) procedure om te bepalen en afsluiten van de passieve controller op uw apparaat. U kunt deze bewerking uitvoeren in de klassieke Azure portal of in Windows PowerShell voor StorSimple.
2. Herhaal deze stap de actieve controller af te sluiten.
3. U moet nu om te kijken naar het vorige vlak van het apparaat. Nadat de twee domeincontrollers volledig worden afgesloten, moet de status LED's op beide domeincontrollers rood knippert. Als u moet het apparaat volledig uitschakelen op dit moment, spiegelen de switches power op zowel stroom en koeling Modules (PCMs) naar de positie OFF. Dit moet het apparaat uitschakelen.

<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Het apparaat niet Fabrieksinstellingen terugzetten
> [!IMPORTANT]
> Als u uw apparaat opnieuw instelt fabrieksinstellingen moet, neem dan contact op met Microsoft Support. De hieronder beschreven procedure moet alleen worden gebruikt in combinatie met Microsoft Support.
> 
> 

Deze procedure wordt beschreven hoe u uw Microsoft Azure StorSimple-apparaat opnieuw instelt fabrieksinstellingen met Windows PowerShell voor StorSimple.
Instellen van een apparaat verwijdert alle gegevens en instellingen van het volledige cluster standaard.

Voer de volgende stappen uit om uw Microsoft Azure StorSimple-apparaat opnieuw instelt fabrieksinstellingen:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Het apparaat opnieuw wordt ingesteld op de standaardinstellingen in Windows PowerShell voor StorSimple
1. Toegang tot het apparaat via de seriële console. Controleer het bannerbericht aangegeven om ervoor te zorgen dat u met de actieve controller verbonden bent.
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
3. Typ de volgende opdracht om in te stellen het hele cluster, het verwijderen van alle gegevens, metagegevens en controller-instellingen bij de opdrachtprompt:
   
    `Reset-HcsFactoryDefault`
   
    Als u in plaats daarvan één controller herstellen, gebruikt u de [Reset HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet uit met de `-scope` parameter.)
   
    Het systeem wordt meerdere keren opnieuw. U wordt gewaarschuwd wanneer het opnieuw instellen is voltooid. Afhankelijk van het systeemmodel kan 45 tot 60 minuten voor een 8100-apparaat en een 8600 dit proces voltooien 60-90 minuten duren.
   
   > [!TIP]
   > * Als u met Update 1.2 of eerder gebruiken de `–SkipFirmwareVersionCheck` -parameter voor het overslaan van de controle van de firmware-versie (anders wordt er een fout met niet overeenkomende firmware: terugzetten op fabrieksinstellingen kan niet doorgaan omdat de firmware-versies niet overeenkomen. ).
   > * De factory opnieuw instellen van procedure kan mislukken voor StorSimple-apparaten met Update 1 of 1.1 in de portal Government en een vervangende geslaagde enkele of dubbele domeincontroller (met vervanging domeincontrollers die zijn verzonden met software vooraf Update 1) hebt uitgevoerd. Dit gebeurt wanneer de fabrieksinstellingen van de installatiekopie op de aanwezigheid van een SHA1-bestand op de domeincontroller die niet bestaat voor 1 software vóór het bijwerken wordt gevalideerd. Als u dat deze de fabrieksinstellingen van fout ziet, neem dan contact op met Microsoft Support om u te helpen met de volgende stappen. Dit probleem is niet zichtbaar met vervanging domeincontrollers die zijn verzonden van de fabriek met Update 1 of hoger.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Vragen en antwoorden over het beheren van apparaatcontrollers
In deze sectie samenvatten we enkele veelgestelde vragen met betrekking tot het beheren van het StorSimple-apparaat domeincontrollers.

**V:** Wat gebeurt er als beide domeincontrollers op mijn apparaat in orde en ingeschakeld zijn op en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als beide domeincontrollers op uw apparaat in orde en ingeschakeld zijn, wordt u gevraagd om bevestiging. U kunt:

* **Opnieuw opstarten van de actieve controller** : U ontvangt een melding dat opnieuw starten van een actieve domeincontroller, het apparaat wordt failover naar de passieve controller. De controller wordt opnieuw opgestart.
* **Een actieve domeincontroller afgesloten** : U ontvangt een melding dat het afsluiten van een actieve domeincontroller leiden uitvaltijd tot zal. U moet ook de knop push op het apparaat in te schakelen op de domeincontroller.

**V:** Wat gebeurt er als u de passieve controller op mijn apparaat is niet beschikbaar is of is uitgeschakeld uitgeschakeld en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als de passieve controller op uw apparaat is niet beschikbaar of ingeschakeld, uitgeschakeld en u wilt:

* **Opnieuw opstarten van de actieve controller** : U wordt gewaarschuwd dat doorgaan met de bewerking in een tijdelijke onderbreking van de service resulteert en wordt u gevraagd om bevestiging.
* **Een actieve domeincontroller afgesloten** : U ontvangt een melding dat het doorgaan met de bewerking zal leiden tot uitvaltijd en u moet de knop push op een of beide domeincontrollers om in te schakelen op het apparaat. U wordt gevraagd om bevestiging.

**V:** Wanneer de domeincontroller opnieuw opstarten of afsluiten doet mislukt verlopen?

**A:** Opnieuw opstarten of afsluiten van een domeincontroller kan mislukken als:

* Een apparaat wordt bijgewerkt.
* Een herstart van de domeincontroller wordt al uitgevoerd.
* Het afsluiten van een domeincontroller wordt al uitgevoerd.

**V:** Hoe kunt u achterhalen als een domeincontroller is opgestart of afgesloten?

**A:** U kunt de status van de domeincontroller op de pagina Onderhoud controleren. De status van de domeincontroller wordt aangegeven of een domeincontroller is opnieuw opgestart of afgesloten. De pagina waarschuwingen bevatten ook een informatieve waarschuwing als de domeincontroller is opgestart of afgesloten. De domeincontroller opnieuw opstarten en afsluiten bewerkingen worden ook opgenomen in de bewerkingslogboeken van de. Voor meer informatie over bewerkingslogboeken, gaat u naar [Bekijk de bewerkingslogboeken](storsimple-service-dashboard.md#view-the-operations-logs).

**V:** Is er gevolgen voor de i/o's als gevolg van een failover van de domeincontroller?

**A:** De TCP-verbindingen tussen de initiators en actieve controller wordt opnieuw ingesteld als gevolg van een failover van de domeincontroller, maar wordt pas gemaakt wanneer de passieve controller wordt ervan uitgegaan dat de bewerking. Mogelijk zijn er een pauze van tijdelijke (minder dan 30 seconden) in de i/o-activiteit tussen initiators en het apparaat in de loop van deze bewerking.

**V:** Hoe kan ik mijn domeincontroller voor het service nadat deze is afgesloten en verwijderd retourneren?

**A:** Als een domeincontroller wilt service herstellen, moet u dit invoegen in het chassis zoals beschreven in [vervangen door een module van de domeincontroller op uw StorSimple-apparaat](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Volgende stappen
* Als u problemen ondervindt met uw StorSimple-apparaatcontrollers die u niet kunt oplossen met behulp van de procedures die worden vermeld in deze zelfstudie [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

