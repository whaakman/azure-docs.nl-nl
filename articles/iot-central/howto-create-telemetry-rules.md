---
title: Maken en beheren van regels voor telemetrie in uw toepassing met Azure IoT Central | Microsoft Docs
description: Azure IoT Central telemetrie regels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 42516e4dd6a85e0d07d4a8e70e958b2ec6e84aad
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225197"
---
# <a name="create-a-telemetry-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Maak een regel Telemetrie en een actie in uw Azure IoT Central-toepassing instellen

U kunt Microsoft Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT Central regels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties zoals het verzenden van een e-mail of werkstroom in Microsoft Flow wordt geactiveerd wanneer aan de voorwaarden van de regel wordt voldaan. In een paar klikken kunt u de voorwaarden om te controleren van de gegevens van uw apparaat en configureer de actie om aan te roepen. In dit artikel wordt de regel telemetrie in detail uitgelegd.

Maakt gebruik van Azure IoT Central [telemetrie metingen](howto-set-up-template.md) apparaatgegevens vast te leggen. Elk type meting heeft een belangrijke kenmerken die de meting definieert. U kunt regels voor het bewaken van elk type apparaat meting en waarschuwingen genereren wanneer de regel wordt geactiveerd. Een telemetrie-regel wordt geactiveerd wanneer de telemetrie van de geselecteerde apparaten een opgegeven drempelwaarde overschrijdt.

## <a name="create-a-telemetry-rule"></a>Een telemetrie-regel maken

Deze sectie leest u hoe u een telemetrie-regel maakt. Dit voorbeeld wordt een verbonden airconditioner-apparaat dat de temperatuur en vochtigheid telemetrie verzendt. De regel controleert de door het apparaat gemelde temperatuur en een e-mailbericht wordt verzonden wanneer het gaat dan 80 graden.

1. Navigeer naar de detailpagina van het apparaat voor het apparaat dat u de regel wilt toevoegen.

1. Als u dit nog niet hebt nog geen regels gemaakt, ziet u het volgende scherm:

    ![Nog geen regels](media/howto-create-telemetry-rules/image1.png)

1. Op de **regels** tabblad **+ nieuwe regel** om te zien welke typen regels die u kunt maken.

    ![Regeltypen](media/howto-create-telemetry-rules/image2.png)

1. Kies de **telemetrie** tegel om het formulier voor het maken van de regel te openen.

    ![Telemetrie-regel](media/howto-create-telemetry-rules/image3.png)

1. Kies een naam die u helpt bij het identificeren van de regel in de sjabloon van dit apparaat.

1. Om in te schakelen direct de regel voor alle apparaten die zijn gemaakt met deze sjabloon, in-/ uitschakelen **regel inschakelen**.

### <a name="configure-the-rule-condition"></a>De regelvoorwaarde configureren

Deze sectie leest u hoe u een voorwaarde voor het bewaken van de temperatuurtelemetrie toevoegen.

1. Kies de **+** naast **voorwaarde**.

1. Kies de **temperatuur** telemetrietype in de vervolgkeuzelijst. Vervolgens kiest u de operator en een drempelwaarde opgeven. U kunt meerdere telemetrie-voorwaarden toevoegen. Wanneer meerdere voorwaarden zijn opgegeven, moeten aan de voorwaarden worden voldaan om de regel te activeren.

    ![Telemetrie-voorwaarde toevoegen](media/howto-create-telemetry-rules/image4.png)

    > [!NOTE]
    > Selecteer ten minste één telemetrie meting wanneer u een voorwaarde voor telemetrie regel definieert.

1. Klik op **opslaan** uw regel op te slaan. De regel meteen live binnen een paar minuten en start de bewaking van telemetrie wordt verzonden naar uw toepassing.

### <a name="add-an-action"></a>Een actie toevoegen

In dit voorbeeld ziet u hoe u een actie toevoegen aan een regel. Dit laat zien hoe u de e-mailactie toevoegen, maar u kunt ook andere acties toevoegen:
-  [Microsoft Flow-actie](howto-add-microsoft-flow.md) aan een werkstroom in Microsoft Flow vliegende start wanneer een regel wordt geactiveerd
- [Webhookactie](howto-create-webhooks.md) naar andere services waarschuwen wanneer een regel wordt geactiveerd

> [!NOTE]
> Alleen 1 actie kan worden gekoppeld aan een enkele regel op dit moment.

1. Kies de **+** naast **acties**. U ziet hier de lijst met beschikbare acties.

    ![Actie toevoegen](media/howto-create-telemetry-rules/image5.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste één keer hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT Central.

   ![Actie configureren](media/howto-create-telemetry-rules/image6.png)

1. Klik op **Opslaan**. De regel meteen live binnen een paar minuten en start de bewaking van telemetrie wordt verzonden naar uw toepassing. Wanneer de voorwaarde die is opgegeven in de regel overeenkomt, wordt de geconfigureerde e-mailactie geactiveerd door de regel.

## <a name="parameterize-the-rule"></a>De regel parameteriseren

Regels kunnen worden bepaalde waarden uit afgeleid **apparaateigenschappen** als parameters. Met behulp van parameters is handig in scenario's waarbij telemetrie drempelwaarden voor verschillende apparaten verschillen. Wanneer u de regel maakt, kiest u een apparaateigenschap waarmee de drempelwaarde, zoals **ideaal drempelwaarde**, in plaats van een absolute waarde, zoals 80 graden op te geven. Wanneer de regel wordt uitgevoerd, dit komt overeen met de telemetrie van apparaten met de waarde die is opgegeven in de apparaateigenschappen.

Met behulp van parameters is een effectieve manier om te beperken van het aantal regels voor het beheren van per apparaat-sjabloon.

Acties kunnen ook worden geconfigureerd met behulp van **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een apparaateigenschap, wordt deze kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijdert u het openen van de regel en kies **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de apparaat-sjabloon en alle bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Bij het omschakelen van de **regel inschakelen voor alle apparaten van deze sjabloon** knop in de regel Hiermee schakelt u of de regel voor alle apparaten die zijn gekoppeld aan de apparaat-sjabloon.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u de regels in uw Azure IoT Central-toepassing bewerken, hier voorgestelde volgende stappen te volgen:

> [!div class="nextstepaction"]
> [Een Microsoft Flow-actie toevoegen aan een regel](howto-add-microsoft-flow.md)
> [uw apparaten beheren](howto-manage-devices.md)
