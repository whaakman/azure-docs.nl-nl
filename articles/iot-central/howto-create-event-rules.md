---
title: Maken en beheren van gebeurtenisregels in uw Azure IoT centrale toepassing | Microsoft Docs
description: Azure IoT centraal gebeurtenisregels kunt u uw apparaten in bijna realtime controleren en om aan te roepen automatisch acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd.
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 30223fdca9d848ddc407981bf4a3ca683a10575a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628365"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Maken van een regel voor gebeurtenissen en meldingen in uw Azure IoT centrale toepassing instellen

U kunt Microsoft Azure IoT Central gebruiken voor het bewaken van uw verbonden apparaten op afstand. Azure IoT centraal regels kunt u uw apparaten in bijna realtime controleren en om aan te roepen automatisch acties, zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd. In een paar klikken kunt u de voorwaarde voor het bewaken van de apparaatgegevens van uw en het configureren van de actie aan te roepen. Dit artikel wordt de gebeurtenis monitoringregel in detail uitgelegd.

Maakt gebruik van Azure IoT centraal [gebeurtenis meting](howto-set-up-template.md) om vast te leggen apparaatgegevens. Elk type meting heeft sleutelkenmerken waarmee de meting. Kunt u regels voor het controleren van elk type apparaat meting en waarschuwingen genereren wanneer de regel wordt geactiveerd. Een regel voor de gebeurtenis wordt geactiveerd wanneer de gebeurtenis geselecteerde apparaat is gemeld door het apparaat.

## <a name="create-an-event-rule"></a>Een gebeurtenisregel maken

In deze sectie wordt beschreven hoe u een regel voor gebeurtenis maken. Dit voorbeeld wordt een gekoeld snoep-machine-apparaat dat foutgebeurtenis rapporten ventilator motor. De regel wordt de gebeurtenis die is gemeld door het apparaat wordt bewaakt en verzendt een e-mail wanneer de gebeurtenis wordt gerapporteerd.

1. Navigeer naar de detailpagina apparaat voor het apparaat dat u de regel wilt toevoegen.

1. Als u alle regels nog niet hebt gemaakt, ziet u het volgende scherm:

    ![Nog geen regels](media\howto-create-event-rules\image1.png)

1. Op de **regels** Kies **+ een nieuwe regel** om te zien welke typen regels die u kunt maken.

    ![Regeltypen](media\howto-create-event-rules\image2.png)

1. Klik op **gebeurtenis** het formulier voor het maken van de regel te openen.

    ![Gebeurtenis-regel](media\howto-create-event-rules\image3.png)

1. Kies een naam die u helpt bij het identificeren van de regel in de sjabloon voor dit apparaat.

1. Om onmiddellijk de regel voor alle apparaten die zijn gemaakt met deze sjabloon, in-of uitschakelen **inschakelen regel**.

### <a name="configure-the-rule-condition"></a>De regelvoorwaarde configureren

Deze sectie wordt beschreven hoe u een voorwaarde voor het bewaken van de meting ventilator motor fout gebeurtenis toevoegt.

1. Kies de **+** naast **voorwaarde**.

1. De meting gebeurtenis kiezen uit de vervolgkeuzelijst die u wilt bewaken. In dit voorbeeld **ventilator Motor fout** gebeurtenis is geselecteerd.

1. U kunt eventueel ook een waarde opgeven voor het geval u wilt bewaken van een specifieke waarde van de gebeurtenis die is gemeld door het apparaat. Bijvoorbeeld als het apparaat rapporteert dezelfde gebeurtenis met verschillende foutcodes geven vervolgens de foutcode als een waarde in de voorwaarde van de regel zorgt ervoor dat de triggers regel alleen wanneer het apparaat deze specifieke waarde als de nettolading verzendt. Dit leeg laat, betekent dat dat de regel wordt geactiveerd wanneer het apparaat de gebeurtenis ongeacht de waarde van de gebeurtenis verzendt.

    ![Gebeurtenis voorwaarde toevoegen](media\howto-create-event-rules\image4.png)

    > [!NOTE]
    > U moet ten minste één gebeurtenis meting selecteren bij het definiëren van een regelvoorwaarde gebeurtenis.

### <a name="configure-the-action"></a>De actie configureren

Deze sectie leest u hoe om op te geven wat de regel doet wanneer de voorwaarde voldoet aan door een actie toe te voegen.

1. Kies de **+** naast **acties**. Hier kunt u de lijst met beschikbare acties bekijken. Tijdens de openbare preview **e** is de enige ondersteunde actie.

    ![Actie toevoegen](media\howto-create-event-rules\image5.png)

1. Kies de **e** actie, voer een geldig e-mailadres in de **naar** veld en geeft u een opmerking in de hoofdtekst van het e-mailbericht moet worden weergegeven wanneer de regel wordt geactiveerd.

    > [!NOTE]
    > E-mailberichten worden alleen verzonden naar de gebruikers die zijn toegevoegd aan de toepassing en ten minste eenmaal hebben aangemeld. Meer informatie over [Gebruikersbeheer](howto-administer.md) in Azure IoT centrale.

   ![Actie configureren](media\howto-create-event-rules\image6.png)

1. Kies voor het opslaan van de regel **opslaan**. De regel gaat live binnen een paar minuten en start de bewaking van de gebeurtenissen die worden verzonden naar uw toepassing. Als de voorwaarde die is opgegeven in de regel overeenkomt met, activeert de regel de geconfigureerde e-actie.

## <a name="parameterize-the-rule"></a>Parameter van de regel

Acties kunnen ook worden geconfigureerd met **Apparaateigenschap** als parameter. Als een e-mailadres wordt opgeslagen als een apparaateigenschap, dan kan worden gebruikt bij het definiëren van de **naar** adres.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als u een regel niet meer nodig hebt, verwijderen door de regel te openen en te kiezen **verwijderen**. De regel wordt verwijderd, wordt deze verwijderd uit de sjabloon van het apparaat en de bijbehorende apparaten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>In- of uitschakelen van een regel voor een apparaat-sjabloon

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Uitschakelen van de **regel inschakelen voor alle apparaten van deze sjabloon** knop in de regel Hiermee schakelt u of de regel voor alle apparaten die zijn gekoppeld aan de sjabloon van het apparaat.

## <a name="enable-or-disable-a-rule-for-a-device"></a>In- of uitschakelen van een regel voor een apparaat

Navigeer naar het apparaat en kies de regel die u wilt in- of uitschakelen. Schakelen tussen de **regel inschakelen voor dit apparaat** om de in- of uitschakelen van de regel voor dat apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u regels maken in uw Azure IoT centrale toepassing, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw apparaten beheren](howto-manage-devices.md).