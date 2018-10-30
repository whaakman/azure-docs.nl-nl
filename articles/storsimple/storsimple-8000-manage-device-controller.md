---
title: Apparaatcontrollers voor StorSimple 8000-serie beheren | Microsoft Docs
description: Informatie over het stoppen, starten, afsluiten of opnieuw instellen van uw StorSimple-apparaat-controllers.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 9a5be85b917682afeaecd67a4176edb7040d5b4b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215426"
---
# <a name="manage-your-storsimple-device-controllers"></a>Uw StorSimple-apparaat domeincontrollers beheren

## <a name="overview"></a>Overzicht

Deze zelfstudie beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd op uw StorSimple-apparaat-controllers. De domeincontrollers in uw StorSimple-apparaat zijn redundante (peer)-domeincontrollers in een actief-passieve configuratie. Op een bepaald moment slechts één domeincontroller actief is en alle bewerkingen voor de schijf en netwerk wordt verwerkt. De andere controller is in de passieve modus. Als de actieve controller is mislukt, wordt de passieve controller automatisch geactiveerd.

Deze zelfstudie bevat stapsgewijze instructies voor het beheren van apparaten met behulp van de:

* **Domeincontrollers** blade voor uw apparaat in de StorSimple Device Manager-service.
* Windows PowerShell voor StorSimple.

Het is raadzaam dat u de apparaatcontrollers via de StorSimple Device Manager-service beheren. Als een actie kan alleen worden uitgevoerd met behulp van Windows PowerShell voor StorSimple, wordt in de zelfstudie een notitie van deze.

Na het lezen van deze zelfstudie, kunt u zich aan:

* Opnieuw opstarten of afsluiten van de controller van een StorSimple-apparaat
* Een StorSimple-apparaat uitschakelen
* Uw StorSimple-apparaat opnieuw instellen naar de fabrieksinstellingen

## <a name="restart-or-shut-down-a-single-controller"></a>Opnieuw opstarten of afsluiten Eén controller
Een controller opnieuw opstarten of afsluiten is niet vereist als onderdeel van de werking van het normale systeem. Afsluitbewerkingen voor een enkel apparaatcontroller gelden alleen in gevallen waarin een hardware-onderdeel van het apparaat moet worden vervangen. Een controller kan ook worden opgestart in een situatie waarin prestaties wordt beïnvloed door het gebruik van excessief veel geheugen of een niet-functionerende controller. U moet mogelijk ook een controller opnieuw opstarten na een geslaagde controller vervangen, als u wilt schakelen en te testen van de controller vervangen.

Opnieuw starten van een apparaat is niet ontregelen verbonden initiators, ervan uitgaande dat de passieve controller beschikbaar is. Als een passieve controller niet is beschikbaar is of is uitgeschakeld, uitgeschakeld, wordt de actieve controller opnieuw opstarten kan leiden tot de onderbreking van de service en uitvaltijd.

> [!IMPORTANT]
> * **Een actieve domeincontroller moet nooit fysiek worden verwijderd omdat dit tot verlies van redundantie en een verhoogd risico op uitvaltijd leiden zou.**
> * De volgende procedure geldt alleen voor de fysieke StorSimple-apparaat. Zie voor meer informatie over het starten, stoppen en opnieuw opstarten van de StorSimple-Cloudapparaat [werken met het cloudapparaat](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

U kunt opnieuw opstarten of afsluiten van een controller voor één apparaat via de Azure-portal van de StorSimple Device Manager-service of Windows PowerShell voor StorSimple.

Voor het beheren van uw apparaatcontrollers vanuit Azure portal, moet u de volgende stappen uitvoeren.

#### <a name="to-restart-or-shut-down-a-controller-in-azure-portal"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Azure portal
1. In uw StorSimple Device Manager-service, gaat u naar **apparaten**. Selecteer uw apparaat uit de lijst met apparaten. 

    ![Kies een apparaat](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Ga naar **instellingen > Controllers**.
   
    ![Controleer of de StorSimple-apparaatcontrollers in orde zijn](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. In de **Controllers** blade, Controleer of de status van beide controllers op uw apparaat **orde**. Selecteer een domeincontroller, met de rechtermuisknop op en selecteer vervolgens **opnieuw** of **afsluiten**.

    ![Selecteer opnieuw opstarten of afsluiten apparaatcontrollers voor StorSimple](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Een taak wordt gemaakt voor het opnieuw opstarten of afsluiten van de controller en krijgt u waarschuwingen van toepassing, indien van toepassing. Voor het controleren van het opnieuw opstarten of afsluiten, gaat u naar **Service > activiteitenlogboeken** en filter vervolgens de parameters die specifiek zijn voor uw service. Als een domeincontroller is afgesloten, moet u push-knop om in te schakelen op de controller deze in te schakelen.

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Windows PowerShell voor StorSimple
Voer de volgende stappen uit als u wilt afsluiten of opnieuw opstarten van een één domeincontroller op uw StorSimple-apparaat uit de Windows PowerShell voor StorSimple.

1. Toegang tot het apparaat via de seriële console of met een Telnet-sessie vanaf een externe computer. Volg de stappen in voor verbinding met de Controller 0 of 1 van de domeincontroller, [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**.
3. In het bannerbericht aangegeven, maak een notitie van de controller u met (Controller 0 of Controller 1 verbonden bent) en of deze de actieve of de passieve controller (stand-by).
   
   * Als u wilt afsluiten Eén controller, bij de opdrachtprompt, typt u:
     
       `Stop-HcsController`
     
       Hiermee wordt de controller waaraan u met verbonden bent afgesloten. Als u de actieve controller stoppen, klikt u vervolgens failover het apparaat-schakeling naar de passieve controller.

   * Typ het volgende om een domeincontroller, bij de prompt opnieuw te starten:
     
       `Restart-HcsController`
     
       De controller waaraan u met verbonden bent opnieuw is opgestart. Als u de actieve controller opnieuw start, failover het-schakeling naar de passieve controller voordat opnieuw wordt opgestart.

## <a name="shut-down-a-storsimple-device"></a>Een StorSimple-apparaat uitschakelen

In deze sectie wordt uitgelegd hoe u een actief of een mislukte StorSimple-apparaat vanaf een externe computer afgesloten. Een apparaat is uitgeschakeld nadat de beide apparaatcontrollers worden afgesloten. Het apparaat wordt afgesloten gebeurt wanneer het apparaat fysiek wordt verplaatst of is uitgevoerd buiten gebruik gesteld.

> [!IMPORTANT]
> Voordat u op het apparaat wordt afgesloten, controleert u de status van onderdelen van het apparaat. Navigeer naar uw apparaat en klik vervolgens op **instellingen > hardwarestatus**. In de **Status en hardwarestatus** blade, Controleer of het LED-status van alle onderdelen van groen. Alleen een gezonde apparaat heeft een groene status. Als uw apparaat wordt afgesloten omlaag naar een niet-functionerende onderdeel vervangen, ziet u een mislukte (rode) of een gedegradeerde status bevindt (geel) voor de betreffende onderdelen.


#### <a name="to-shut-down-a-storsimple-device"></a>Om een StorSimple-apparaat af te sluiten

1. Gebruik de [opnieuw opstarten of afsluiten van een domeincontroller](#restart-or-shut-down-a-single-controller) procedure om te bepalen en afsluiten van de passieve controller op uw apparaat. U kunt deze bewerking uitvoeren in Azure portal of in Windows PowerShell voor StorSimple.
2. Herhaal deze stap om de actieve controller af te sluiten.
3. U moet er nu uitzien op het vlak van het vorige van het apparaat. Nadat de twee controllers volledig worden afgesloten, moet de status-LED's op beide controllers red knipperende. Als u moet het apparaat volledig uitschakelen op dit moment, spiegelen om de energie-switches op zowel voeding en koeling Modules (PCMs) op de positie OFF. Dit moet het apparaat uitschakelen.

## <a name="reset-the-device-to-factory-default-settings"></a>Het apparaat terugzetten op fabrieksinstellingen

> [!IMPORTANT]
> Als u uw apparaat terugzetten op fabrieksinstellingen moet, neem dan contact op met Microsoft Support. De hieronder beschreven procedure moet worden gebruikt alleen in combinatie met Microsoft Support.

Deze procedure wordt beschreven hoe u uw Microsoft Azure StorSimple-apparaat opnieuw instelt standaardinstellingen met behulp van Windows PowerShell voor StorSimple.
Alle gegevens en instellingen verwijdert opnieuw instellen van een apparaat uit het hele cluster standaard.

De volgende stappen uitvoeren om uw Microsoft Azure StorSimple-apparaat opnieuw instelt fabrieksinstellingen:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Het apparaat terugzetten naar de standaardinstellingen in Windows PowerShell voor StorSimple
1. Toegang tot het apparaat via de seriële console. Controleer het bannerbericht aangegeven om ervoor te zorgen dat u met verbonden bent de **Active** controller.
2. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**.
3. Bij de prompt, typ de volgende opdracht in te stellen het hele cluster, alle gegevens, metagegevens en -controller-instellingen worden verwijderd:
   
    `Reset-HcsFactoryDefault`
   
    Als u wilt herstellen in plaats daarvan één controller, gebruikt u de [Reset-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet met de `-scope` parameter.)
   
    Het systeem wordt meerdere keren opnieuw. U krijgt bericht wanneer het opnieuw instellen is voltooid. Afhankelijk van het systeemmodel duurt het 45-60 minuten voor een 8100-apparaat en 60-90 minuten voor een 8600 dit proces te voltooien.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Vragen en antwoorden over het beheren van apparaatcontrollers
In deze sectie hebben we samengevat enkele veelgestelde vragen met betrekking tot het beheren van domeincontrollers voor StorSimple-apparaat.

**V:** Wat gebeurt er als beide controllers op mijn apparaat in orde en ingeschakeld zijn op en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als beide controllers op uw apparaat in orde en ingeschakeld, u om bevestiging wordt gevraagd. U kunt kiezen om:

* **De actieve controller opnieuw** : U krijgt een melding dat het apparaat een failover uitvoeren naar de passieve controller opnieuw starten van een actieve controller veroorzaakt. De controller wordt opnieuw opgestart.
* **Een actieve controller afsluiten** : U krijgt een melding dat een actieve controller afsluiten tot downtime leidt. U moet ook de / uit-knop op het apparaat om in te schakelen op de controller te pushen.

**V:** Wat gebeurt er als de passieve controller op mijn apparaat niet beschikbaar is of is uitgeschakeld is uitgeschakeld en ik opnieuw opstarten of afsluiten van de actieve controller?

**A:** Als de passieve controller op uw apparaat is niet beschikbaar is of is uitgeschakeld uitgeschakeld en u wilt:

* **De actieve controller opnieuw** : U krijgt een melding dat u doorgaat met de bewerking in een tijdelijke onderbreking van de service resulteert, en u wordt gevraagd om bevestiging.
* **Een actieve controller afsluiten** : U krijgt een melding die u doorgaat met de bewerking resulteert in uitvaltijd. U moet ook push-knop op een of beide controllers om in te schakelen op het apparaat. U wordt gevraagd om bevestiging.

**V:** Wanneer de controller opnieuw opstarten of afsluiten doet kloktijd mislukt?

**A:** Opnieuw opstarten of afsluiten van een domeincontroller kan mislukken als:

* Een apparaatupdate wordt uitgevoerd.
* Een controller opnieuw opstarten wordt al uitgevoerd.
* Een controller is afgesloten, wordt al uitgevoerd.

**V:** Hoe kunt u achterhalen als een domeincontroller is opnieuw opgestart of afgesloten?

**A:** U kunt de status van de controller op de blade van de domeincontroller controleren. De status van de controller wordt aangegeven of een domeincontroller wordt momenteel opnieuw opstarten of afsluiten. Bovendien de **waarschuwingen** blade bevatten een informatieve waarschuwing als de controller opnieuw wordt opgestart of afgesloten. De controller opnieuw opstarten en afsluiten bewerkingen zijn ook opgenomen in de activiteitenlogboeken. Voor meer informatie over activiteitenlogboeken, gaat u naar [de activiteitenlogboeken weergeven](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**V:** Is er gevolgen voor de i/o Als gevolg van een failover van de controller?

**A:** De TCP-verbindingen tussen de initiators en actieve controller opnieuw ingesteld als gevolg van een failover van de controller, maar wordt pas gemaakt wanneer de passieve controller wordt ervan uitgegaan dat de bewerking. Mogelijk zijn er een pauze van tijdelijke (minder dan 30 seconden) in de i/o-activiteit tussen initiators en het apparaat in de loop van deze bewerking.

**V:** Hoe kan ik mijn controller voor de service nadat deze is afgesloten en verwijderd retourneren?

**A:** Als u wilt een controller terugkeren naar de service, moet u dit invoegen in het chassis zoals beschreven in [vervangen door een netwerkcontroller module op uw StorSimple-apparaat](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Volgende stappen
* Als u problemen ondervindt met uw StorSimple-apparaatcontrollers die u niet kunt oplossen met behulp van de procedures in deze zelfstudie [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

