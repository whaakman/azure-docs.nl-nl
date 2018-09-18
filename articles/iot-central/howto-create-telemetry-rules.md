---
title: Maken en beheren van regels voor telemetrie in uw toepassing met Azure IoT Central | Microsoft Docs
description: Azure IoT Central telemetrie regels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
author: ankitgupta
ms.author: ankitgup
ms.date: 08/14/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1b82ac3bf67370a2c39e85bf5691da38539edb74
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729305"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Maak een regel Telemetrie en meldingen in uw Azure IoT Central-toepassing instellen

*In dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

U kunt Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT Central regels zorgen ervoor dat u bewaak uw apparaten in bijna realtime en automatisch acties aanroepen, zoals een e-mail verzenden of Microsoft Flow activeren. In een paar klikken kunt u de voorwaarde waarvoor u wilt controleren van de gegevens van uw apparaat en de bijbehorende actie configureren. In dit artikel wordt uitgelegd hoe u regels maken om te controleren van de telemetrie die is verzonden door het apparaat.

Apparaten kunnen telemetrie meting gebruiken voor het verzenden van numerieke gegevens van het apparaat. Een telemetrie-regel wordt geactiveerd wanneer de telemetrie van de geselecteerde apparaten een opgegeven drempelwaarde overschrijdt.

## <a name="create-a-telemetry-rule"></a>Een telemetrie-regel maken

De apparaat-sjabloon moet ten minste één telemetrie meting gedefinieerd hebben voor het maken van een telemetrie-regel. Dit voorbeeld wordt een gekoeld Verkoopautomaat-apparaat dat de temperatuur en vochtigheid telemetrie verzendt. De regel controleert de door het apparaat gemelde temperatuur en een e-mailbericht wordt verzonden wanneer het gaat dan 80 graden.

1. Gebruik Device Explorer, Ga naar de sjabloon van het apparaat waarvoor u de regel voor het toevoegen.

1. Klik op een bestaand apparaat onder de geselecteerde sjabloon. 

    >[!TIP] 
    >Als de sjabloon niet apparaten hebt en voeg eerst een nieuw apparaat toe.

1. Als u geen regels nog gemaakt nog, ziet u het volgende scherm:

    ![Nog geen regels](media\howto-create-telemetry-rules\Rules_Landing_Page.png)

1. Op de **regels** tabblad **sjabloon bewerken** en vervolgens **+ nieuwe regel** om te zien welke typen regels die u kunt maken.

1. Klik op **telemetrie** om een regel voor het controleren van telemetrie van apparaten te maken.

    ![Regeltypen](media\howto-create-telemetry-rules\Rule_Types.png)

1. Voer een naam die u helpt bij het identificeren van de regel in de sjabloon van dit apparaat.

1. Om in te schakelen direct de regel voor alle apparaten die zijn gemaakt voor deze sjabloon, in-/ uitschakelen **regel inschakelen voor alle apparaten voor deze sjabloon**.

   ![Details van de regel](media\howto-create-telemetry-rules\Rule_Detail.png)
    
    De regel wordt automatisch toegepast op alle apparaten die onder de apparaat-sjabloon.
    

### <a name="configure-the-rule-conditions"></a>Voorwaarden van de regel configureren

Voorwaarde definieert de criteria die wordt bewaakt door de regel.

1. Klik op **+** naast **voorwaarden** om toe te voegen een nieuwe voorwaarde.

1. Selecteer de telemetrie die u controleren wilt in de **meting** vervolgkeuzelijst.

   ![Voorwaarde](media\howto-create-telemetry-rules\Aggregate_Condition_Filled_Out.png)

1. Kies vervolgens **aggregatie**, **Operator**, en geef een **drempelwaarde** waarde.
    - Aggregatie is optioneel. Zonder aggregatie, de regel wordt geactiveerd voor elk gegevenspunt telemetrie die aan de voorwaarde voldoet. Bijvoorbeeld, als de regel is geconfigureerd om te activeren wanneer temperatuur hoger is dan 80 en vervolgens de regel wordt geactiveerd vrijwel direct wanneer het apparaat temperatuur > 80 rapporteert.
    - Als een statistische functie, zoals gemiddelde, minimum, maximum, aantal gekozen vervolgens de gebruiker moet opgeven een **cumulatieve tijdvenster** over waarop de voorwaarde moet worden geëvalueerd. Als u de periode van '5 minuten' en de regel ziet er bijvoorbeeld voor de gemiddelde temperatuur hoger 80, de regel wordt geactiveerd wanneer de gemiddelde temperatuur hoger dan 80 voor ten minste vijf minuten is. De evaluatiefrequentie regel is hetzelfde als de **cumulatieve tijdvenster**, wat betekent dat, in dit voorbeeld wordt de regel is geëvalueerd om de 5 minuten.

    >[!NOTE]
    >Meer dan één telemetrie meting kan worden toegevoegd onder **voorwaarde**. Wanneer meerdere voorwaarden zijn opgegeven, moeten aan de voorwaarden worden voldaan om de regel te activeren. Elke voorwaarde wordt impliciet gekoppeld door een 'En'-component. Wanneer u statistische functie gebruikt, moet elke meting worden geaggregeerd.
    
    

### <a name="configure-actions"></a>Configureren van acties

Deze sectie leest u over het instellen van acties moet worden uitgevoerd wanneer de regel wordt geactiveerd. Acties ophalen aangeroepen wanneer de voorwaarden die zijn opgegeven in de regel in waar resulteren.

1. Kies de **+** naast **acties**. U ziet hier de lijst met beschikbare acties.  

    ![Actie toevoegen](media\howto-create-telemetry-rules\Add_Action.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste één keer hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT Central.

   ![Actie configureren](media\howto-create-telemetry-rules\Configure_Action.png)

1. Als u wilt de regel niet opslaan, kiest u **opslaan**. De regel meteen live binnen een paar minuten en start de bewaking van telemetrie wordt verzonden naar uw toepassing. Wanneer de voorwaarde die is opgegeven in de regel overeenkomt, wordt de geconfigureerde e-mailactie geactiveerd door de regel.

1. Kies **gedaan** om af te sluiten de **sjabloon bewerken** modus.

U kunt andere acties toevoegen aan de regel, zoals Microsoft Flow en webhooks. U kunt maximaal 5 acties per regel toevoegen.

- [Microsoft Flow-actie](howto-add-microsoft-flow.md) aan een werkstroom in Microsoft Flow vliegende start wanneer een regel wordt geactiveerd 
- [Webhookactie](howto-create-webhooks.md) naar andere services waarschuwen wanneer een regel wordt geactiveerd

## <a name="parameterize-the-rule"></a>De regel parameteriseren

Regels kunnen worden bepaalde waarden uit afgeleid **apparaateigenschappen** als parameters. Met behulp van parameters is handig in scenario's waarbij telemetrie drempelwaarden voor verschillende apparaten verschillen. Wanneer u de regel maakt, kiest u een apparaateigenschap waarmee de drempelwaarde, zoals **ideaal drempelwaarde**, in plaats van een absolute waarde, zoals 80 graden op te geven. Wanneer de regel wordt uitgevoerd, dit komt overeen met de telemetrie van apparaten met de waarde in de eigenschap is ingesteld.

Met behulp van parameters is een effectieve manier om te beperken van het aantal regels voor het beheren van per apparaat-sjabloon.

Acties kunnen ook worden geconfigureerd met behulp van **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een eigenschap, wordt deze kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijdert u het openen van de regel en kies **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de apparaat-sjabloon en alle bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor alle apparaten van deze sjabloon** knop in de regel wilt in- of uitschakelen van de regel voor alle apparaten die gekoppeld aan de apparaat-sjabloon zijn.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u regels maken in uw Azure IoT Central-toepassing, zijn hier enkele volgende stap:

- [Microsoft Flow-actie toevoegen in regels](howto-add-microsoft-flow.md)
- [Webhook-actie toevoegen in regels](howto-create-webhooks.md)
- [Uw apparaten beheren](howto-manage-devices.md)
