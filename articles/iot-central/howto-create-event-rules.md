---
title: Maken en beheren van gebeurtenisregels in uw Azure IoT Central-toepassing | Microsoft Docs
description: Azure IoT Central gebeurtenisregels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
author: ankitgupta
ms.author: ankitgup
ms.date: 08/14/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 889f3928ee72c035035abb635eb71ec0b06a3b45
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730150"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Maken van een regel voor gebeurtenissen en meldingen in uw Azure IoT Central-toepassing instellen

*In dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

U kunt Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT Central regels zorgen ervoor dat u bewaak uw apparaten in bijna realtime en automatisch acties aanroepen, zoals een e-mail verzenden of Microsoft Flow activeren. In een paar klikken kunt u de voorwaarde waarvoor u wilt controleren van de gegevens van uw apparaat en de bijbehorende actie configureren. In dit artikel wordt uitgelegd hoe u regels maken om te controleren gebeurtenissen die worden verzonden door het apparaat.

Apparaten kunnen gebeurtenis meting gebruiken voor het verzenden van voor belangrijke of informatieve apparaatgebeurtenissen. Een regel voor de gebeurtenis wordt geactiveerd wanneer de gebeurtenis van het geselecteerde apparaat is gemeld door het apparaat.

## <a name="create-an-event-rule"></a>Een gebeurtenisregel maken

De apparaat-sjabloon moet ten minste één gebeurtenis meting gedefinieerd hebben voor het maken van een regel voor gebeurtenissen. Dit voorbeeld wordt een gekoeld Verkoopautomaat-apparaat dat een ventilator motor is een foutgebeurtenis gemeld. De regel controleert de gebeurtenis die is gerapporteerd door het apparaat en een e-mail verzenden zodra de gebeurtenis wordt gerapporteerd.

1. Gebruik Device Explorer, Ga naar de sjabloon van het apparaat waarvoor u de regel voor het toevoegen.

1. Klik op een bestaand apparaat onder de geselecteerde sjabloon. 

    >[!TIP] 
    >Als de sjabloon niet apparaten hebt en voeg eerst een nieuw apparaat toe.

1. Als u dit nog niet hebt nog geen regels gemaakt, ziet u het volgende scherm:

    ![Nog geen regels](media\howto-create-event-rules\Rules_Landing_Page.png)


1. Op de **regels** tabblad **sjabloon bewerken** en vervolgens **+ nieuwe regel** om te zien welke typen regels die u kunt maken.


1. Klik op de **gebeurtenis** tegel om een gebeurtenis bewaking van de regel te maken.

    ![Regeltypen](media\howto-create-event-rules\Rule_Types.png)

    
1. Voer een naam die u helpt bij het identificeren van de regel in de sjabloon van dit apparaat.

1. Om in te schakelen direct de regel voor alle apparaten die zijn gemaakt met deze sjabloon, in-/ uitschakelen **regel inschakelen voor alle apparaten voor deze sjabloon**.

    ![Details van de regel](media\howto-create-event-rules\Rule_Detail.png)

    De regel wordt automatisch toegepast op alle apparaten die onder de apparaat-sjabloon.

### <a name="configure-the-rule-conditions"></a>Voorwaarden van de regel configureren

Voorwaarde definieert de criteria die wordt bewaakt door de regel.

1. Kies de **+** naast **voorwaarden** om toe te voegen een nieuwe voorwaarde.

1. Kies de gebeurtenis die u wilt bewaken in de vervolgkeuzelijst meting. In dit voorbeeld **ventilator Motor fout** gebeurtenis is geselecteerd.

   ![Voorwaarde](media\howto-create-event-rules\Condition_Filled_Out.png) 


1. (Optioneel) u kunt ook instellen **aantal** als **aggregatie** en de bijbehorende drempelwaarde opgeven.

    - Zonder aggregatie, de regel wordt geactiveerd voor elk gegevenspunt gebeurtenis die aan de voorwaarde voldoet. Bijvoorbeeld, als u een voorwaarde om te activeren wanneer een gebeurtenis '-ventilator Motor fout' van de regel configureert wordt vervolgens de regel geactiveerd bijna onmiddellijk wanneer het apparaat dat de gebeurtenis meldt.
    - Als het aantal wordt gebruikt als een statistische functie, hebt u voor een **drempelwaarde** en een **cumulatieve tijdvenster** over waarop de voorwaarde moet worden geëvalueerd. In dit geval wordt het aantal gebeurtenissen is samengevoegd en de regel wordt geactiveerd als het aantal verzamelde gebeurtenissen komt overeen met de drempelwaarde.
 
    Bijvoorbeeld, als u waarschuwen wilt wanneer er meer dan drie apparaatgebeurtenissen binnen 5 minuten, selecteert u de gebeurtenis en de statistische functie "aantal", operator, als "groter is dan" en "drempelwaarde' als 3. Stel 'Aggregatie periode' van '5 minuten'. De regel wordt geactiveerd wanneer er meer dan drie gebeurtenissen worden verzonden door het apparaat binnen 5 minuten. De evaluatiefrequentie regel is hetzelfde als de **cumulatieve tijdvenster**, wat betekent dat, in dit voorbeeld wordt de regel is geëvalueerd om de 5 minuten. 

    ![Gebeurtenis voorwaarde toevoegen](media\howto-create-event-rules\Aggregate_Condition_Filled_Out.png)

    >[!NOTE] 
    >Meer dan één gebeurtenis meting kan worden toegevoegd onder **voorwaarde**. Wanneer meerdere voorwaarden zijn opgegeven, moeten aan de voorwaarden worden voldaan om de regel te activeren. Elke voorwaarde wordt impliciet gekoppeld door een 'En'-component. Wanneer u statistische functie gebruikt, moet elke meting worden geaggregeerd.

### <a name="configure-actions"></a>Configureren van acties

Deze sectie leest u over het instellen van acties moet worden uitgevoerd wanneer de regel wordt geactiveerd. Acties ophalen aangeroepen wanneer de voorwaarden die zijn opgegeven in de regel in waar resulteren.

1. Kies de **+** naast **acties**. U ziet hier de lijst met beschikbare acties. 

    ![Actie toevoegen](media\howto-create-event-rules\Add_Action.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste één keer hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT Central.

   ![Actie configureren](media\howto-create-event-rules\Configure_Action.png)

1. Als u wilt de regel niet opslaan, kiest u **opslaan**. De regel meteen live binnen een paar minuten en start de bewaking van de gebeurtenissen worden verzonden naar uw toepassing. Wanneer de voorwaarde die is opgegeven in de regel overeenkomt, wordt de geconfigureerde e-mailactie geactiveerd door de regel.

1. Kies **gedaan** om af te sluiten de **sjabloon bewerken** modus.

U kunt andere acties toevoegen aan de regel, zoals Microsoft Flow en webhooks. U kunt maximaal 5 acties per regel toevoegen.

- [Microsoft Flow-actie](howto-add-microsoft-flow.md) aan een werkstroom in Microsoft Flow vliegende start wanneer een regel wordt geactiveerd 
- [Webhookactie](howto-create-webhooks.md) naar andere services waarschuwen wanneer een regel wordt geactiveerd

## <a name="parameterize-the-rule"></a>De regel parameteriseren

Acties kunnen ook worden geconfigureerd met behulp van **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een apparaateigenschap, wordt deze kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijdert u het openen van de regel en kies **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de apparaat-sjabloon en alle bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor alle apparaten van deze sjabloon** knop inschakelen of uitschakelen van de regel voor alle apparaten die gekoppeld aan de apparaat-sjabloon zijn.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u regels maken in uw Azure IoT Central-toepassing, zijn hier enkele volgende stap:

- [Microsoft Flow-actie toevoegen in regels](howto-add-microsoft-flow.md)
- [Webhook-actie toevoegen in regels](howto-create-webhooks.md)
- [Uw apparaten beheren](howto-manage-devices.md)
