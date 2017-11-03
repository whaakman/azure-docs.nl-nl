---
title: StorSimple 8000 series apparaatcontrollers beheren | Microsoft Docs
description: Informatie over het stoppen, opnieuw opstarten, afsluiten of opnieuw instellen van uw StorSimple-apparaat-domeincontrollers.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 75c1bdb570967b6d1902697597f0b5bf3f4ffb7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Beheren van uw StorSimple-apparaat-domeincontrollers

## <a name="overview"></a>Overzicht

Deze zelfstudie beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd op uw StorSimple-apparaat-domeincontrollers. De domeincontrollers in uw StorSimple-apparaat zijn redundante (peer)-domeincontrollers in een actief / passief-configuratie. Op een bepaald moment slechts één domeincontroller is actief en alle bewerkingen van de schijf en netwerk wordt verwerkt. De andere controller is in de passieve modus. Als de actieve controller mislukt, wordt de passieve controller automatisch geactiveerd.

Deze zelfstudie bevat stapsgewijze instructies voor het beheren van apparaten met behulp van de:

* **Domeincontrollers** blade voor uw apparaat in de service Manager voor StorSimple-apparaat.
* Windows PowerShell voor StorSimple.

Het is raadzaam dat u de apparaatcontrollers via de service Manager voor StorSimple-apparaat beheren. Als een actie kan alleen worden uitgevoerd met behulp van Windows PowerShell voor StorSimple, maakt de zelfstudie een opmerking ervan.

Na het lezen van deze zelfstudie, kunt u zich kunt:

* Opnieuw opstarten of afsluiten van een StorSimple-apparaat-controller
* Een StorSimple-apparaat afsluiten
* Uw StorSimple-apparaat opnieuw instellen op de standaardwaarden

## <a name="restart-or-shut-down-a-single-controller"></a>Opnieuw opstarten of afsluiten van een één-controller
Een domeincontroller opnieuw opstarten of afsluiten is niet vereist als onderdeel van de werking van het normale systeem. Afsluitbewerkingen voor een domeincontroller enkel apparaat gelden alleen in gevallen waarin een hardware-onderdeel van het apparaat moet worden vervangen. Een controller mogelijk ook opnieuw opstarten in een situatie waarin prestaties is van invloed op een overmatige geheugengebruik of een niet-functionerende controller. U moet mogelijk ook opnieuw starten van een domeincontroller na de vervanging van een geslaagde domeincontroller als u wilt inschakelen en het testen van de controller vervangen.

Opnieuw starten van een apparaat is niet verbonden initiators, ervan uitgaande dat de passieve domeincontroller beschikbaar is, wordt verplaatst. Als een passieve controller niet is beschikbaar of ingeschakeld, uitgeschakeld en opnieuw starten van de actieve controller kan leiden tot de onderbreking van de service en uitvaltijd.

> [!IMPORTANT]
> * **Een actieve domeincontroller moet nooit fysiek worden verwijderd omdat dit tot verlies van redundantie en een verhoogd risico op uitval leiden zou.**
> * De volgende procedure geldt alleen voor het fysieke StorSimple-apparaat. Zie voor meer informatie over het starten, stoppen en opnieuw opstarten van het apparaat van de Cloud StorSimple [werken met het toestel cloud](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

U kunt opnieuw opstarten of afsluiten van een domeincontroller enkel apparaat via de Azure-portal van de service Manager voor StorSimple-apparaat of de Windows PowerShell voor StorSimple.

Als u wilt uw apparaatcontrollers beheren vanuit de Azure-portal, moet u de volgende stappen uitvoeren.

#### <a name="to-restart-or-shut-down-a-controller-in-azure-portal"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Azure-portal
1. In uw StorSimple-apparaat Manager-service, gaat u naar **apparaten**. Selecteer uw apparaat in de lijst met apparaten. 

    ![Kies een apparaat](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Ga naar **instellingen > domeincontrollers**.
   
    ![Controleer of de StorSimple-apparaat domeincontrollers zijn in orde](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. In de **domeincontrollers** blade, Controleer of de status van beide domeincontrollers op uw apparaat **goed**. Selecteer een domeincontroller, klik met de rechtermuisknop en selecteer vervolgens **opnieuw** of **afgesloten**.

    ![Selecteer opnieuw opstarten of afsluiten apparaatcontrollers voor StorSimple](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Een taak voor opnieuw opstarten of afsluiten van de domeincontroller wordt gemaakt en krijgt u waarschuwingen van toepassing, indien van toepassing. Voor het bewaken van opnieuw opstarten of afsluiten, gaat u naar **Service > activiteitenlogboeken** en filter vervolgens de parameters die specifiek zijn voor uw service. Als een domeincontroller is afgesloten, moet u de knop om in te schakelen op de controller te schakelen push.

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Windows PowerShell voor StorSimple
Voer de volgende stappen uit als u wilt afsluiten of opnieuw opstarten van één controller op uw StorSimple-apparaat uit de Windows PowerShell voor StorSimple.

1. Toegang tot het apparaat via de seriële console of een Telnet-sessie vanaf een externe computer. Voor verbinding met de Controller 0 of 1 van de domeincontroller, volg de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
3. In het bannerbericht aangegeven, noteer de domeincontroller die u met (Controller 0 of 1 voor Controller verbonden bent) en de actieve of de passieve controller (standby).
   
   * Als u wilt afsluiten één controller bij de opdrachtprompt, typt u:
     
       `Stop-HcsController`
     
       Dit is de controller waaraan u met verbonden bent afgesloten. Als u de actieve controller stopt, klikt u vervolgens wordt het apparaat overgenomen door naar de passieve controller.

   * Typ het volgende om een domeincontroller, bij de prompt opnieuw te starten:
     
       `Restart-HcsController`
     
       De controller waaraan u met verbonden bent opnieuw is opgestart. Als u de actieve controller opstarten, failover naar de passieve controller voordat opnieuw wordt opgestart.

## <a name="shut-down-a-storsimple-device"></a>Een StorSimple-apparaat afsluiten

Deze sectie wordt uitgelegd hoe u een actieve of een mislukte StorSimple-apparaat vanaf een externe computer afsluiten. Een apparaat is uitgeschakeld nadat de apparaatcontrollers worden afgesloten. Het afsluiten van een apparaat wordt uitgevoerd zodra het apparaat fysiek wordt verplaatst of buiten de service wordt uitgevoerd.

> [!IMPORTANT]
> Voordat u het apparaat wordt afgesloten, controleert u de status van de Apparaatonderdelen. Navigeer naar het apparaat en klik vervolgens op **instellingen > Hardware health**. In de **Status- en hardware health** blade, Controleer of de LED-status van alle onderdelen groen is. Alleen een gezonde apparaat heeft een groene status. Als uw apparaat is wordt afgesloten om een niet-functionerende onderdeel vervangen, ziet u een mislukte (rode) of een gedegradeerde (geel) status voor de betreffende onderdelen.


#### <a name="to-shut-down-a-storsimple-device"></a>Een StorSimple-apparaat afsluiten

1. Gebruik de [opnieuw opstarten of afsluiten van een domeincontroller](#restart-or-shut-down-a-single-controller) procedure om te bepalen en afsluiten van de passieve controller op uw apparaat. U kunt deze bewerking uitvoeren in de Azure-portal of in Windows PowerShell voor StorSimple.
2. Herhaal deze stap de actieve controller af te sluiten.
3. Nu moet u het back-vlak van het apparaat bekijken. Nadat de twee domeincontrollers volledig worden afgesloten, moet de status LED's op beide domeincontrollers rood knippert. Als u moet het apparaat volledig uitschakelen op dit moment, spiegelen de switches power op zowel stroom en koeling Modules (PCMs) naar de positie OFF. Dit moet het apparaat uitschakelen.

## <a name="reset-the-device-to-factory-default-settings"></a>Het apparaat niet Fabrieksinstellingen terugzetten

> [!IMPORTANT]
> Als u uw apparaat opnieuw instelt fabrieksinstellingen moet, neem dan contact op met Microsoft Support. De hieronder beschreven procedure moet alleen worden gebruikt in combinatie met Microsoft Support.

Deze procedure wordt beschreven hoe u uw Microsoft Azure StorSimple-apparaat opnieuw instelt fabrieksinstellingen met Windows PowerShell voor StorSimple.
Instellen van een apparaat verwijdert alle gegevens en instellingen van het volledige cluster standaard.

Voer de volgende stappen uit om uw Microsoft Azure StorSimple-apparaat opnieuw instelt fabrieksinstellingen:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Het apparaat opnieuw wordt ingesteld op de standaardinstellingen in Windows PowerShell voor StorSimple
1. Toegang tot het apparaat via de seriële console. Controleer het bannerbericht aangegeven om ervoor te zorgen dat u met verbonden bent de **Active** controller.
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
3. Typ de volgende opdracht om in te stellen het hele cluster, het verwijderen van alle gegevens, metagegevens en controller-instellingen bij de opdrachtprompt:
   
    `Reset-HcsFactoryDefault`
   
    Als u in plaats daarvan één controller herstellen, gebruikt u de [Reset HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet uit met de `-scope` parameter.)
   
    Het systeem wordt meerdere keren opnieuw. U wordt gewaarschuwd wanneer het opnieuw instellen is voltooid. Afhankelijk van het systeemmodel kan 45 tot 60 minuten voor een 8100-apparaat en een 8600 dit proces voltooien 60-90 minuten duren.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Vragen en antwoorden over het beheren van apparaatcontrollers
In deze sectie samenvatten we enkele veelgestelde vragen met betrekking tot het beheren van het StorSimple-apparaat domeincontrollers.

**V:** Wat gebeurt er als beide domeincontrollers op mijn apparaat in orde en ingeschakeld zijn op en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als beide domeincontrollers op uw apparaat in orde en ingeschakeld zijn, u om bevestiging wordt gevraagd. U kunt:

* **Opnieuw opstarten van de actieve controller** : U wordt gewaarschuwd dat het apparaat failover naar de passieve controller opnieuw starten van een actieve domeincontroller veroorzaakt. De domeincontroller opnieuw wordt opgestart.
* **Een actieve domeincontroller afgesloten** : U wordt gewaarschuwd dat het afsluiten van een actieve domeincontroller in uitvaltijd resulteert. U moet ook de knop push op het apparaat in te schakelen op de domeincontroller.

**V:** Wat gebeurt er als u de passieve controller op mijn apparaat is niet beschikbaar is of is uitgeschakeld uitgeschakeld en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als de passieve controller op uw apparaat is niet beschikbaar of ingeschakeld, uitgeschakeld en u wilt:

* **Opnieuw opstarten van de actieve controller** : U wordt gewaarschuwd dat doorgaan met de bewerking in een tijdelijke onderbreking van de service resulteert en u om bevestiging wordt gevraagd.
* **Afsluiten van een actieve domeincontroller** : U wordt gewaarschuwd dat doorgaan met de bewerking resulteert in uitvaltijd. U moet ook de knop push op een of beide domeincontrollers om in te schakelen op het apparaat. U wordt gevraagd om bevestiging.

**V:** Wanneer de domeincontroller opnieuw opstarten of afsluiten doet mislukt verlopen?

**A:** Opnieuw opstarten of afsluiten van een domeincontroller kan mislukken als:

* Een apparaat wordt bijgewerkt.
* Een herstart van de domeincontroller wordt al uitgevoerd.
* Het afsluiten van een domeincontroller wordt al uitgevoerd.

**V:** Hoe kunt u achterhalen als een domeincontroller is opgestart of afgesloten?

**A:** U kunt de status van de domeincontroller op de blade Controller controleren. De status van de domeincontroller wordt aangegeven of een domeincontroller wordt momenteel opnieuw opstarten of afsluiten. Bovendien de **waarschuwingen** blade bevatten een informatieve waarschuwing als de domeincontroller opnieuw wordt opgestart of afgesloten. De domeincontroller opnieuw opstarten en afsluiten bewerkingen zijn ook opgenomen in de logboeken acitivity. Ga voor meer informatie over acitivity logboeken naar [Bekijk de activiteitenlogboeken](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**V:** Is er gevolgen voor de i/o's als gevolg van een failover van de domeincontroller?

**A:** De TCP-verbindingen tussen de initiators en actieve controller wordt opnieuw ingesteld als gevolg van een failover van de domeincontroller, maar wordt pas gemaakt wanneer de passieve controller wordt ervan uitgegaan dat de bewerking. Mogelijk zijn er een pauze van tijdelijke (minder dan 30 seconden) in de i/o-activiteit tussen initiators en het apparaat in de loop van deze bewerking.

**V:** Hoe kan ik mijn domeincontroller voor het service nadat deze is afgesloten en verwijderd retourneren?

**A:** Als een domeincontroller wilt service herstellen, moet u dit invoegen in het chassis zoals beschreven in [vervangen door een module van de domeincontroller op uw StorSimple-apparaat](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Volgende stappen
* Als u problemen ondervindt met uw StorSimple-apparaatcontrollers die u niet kunt oplossen met behulp van de procedures die worden vermeld in deze zelfstudie [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

