---
title: Het Azure IoT Central-toepassings dashboard configureren | Microsoft Docs
description: Als ontwerper leert u hoe u het standaard dash board van Azure IoT Central Application kunt configureren.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 94ad51ac11687dfe060176132e2030d61b8d4ffc
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850220"
---
# <a name="configure-the-application-dashboard"></a>Het toepassings dashboard configureren

Het **dash board** is de pagina die wordt geladen wanneer gebruikers die toegang hebben tot de toepassing, naar de URL van de toepassing navigeren. Als u het voor beeld-toepassings sjabloon **Contoso** of **voorbeeld Devkits** hebt geselecteerd om uw toepassing te maken, heeft uw toepassing een vooraf gedefinieerd dash board. Als u de sjabloon voor de **aangepaste toepassings** toepassing hebt gekozen, is het dash board leeg.

> [!NOTE]
> Gebruikers kunnen ook [hun eigen persoonlijke Dash boards maken](howto-personalize-dashboard.md) om te gebruiken in plaats van het standaard toepassings dashboard.

## <a name="add-tiles"></a>Tegels toevoegen

In de volgende scherm afbeelding ziet u het dash board in een toepassing die is gemaakt op basis van de **voorbeeld sjabloon contoso** . Als u het standaard dashboard voor uw toepassing wilt aanpassen, selecteert u **bewerken** in de rechter bovenhoek van de pagina.

![Dash board voor toepassingen op basis van de sjabloon ' voor beeld Contoso '](media/howto-configure-homepage/image1a.png)

Als u **bewerken**selecteert, wordt het deel venster dash board-bibliotheek geopend. De bibliotheek bevat de tegels en dashboard primitieven die u kunt gebruiken om het dash board aan te passen.

![Dashboard bibliotheek](media/howto-configure-homepage/image2a.png)

U kunt bijvoorbeeld een tegel met **Apparaatinstellingen en eigenschappen** toevoegen om een selectie van de huidige instellingen en eigenschappen voor een apparaat weer te geven. Hiertoe selecteert u eerst een **apparaataccount** en selecteert u vervolgens een **apparaatinstantie.** Daarna geeft u de tegel een titel en selecteert u een **instelling** of **eigenschap** om weer te geven. Op de volgende scherm afbeelding ziet u de instellingen en eigenschappen die zijn geselecteerd om aan de tegel toe te voegen. Selecteer **gereed** om de wijziging op te slaan in het dash board.

![Formulier Details van apparaat configureren met Details voor instellingen en eigenschappen](media/howto-configure-homepage/image3a.png)

Wanneer een operator nu het standaard toepassings dashboard weergeeft, zien ze de nieuwe tegel met de instelling **temperatuur instellen** voor het apparaat:

![Tabblad dash board met weer gegeven instellingen en eigenschappen voor de tegel](media/howto-configure-homepage/image4a.png)

U kunt andere tegel typen in de bibliotheek verkennen om te ontdekken hoe u het standaard Application dash board kunt aanpassen.

Zie [Dashboard tegels gebruiken](howto-use-tiles.md)voor meer informatie over het gebruik van tegels in azure IOT Central.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u uw Azure IoT Central standaard toepassings dashboard configureert, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Meer informatie over het voorbereiden en uploaden van installatie kopieën](howto-prepare-images.md)
