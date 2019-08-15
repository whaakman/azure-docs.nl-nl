---
title: De NewTek TriCaster Encoder configureren voor het verzenden van een live stream met één bitsnelheid | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe u de TriCaster Live Encoder configureert om een enkele bitrate stroom te verzenden naar AMS-kanalen die zijn ingeschakeld voor Live encoding.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 0e793a5aa7d619b0bb7a1d3efcdf665ea400c555
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "69016741"
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>De NewTek TriCaster Encoder gebruiken om een live stream met één bitsnelheid te verzenden  
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Elementair Live](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

In dit artikel wordt beschreven hoe u de [NewTek TriCaster](https://newtek.com/products/tricaster-40.html) Live Encoder configureert om een enkele bitrate stroom te verzenden naar AMS-kanalen die zijn ingeschakeld voor Live encoding. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Wanneer u TriCaster gebruikt voor het verzenden van een bijdrage aan AMS-kanalen die zijn ingeschakeld voor Live encoding, kunnen er video-en audio storingen voor komen in uw live-evenement als u bepaalde functies van TriCaster gebruikt, zoals het snel knippen tussen feeds of overschakelen naar/van pastels. Het AMS-team werkt aan het oplossen van deze problemen. het is dan ook niet aanbevolen deze functies te gebruiken.
>
>

## <a name="prerequisites"></a>Vereisten

* [Een Azure Media Services-account maken](media-services-portal-create-account.md)
* Controleer of er is een Streaming-eindpunt is uitgevoerd. Zie voor meer informatie, [Streaming-eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)
* Installeer de nieuwste versie van de [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) hulpprogramma.
* Het hulpprogramma voor starten en verbinding maken met uw AMS-account.

## <a name="tips"></a>Tips

* Gebruik indien mogelijk een internetverbinding ' hardwired '.
* Een goede vuistregel bij het bepalen van de bandbreedtevereisten voor, is het dubbele van de streaming bitsnelheden. Hoewel dit niet verplicht is, vermindert het de gevolgen van opstoppingen in het netwerk.
* Wanneer u coderingsprogramma's op basis van software, sluit u alle onnodige programma's.

## <a name="create-a-channel"></a>Een kanaal maken

1. In het AMSE-hulpprogramma, gaat u naar de **Live** tabblad, en met de rechtermuisknop op het gebied van het kanaal. Selecteer **kanaal maken...** in het menu.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Geef de naam van een kanaal, het beschrijvingsveld is optioneel. Selecteer onder instellingen voor Channel **Standard** voor de optie voor Live codering met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen omdat laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Het kanaal kan zo lang 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> De facturering begint zodra kanaal krijgt de status gereed. Zie voor meer informatie, [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfigure_tricaster_rtmpconfigure-the-newtek-tricaster-encoder"></a><a id="configure_tricaster_rtmp"/>De NewTek TriCaster-Encoder configureren

In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie worden de configuratiestappen in meer detail beschreven.

**Video**:

* Videocodec H.264
* Uplinkpoortprofiel Hoog (niveau 4,0)
* Bitsnelheid 5000 kbps
* Keyframe: 2 seconden (60 seconden)
* Frame frequentie: 30

**Audio**:

* Videocodec AAC (LC)
* Bitsnelheid 192 kbps
* Sample frequentie: 44,1 kHz

### <a name="configuration-steps"></a>Configuratiestappen

1. Maak een nieuw **NewTek TriCaster** -project, afhankelijk van welke video-invoer bron wordt gebruikt.
2. Ga in dat project naar de knop **Stream** en klik op het tandwiel pictogram ernaast om toegang te krijgen tot het menu voor het configureren van de stream.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Zodra het menu is geopend, klikt u op **Nieuw** onder de kop verbinding. Selecteer **Adobe Flash**wanneer u wordt gevraagd om het verbindings type.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Klik op **OK**.
5. U kunt nu een FMLE-profiel importeren door te klikken op de vervolg keuze pijl onder **streaming profile** en te navigeren om te **Bladeren**.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Ga naar de locatie waar het geconfigureerde FMLE-profiel is opgeslagen.
7. Selecteer deze en druk op **OK**.

    Zodra het profiel is geüpload, gaat u verder met de volgende stap.
8. Haal de invoer-URL van het kanaal op om deze toe te wijzen aan het TriCaster **RTMP-eind punt**.

    Ga terug naar het AMSE-hulpprogramma en de voltooiingsstatus kanaal controleren. Zodra de status is gewijzigd van **vanaf** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal actief is, klikt u met de rechter muisknop op de naam van het kanaal, gaat u omlaag om de **invoer-URL naar het klem bord te verplaatsen** en selecteert u vervolgens **primaire invoer-URL**.  

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Plak deze informatie in het veld **locatie** onder **Flash Server** binnen het TriCaster-project. Wijs ook een stroom naam in het veld **Stream-id** toe.

    Als er stroom gegevens aan het FMLE-profiel zijn toegevoegd, kan het ook worden geïmporteerd in deze sectie door te klikken op **Instellingen importeren**, naar het opgeslagen FMLE-profiel te gaan en op **OK**te klikken. De relevante Flash server-velden moeten worden ingevuld met de informatie van FMLE.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Wanneer u klaar bent, klikt u op **OK** onder aan het scherm. Wanneer video-en audio-invoer in de TriCaster klaar zijn, begint u te streamen naar AMS door te klikken op de knop **Stream** .

     ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Voordat u klikt op **Stream**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet laat het kanaal in een status gereed hebben zonder een invoer bijdrage feed voor langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview-versie** en selecteer **met Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout wordt ontvangen, moet het kanaal opnieuw worden ingesteld en moeten de coderings instellingen worden aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor hulp.  

## <a name="create-a-program"></a>Een programma maken

1. Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied van het programma en selecteert u **nieuw programma maken**.  

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Naam van het programma en wijzig indien nodig, de **voor het archiefvenster** (die standaard vier uur). U kunt ook opgeven van een opslaglocatie bevinden of behoud de standaardwaarde.  
3. Controleer de **Start het programma nu** vak.
4. Klik op **programma maken**.  

    >[!NOTE]
    >Maken van een programma duurt minder lang dan het maken van kanalen.
        
5. Zodra het programma wordt uitgevoerd, bevestigt u afspelen door met de rechtermuisknop op het programma en te navigeren naar **afspelen van de programma's** en vervolgens de optie **met Azure Media Player**.  
6. Nadat u hebt bevestigd, met de rechtermuisknop op het programma opnieuw uit en selecteer **de URL van de uitvoer naar Klembord kopiëren** (of het ophalen van deze informatie van de **gegevens en instellingen van het programma** optie in het menu).

De stroom is nu klaar om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor het weergeven van live.  

## <a name="troubleshooting"></a>Problemen oplossen

Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor hulp.

## <a name="next-step"></a>Volgende stap

Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
