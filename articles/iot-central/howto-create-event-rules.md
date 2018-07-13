---
title: Maken en beheren van gebeurtenisregels in uw Azure IoT Central-toepassing | Microsoft Docs
description: Azure IoT Central gebeurtenisregels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: ede7748b1471136cf792c2b30b7c90e12b0b274a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006845"
---
# <a name="create-an-event-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Maken van een regel voor gebeurtenissen en een actie in uw Azure IoT Central-toepassing instellen

U kunt Microsoft Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT Central regels zorgen ervoor dat u uw apparaten in bijna realtime controleren en automatisch aanroepen van acties zoals het verzenden van een e-mail of werkstroom in Microsoft Flow wordt geactiveerd wanneer aan de voorwaarden van de regel wordt voldaan. In een paar klikken kunt u de voorwaarde om te controleren van de gegevens van uw apparaat en configureer de actie om aan te roepen. In dit artikel wordt de gebeurtenis bewakingsregel in detail uitgelegd.

Maakt gebruik van Azure IoT Central [gebeurtenis meting](howto-set-up-template.md) apparaatgegevens vast te leggen. Elk type meting heeft een belangrijke kenmerken die de meting definieert. U kunt regels voor het bewaken van elk type apparaat meting en waarschuwingen genereren wanneer de regel wordt geactiveerd. Een regel voor de gebeurtenis wordt geactiveerd wanneer de gebeurtenis van het geselecteerde apparaat is gemeld door het apparaat.

## <a name="create-an-event-rule"></a>Een gebeurtenisregel maken

Deze sectie leest u hoe u een gebeurtenis-regel maakt. Dit voorbeeld wordt een apparaat gekoeld Verkoopautomaat die rapporten ventilator motor is een foutgebeurtenis. De regel controleert de gebeurtenis die is gerapporteerd door het apparaat en een e-mail verzenden zodra de gebeurtenis wordt gerapporteerd.

1. Navigeer naar de detailpagina van het apparaat voor het apparaat dat u de regel wilt toevoegen.

1. Als u dit nog niet hebt nog geen regels gemaakt, ziet u het volgende scherm:

    ![Nog geen regels](media/howto-create-event-rules/image1.png)

1. Op de **regels** tabblad **+ nieuwe regel** om te zien welke typen regels die u kunt maken.

    ![Regeltypen](media/howto-create-event-rules/image2.png)

1. Klik op **gebeurtenis** om het formulier voor het maken van de regel te openen.

    ![Regel voor gebeurtenissen](media/howto-create-event-rules/image3.png)

1. Kies een naam die u helpt bij het identificeren van de regel in de sjabloon van dit apparaat.

1. Om in te schakelen direct de regel voor alle apparaten die zijn gemaakt met deze sjabloon, in-/ uitschakelen **regel inschakelen**.

### <a name="configure-the-rule-condition"></a>De regelvoorwaarde configureren

Deze sectie leest u hoe u een voorwaarde voor het bewaken van de meting ventilator motor fout gebeurtenis toevoegen.

1. Kies de **+** naast **voorwaarde**.

1. Kies de meting van de gebeurtenis in de vervolgkeuzelijst die u wilt bewaken. In dit voorbeeld **ventilator Motor fout** gebeurtenis is geselecteerd.

1. U kunt eventueel ook een waarde opgeven voor het geval u wilt bewaken van een specifieke waarde van de gebeurtenis wordt gerapporteerd door het apparaat. Bijvoorbeeld als het apparaat rapporteert dezelfde gebeurtenis met verschillende foutcodes geven vervolgens de foutcode als een waarde in de voorwaarde van de regel zorgt ervoor dat de regel wordt geactiveerd wanneer het apparaat die specifieke waarde als de nettolading verzendt. Dit leeg laat, betekent dat dat de regel wordt geactiveerd wanneer het apparaat de gebeurtenis, ongeacht de waarde van de gebeurtenis verzendt.

    ![Gebeurtenis voorwaarde toevoegen](media/howto-create-event-rules/image4.png)

    > [!NOTE]
    > U moet ten minste één gebeurtenis meting selecteren bij het definiëren van een gebeurtenis regelvoorwaarde.

1. Klik op **opslaan** uw regel op te slaan. De regel meteen live binnen een paar minuten en start de bewaking van gebeurtenissen naar uw toepassing worden verzonden.

### <a name="add-an-action"></a>Een actie toevoegen

Deze sectie leest u hoe u een actie toevoegen aan een regel. Dit laat zien hoe u de e-mailactie toevoegen, maar u kunt ook [toevoegen van een Microsoft Flow-actie](howto-add-microsoft-flow.md) op de regel moet een vliegende start een werkstroom in Microsoft Flow wanneer de regel wordt geactiveerd.

> [!NOTE]
> Alleen 1 actie kan worden gekoppeld aan een enkele regel op dit moment.

1. Kies de **+** naast **acties**. U ziet hier de lijst met beschikbare acties.

    ![Actie toevoegen](media/howto-create-event-rules/image5.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste één keer hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT Central.

   ![Actie configureren](media/howto-create-event-rules/image6.png)

1. Klik op **Opslaan**. De regel meteen live binnen een paar minuten en start de bewaking van de gebeurtenissen worden verzonden naar uw toepassing. Wanneer de voorwaarde die is opgegeven in de regel overeenkomt, wordt de geconfigureerde e-mailactie geactiveerd door de regel.

## <a name="parameterize-the-rule"></a>De regel parameteriseren

Acties kunnen ook worden geconfigureerd met behulp van **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een apparaateigenschap, wordt deze kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijdert u het openen van de regel en kies **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de apparaat-sjabloon en alle bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Bij het omschakelen van de **regel inschakelen voor alle apparaten van deze sjabloon** knop in de regel Hiermee schakelt u of de regel voor alle apparaten die zijn gekoppeld aan de apparaat-sjabloon.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u regels maken in uw Azure IoT Central-toepassing, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Een Microsoft Flow-actie toevoegen aan een regel](howto-add-microsoft-flow.md)
> [over het beheren van uw apparaten](howto-manage-devices.md).
