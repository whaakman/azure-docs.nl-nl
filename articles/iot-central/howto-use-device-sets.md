---
title: Hiermee stelt u apparaat gebruiken in uw Azure IoT centrale toepassing | Microsoft Docs
description: Als operator stelt u het apparaat kunnen gebruiken in uw Azure IoT centrale toepassing.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: ef1fa64a276926a35dbf98646317bfe29200bb22
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261675"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Apparaatsets te gebruiken in uw Azure IoT centrale toepassing

Dit artikel wordt beschreven hoe, als een operator voor het gebruik van apparaat wordt ingesteld in uw Microsoft Azure IoT centrale toepassing.

Een apparaatset is een lijst met apparaten die samen worden gegroepeerd, omdat ze allemaal aan bepaalde opgegeven criteria voldoen. Apparaat wordt ingesteld voor hulp bij het beheren, visualiseren en analyseren van apparaten op schaal door apparaten groeperen in kleinere, logische groepen. Bijvoorbeeld, maken u een lijst met alle airconditioner apparaten in Haarlem inschakelen van de technicus Seattle gezocht naar alle apparaten waarvoor ze verantwoordelijk is. In dit artikel leest u hoe maken en configureren van Apparaatsets.

## <a name="create-a-device-set"></a>Een apparaatset met maken

Maken van een set apparaten:

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Klik op **+ New**.

    ![Nieuw apparaat instellen](media/howto-use-device-sets/image1.png)

1. Geef uw set apparaten een naam die is uniek voor de gehele toepassing. U kunt ook een beschrijving toevoegen. Een apparaatset kan alleen apparaten uit een sjabloon enkel apparaat bevatten. Kies het apparaat te gebruiken sjabloon voor deze set.

1. Maken van de query voor het identificeren van de apparaten voor het apparaat instellen door een eigenschap, een vergelijkingsoperator en een waarde te selecteren. U kunt meerdere query's en apparaten die voldoen aan toevoegen **alle** de criteria in de apparaatset worden geplaatst. De apparaatset die u maakt, is toegankelijk voor iedereen met toegang tot de toepassing, zodat iedereen kunt weergeven, wijzigen of verwijderen van de set apparaten.

    ![Apparaat set-query](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > De apparaatset is een dynamische query. Telkens wanneer u de lijst van apparaten bekijken, kunnen er verschillende apparaten in de lijst. De lijst is afhankelijk van welke apparaten momenteel voldoen aan de criteria van de query.

1. Kies **Opslaan**.

## <a name="configure-the-dashboard-for-your-device-set"></a>Het Dashboard voor het instellen van uw apparaat configureren

Nadat u uw apparaatset hebt gemaakt, kunt u de **Dashboard**. De **Dashboard** is de startpagina waarin u afbeeldingen en koppelingen kunt plaatsen. U kunt ook rasters die lijst van de apparaten in de apparaatset toevoegen.

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Kies de **Dashboard** tabblad.

1. Schakel **ontwerpmodus**.

    ![Ontwerpmodus op](media/howto-use-device-sets/image3.png)

1. Zie voor meer informatie over het toevoegen van een installatiekopie van een [installatiekopieën van het voorbereiden en uploaden naar uw Azure IoT centrale toepassing](howto-prepare-images.md).

1. Een tegel koppeling toevoegen:
    1. Kies **koppeling** in het rechterdeelvenster.

        ![Kies koppeling](media/howto-use-device-sets/image6.png)

    1. De koppeling geven een **titel**.
    1. Kies een URL moet worden geopend wanneer de koppeling wordt geklikt.
    1. De koppeling geven een beschrijving op die hieronder toont de **titel**.
    1. Kies **Opslaan**.

        ![Koppeling opslaan](media/howto-use-device-sets/image7.png)

    1. U kunt verplaatsen en op de tegel koppeling vergroten of verkleinen de **Dashboard**.

1. Voeg een raster. Een raster is een tabel van apparaten in het apparaat instellen met de geselecteerde kolommen.
    1. Kies **raster** in het rechterdeelvenster.

        ![Kies raster](media/howto-use-device-sets/image8.png)

    1. Geef het raster een **titel**.
    1. Selecteer de kolommen moet worden weergegeven met de knop instellingen. Kies de kolom die u wilt weergeven en kies de pijl naar rechts om deze te selecteren in het deelvenster die wordt weergegeven.
    1. Kies **OK**.
    1. Kies **Opslaan**.

        ![Raster opslaan](media/howto-use-device-sets/image9.png)

    1. Slepen en neerzetten van het raster om het te plaatsen op de **Dashboard**.

    > [!NOTE]
    > U kunt meerdere installatiekopieën, koppelingen en rasters toevoegen.
  
    1. Schakel **ontwerpmodus**.

    ![Ontwerpmodus uitschakelen](media/howto-use-device-sets/image10.png)


### <a name="configuring-location-map-in-your-device-sets-dashboard"></a>Dashboard instellen locatiekaart configureren in uw apparaat 
U kunt toevoegen om een locatiekaart voor het visualiseren van locatie van uw apparaten worden ingesteld in een Map. 

Om te kunnen toevoegen aan een locatiekaart naar apparaat u stelt dashboard hebt u geconfigureerde locatie-eigenschap in de sjabloon voor het apparaat, Zie [maakt u een locatie-eigenschap aangedreven door Azure Maps](howto-set-up-template.md).


1. Selecteer op het apparaat Sets Dashboard kaart in de bibliotheek. 

    ![Dashboard Maps wordt ingesteld voor apparaat](media/howto-use-device-sets/LocationMaps1.png)


2. Geef een titel en kies de locatie-eigenschap die u eerder hebt geconfigureerd als onderdeel van de eigenschap van uw apparaat.

    ![Dashboard Maps configureren](media/howto-use-device-sets/LocationMaps2.png)

3. Opslaan en u ziet de kaart tegel om de locatie van uw apparaten weer te geven in het apparaat ingesteld.

    ![Dashboard Maps opslaan](media/howto-use-device-sets/LocationMaps3.png)


5. U hebt geconfigureerd met inbegrip van de locatie zijn toegewezen aan de apparaten locatie in een oogopslag visualiseren nu als een operator bekijkt het apparaat sets dashboard, ze alle tegels zien kan.

    ![Maps-Operator dashboardweergave](media/howto-use-device-sets/LocationMaps4.png)

    U kunt zich vergroten of verkleinen van de kaart aan de gewenste grootte.




## <a name="configure-the-list-for-your-device-set"></a>De lijst voor het instellen van uw apparaat configureren

Nadat u uw apparaatset hebt gemaakt, kunt u de **lijst**. De **lijst** bevat alle apparaten in het apparaat instellen in een tabel met de geselecteerde kolommen.

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Kies de **lijst** tabblad.

1. Kies **kolomopties**.

    ![Kolomopties voor](media/howto-use-device-sets/image11.png)

1. Kies de kolommen moet worden weergegeven door het selecteren van de kolom die u wilt weergeven en vervolgens de pijl naar rechts om deze te selecteren.

    ![Kies kolom](media/howto-use-device-sets/image12.png)

1. Kies **OK**.

## <a name="analytics"></a>Analyse

De analyses in apparaatsets is hetzelfde als het belangrijkste analytics-tabblad in het navigatiemenu links. Voor meer informatie over analytics in het artikel op [analytics maken](howto-create-analytics.md).

## <a name="next-steps"></a>Volgende stappen

U kunt het gebruik van sets van apparaten in uw Azure IoT centrale toepassing hebt geleerd, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Telemetrie-regels maken](howto-create-telemetry-rules.md)
