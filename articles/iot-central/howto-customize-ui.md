---
title: De Azure IoT Central gebruikersinterface aanpassen | Microsoft Docs
description: Over het aanpassen van de koppelingen thema en Help-informatie voor uw Azure IoT central-toepassing
author: dominicbetts
ms.author: dobett
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0256396cd228898f3852772b113e6064a0656746
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237655"
---
# <a name="customize-the-azure-iot-central-ui"></a>De Azure IoT Central gebruikersinterface aanpassen 

*In dit artikel is van toepassing op beheerders.*

IoT Central kunt u de gebruikersinterface van uw toepassing aanpassen door aangepaste thema's toepassen en wijzigen van de help-koppelingen om te verwijzen naar uw eigen aangepaste help-resources. De volgende schermafbeelding ziet u een pagina in met behulp van het standaardthema:

![Standard IoT Central-thema](./media/howto-customize-ui/standard-ui.png)

De volgende schermafbeelding ziet u een pagina in met behulp van een aangepaste schermafbeelding met de aangepaste UI-elementen gemarkeerd:

![Aangepaste IoT Central thema](./media/howto-customize-ui/themed-ui.png)

## <a name="create-theme"></a>Een thema maken

Voor het maken van een aangepast thema, gaat u naar de **aanpassen van uw toepassing** pagina in de **beheer** sectie:

![IoT Central thema 's](./media/howto-customize-ui/themes.png)

Op deze pagina kunt u de volgende aspecten van uw toepassing aanpassen:

### <a name="application-logo"></a>Logo van toepassing

Een PNG-afbeelding niet groter zijn dan 1 MB, met een transparante achtergrond. Dit logo wordt weergegeven aan de linkerkant op de titelbalk van de IoT Central-toepassing.

Als uw logo-installatiekopie de naam van uw toepassing bevat, kunt u de tekst van de naam van toepassing verbergen. Zie voor meer informatie, [Toepassingsinstellingen beheren](./howto-administer.md#manage-application-settings).

### <a name="browser-icon-favicon"></a>Browserpictogram (favicon)

Een PNG-afbeelding niet groter zijn dan 32 x 32 pixels, met een transparante achtergrond. Een webbrowser kan deze installatiekopie in de adresbalk, geschiedenis, bladwijzers en browsertabblad gebruiken.

### <a name="browser-colors"></a>, Browserkleuren

U kunt de kleur van de paginakoptekst en de kleur die wordt gebruikt voor knoppen en andere belangrijke functies accenting wijzigen. Gebruik een waarde van de hexadecimale kleur zes teken in de indeling `##ff6347`. Voor meer informatie over **hexadecimale waarde** notatie kleur, Zie [HTML kleuren](https://www.w3schools.com/html/html_colors.asp).

> [!NOTE]
> U kunt altijd terugkeren naar de standaardopties op de **aanpassen van uw toepassing** pagina.

### <a name="changes-for-operators"></a>Wijzigingen voor operators

Als een beheerder maakt een aangepast thema, operators en andere gebruikers van uw toepassing kunnen niet meer en kies vervolgens een thema in **instellingen**.

## <a name="replace-help-links"></a>Help-koppelingen vervangen

Aangepaste help-informatie om aan te bieden uw operators en andere gebruikers, wijzigt u de koppelingen op de toepassing **Help** menu.

Als u wilt wijzigen van de help-koppelingen, gaat u naar de **help aanpassen** pagina in de **beheer** sectie:

![IoT Central help-koppelingen aanpassen](./media/howto-customize-ui/help-links.png)

U kunt ook nieuwe vermeldingen toevoegen aan het menu help en standaardvermeldingen te verwijderen:

![Aangepaste IoT Central help](./media/howto-customize-ui/custom-help.png)

> [!NOTE]
> U kunt altijd terugkeren naar de standaard help-koppelingen op de **help aanpassen** pagina.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het aanpassen van de gebruikersinterface in uw toepassing IoT Central, hier enkele Voorgestelde volgende stappen te volgen:

> [!div class="nextstepaction"]
> [Beheren van uw toepassing](./howto-administer.md)
> [configureren van het dashboard](./howto-configure-homepage.md)