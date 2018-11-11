---
title: Hiermee stelt u apparaten gebruikt in uw Azure IoT Central-toepassing | Microsoft Docs
description: Als operator stelt het gebruik van apparaten in uw Azure IoT Central-toepassing.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: a5dd018197ff78ff0563349fd941308c3684a456
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004116"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Hiermee stelt u apparaten gebruikt in uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe, als een operator voor het gebruik van apparaat wordt ingesteld in uw Azure IoT Central-toepassing.

Een apparaat is een lijst met apparaten die samen worden gegroepeerd, omdat ze allemaal overeenkomen met bepaalde opgegeven criteria. Apparaat Hiermee stelt u helpen te beheren, visualiseren en analyseren van apparaten op schaal door het groeperen van apparaten in kleinere, logische groepen. Bijvoorbeeld, maken u een lijst met alle airconditioner apparaten in Seattle om in te schakelen van de technicus Seattle om alle apparaten waarvoor de medewerker van de verantwoordelijk is te vinden. In dit artikel wordt beschreven hoe u maken en configureren van Apparaatsets.

## <a name="create-a-device-set"></a>Maken van een apparaat instellen

Voor het maken van een apparaat instellen:

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Klik op **+ New**.

    ![Nieuw apparaat instellen](media/howto-use-device-sets/image1.png)

1. Geeft de apparaat-verzameling een naam die uniek is voor de gehele toepassing. U kunt ook een beschrijving toevoegen. Het instellen van een apparaat kan alleen apparaten uit de sjabloon voor een enkel apparaat bevatten. Kies de sjabloon apparaat moet worden gebruikt voor deze verzameling.

1. De query maken om te bepalen welke apparaten er voor het apparaat instellen door een eigenschap, een vergelijkingsoperator en een waarde te selecteren. U kunt meerdere query's en apparaten die voldoen aan toevoegen **alle** de criteria in de apparaatset worden geplaatst. De apparaat-verzameling die u maakt, is toegankelijk voor iedereen die toegang tot de toepassing, heeft zodat iedereen kan bekijken, wijzigen of verwijderen van het apparaat instellen.

    ![Apparaat instellen-query](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > De apparaatset is een dynamische query. Telkens wanneer u de lijst met apparaten weergeven, mogelijk zijn er verschillende apparaten in de lijst. De lijst is afhankelijk van welke apparaten op dit moment voldoen aan de criteria van de query.

1. Kies **Opslaan**.

## <a name="configure-the-dashboard-for-your-device-set"></a>Het Dashboard voor het instellen van uw apparaat configureren

Nadat u uw apparaat instellen hebt gemaakt, kunt u configureren de **Dashboard**. De **Dashboard** is de startpagina waar kunt u afbeeldingen en koppelingen plaatsen. U kunt ook rasters die lijst van de apparaten in de apparaatset toevoegen.

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Kies de **Dashboard** tabblad.

1. Klik op **sjabloon bewerken**.

    ![Ontwerpmodus op](media/howto-use-device-sets/image3.png)

1. Zie voor meer informatie over het toevoegen van een installatiekopie van een [voorbereiden en uploaden van afbeeldingen aan uw Azure IoT Central toepassing](howto-prepare-images.md).

1. Een tegel koppeling toevoegen:
    1. Kies **koppeling** in het rechterdeelvenster.
    1. Geef uw koppeling een **titel**.
    1. Kies een URL worden geopend wanneer de koppeling wordt geklikt.
    1. Geef uw koppeling een beschrijving waarin wordt weergegeven onder de **titel**.
    1. Kies **Opslaan**.

        ![Koppeling opslaan](media/howto-use-device-sets/image7.png)

    1. U kunt verplaatsen en het formaat van de tegel koppelen op de **Dashboard**.

1. Voeg een raster. Een raster is een tabel van de apparaten in het apparaat instellen met de kolommen die u kiest.
    1. Kies **raster** in het rechterdeelvenster.

        ![Kies raster](media/howto-use-device-sets/image8.png)

    1. Het raster geeft een **titel**.
    1. Selecteer de kolommen die moeten worden weergegeven door te kiezen **toevoegen/verwijderen**. Kies in het deelvenster dat verschijnt, de kolom die u wilt weergeven en kies de pijl-rechts om deze te selecteren.
    1. Kies **OK**.
    1. Kies **Opslaan**.

        ![Raster opslaan](media/howto-use-device-sets/image9.png)

    1. Slepen en neerzetten het raster om het te plaatsen op de **Dashboard**.

    > [!NOTE]
    > U kunt meerdere afbeeldingen, koppelingen en rasters toevoegen.
  
    1. Klik op **Gereed**.

    ![Ontwerpmodus uitschakelen](media/howto-use-device-sets/image10.png)


### <a name="configuring-location-map-in-your-device-sets-dashboard"></a>Dashboard instellen locatiekaart configureren in uw apparaat 
U kunt toevoegen om een locatiekaart voor het visualiseren van de locatie van uw apparaten ingesteld op een kaart.

Als u wilt toevoegen aan een locatiekaart op uw apparaat wordt ingesteld dashboard hebt u geconfigureerde locatie-eigenschap in de sjabloon voor het apparaat, Zie [maakt u een locatie-eigenschap aangestuurd door Azure Maps](howto-set-up-template.md).


1. Selecteer de kaart uit de bibliotheek op uw Dashboard apparaat instellen.

    ![Dashboard Maps wordt ingesteld voor apparaat](media/howto-use-device-sets/LocationMaps1.png)

2. Geef een titel in en kies de locatie-eigenschap die u eerder hebt geconfigureerd als onderdeel van de eigenschap van uw apparaat.
3. Opslaan en u ziet de kaart tegel weergeven van de locatie van uw apparaten in het apparaat ingesteld.
4. Nu als een operator weergaven dashboard Hiermee stelt u het apparaat, de operator alle tegels die u hebt geconfigureerd ziet, met inbegrip van de locatie van de kaart voor het visualiseren van alle apparaten-locatie in een oogopslag! 
    
> [!NOTE] 
> U kunt zich grootte van de kaart om de gewenste grootte te wijzigen. Te klikken op een pincode op de kaart wordt weergegeven de apparaatgegevens, naam en locatie. U kunt klikken op het pop-upvenster om naar de eigenschappenpagina van het apparaat te gaan.  


## <a name="configure-the-list-for-your-device-set"></a>De lijst voor het instellen van uw apparaat configureren

Nadat u uw apparaat instellen hebt gemaakt, kunt u configureren de **lijst**. De **lijst** ziet u alle apparaten in het apparaat instellen in een tabel met de kolommen die u kiest.

1. Kies **Apparaatsets** in het navigatiemenu links.

1. Kies de **lijst** tabblad.

1. Kies **kolomopties**.

    ![Kolomopties voor](media/howto-use-device-sets/image11.png)

1. Kies de kolommen die moeten worden weergegeven door het selecteren van de kolom die u wilt weergeven en het kiezen van de pijl-rechts om deze te selecteren.

    ![Kolom kiezen](media/howto-use-device-sets/image12.png)

1. Kies **OK**.

## <a name="analytics"></a>Analyse

De analyse in apparaatsets is hetzelfde als het tabblad belangrijkste analytische gegevens in het navigatiemenu links. U meer informatie over analytics in het artikel op [over het maken van analytics](howto-create-analytics.md).

## <a name="next-steps"></a>Volgende stappen

Nu dat u het apparaat wordt gebruikt in uw Azure IoT Central-toepassing hebt geleerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Over het maken van regels voor telemetrie](howto-create-telemetry-rules.md)
