---
title: Maken en beheren van telemetrie regels in uw Azure IoT centrale toepassing | Microsoft Docs
description: Azure IoT centraal telemetrie regels kunt u uw apparaten in bijna realtime controleren en om aan te roepen automatisch acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 47497fbda90ecb6ebe5b5a8675069a7fb262a3c6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Maak een regel Telemetrie en meldingen in uw Azure IoT centrale toepassing instellen

U kunt Microsoft Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT centraal regels kunt u uw apparaten in bijna realtime controleren en om aan te roepen automatisch acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd. In een paar klikken kunt u de voorwaarde voor het bewaken van de apparaatgegevens van uw en het configureren van de actie aan te roepen. Dit artikel wordt de regel telemetrie in detail uitgelegd.

Maakt gebruik van Azure IoT centraal [telemetrie metingen](howto-set-up-template.md) om vast te leggen apparaatgegevens. Elk type meting heeft sleutelkenmerken waarmee de meting. Kunt u regels voor het controleren van elk type apparaat meting en waarschuwingen genereren wanneer de regel wordt geactiveerd. Een regel telemetrie wordt geactiveerd wanneer de telemetrie van het geselecteerde apparaat een opgegeven drempelwaarde overschrijdt.

## <a name="create-a-telemetry-rule"></a>Maak een regel telemetrie

In deze sectie wordt beschreven hoe u een regel telemetrie maken. In dit voorbeeld wordt een verbonden airconditioner-apparaat dat de temperatuur en vochtigheid telemetrie verzendt. De regel controleert de temperatuur gemeld door het apparaat en een e-mailbericht wordt verzonden wanneer deze hoger dan 80 graden is.

1. Navigeer naar de detailpagina apparaat voor het apparaat dat u de regel wilt toevoegen.

1. Als u alle regels nog niet hebt gemaakt, ziet u het volgende scherm:

    ![Nog geen regels](media\howto-create-telemetry-rules\image1.png)

1. Op de **regels** Kies **+ een nieuwe regel** om te zien welke typen regels die u kunt maken.

    ![Regeltypen](media\howto-create-telemetry-rules\image2.png)

1. Kies de **telemetrie** tegel om het formulier voor het maken van de regel te openen.

    ![Telemetrie-regel](media\howto-create-telemetry-rules\image3.png)

1. Kies een naam die u helpt bij het identificeren van de regel in de sjabloon voor dit apparaat.

1. Om onmiddellijk de regel voor alle apparaten die zijn gemaakt met deze sjabloon, in-of uitschakelen **inschakelen regel**.

### <a name="configure-the-rule-condition"></a>De regelvoorwaarde configureren

Deze sectie wordt beschreven hoe u een voorwaarde voor het bewaken van de telemetrie temperatuur toevoegen.

1. Kies de **+** naast **voorwaarde**.

1. Kies de **temperatuur** type telemetrie uit de vervolgkeuzelijst. Kies de operator en een drempelwaarde opgeven. U kunt meerdere telemetrie-voorwaarden toevoegen. Wanneer meerdere voorwaarden zijn opgegeven, moeten aan de voorwaarden worden voldaan om de regel voor het activeren van.

    ![Telemetrie-voorwaarde toevoegen](media\howto-create-telemetry-rules\image4.png)

    > [!NOTE]
    > Selecteer ten minste één telemetrie meting wanneer u een regelvoorwaarde telemetrie definieert.

### <a name="configure-the-action"></a>De actie configureren

Deze sectie leest u hoe om op te geven wat de regel doet wanneer de voorwaarde voldoet aan door een actie toe te voegen.

1. Kies de **+** naast **acties**. Hier kunt u de lijst met beschikbare acties bekijken. Tijdens de openbare preview **e** is de enige ondersteunde actie.

    ![Actie toevoegen](media\howto-create-telemetry-rules\image5.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste eenmaal hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT centrale.

   ![Actie configureren](media\howto-create-telemetry-rules\image6.png)

1. Kies voor het opslaan van de regel **opslaan**. De regel gaat live binnen een paar minuten en start de bewaking van telemetrie naar uw toepassing wordt verzonden. Als de voorwaarde die is opgegeven in de regel overeenkomt met, activeert de regel de geconfigureerde e-actie.

## <a name="parameterize-the-rule"></a>Parameter van de regel

Regels kunnen bepaalde waarden van worden afgeleid **apparaateigenschappen** als parameters. Het gebruik van parameters is nuttig in scenario's waarbij telemetrie drempelwaarden voor verschillende apparaten verschillen. Wanneer u de regel maakt, kiest u een apparaateigenschap die de drempelwaarde, zoals specificeert **Maximum ideaal drempelwaarde**, in plaats van een absolute waarde, zoals 80 graden bieden. Wanneer de regel wordt uitgevoerd, overeenkomt met de apparaattelemetrie met de waarde die is opgegeven in de apparaateigenschap.

Met behulp van parameters is een effectieve manier om te beperken het aantal regels voor het beheren van per apparaat-sjabloon.

Acties kunnen ook worden geconfigureerd met **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een apparaateigenschap, dan kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijderen door de regel te openen en te kiezen **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de sjabloon van het apparaat en de bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Uitschakelen van de **regel inschakelen voor alle apparaten van deze sjabloon** knop in de regel Hiermee schakelt u of de regel voor alle apparaten die zijn gekoppeld aan de sjabloon van het apparaat.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe regels in uw Azure IoT centrale toepassing bewerken, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw apparaten beheren](howto-manage-devices.md).