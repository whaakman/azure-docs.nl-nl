---
title: Configureren van het coderingsprogramma NewTek TriCaster voor het verzenden van een single-bitrate live stream | Microsoft Docs
description: Dit onderwerp leest hoe u configureert het live coderingsprogramma Tricaster voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: eecb17b0e93e1717c5b79701c69e5263e73add9a
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Het coderingsprogramma NewTek TriCaster gebruiken voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Live elemental](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Dit artikel laat zien hoe u configureert de [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live codering verzendt een single-bitrate stream naar AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u op Mac- of Linux, gebruikt u de Azure portal maken [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Wanneer u Tricaster gebruikt voor het verzenden van een bijdrage feed AMS-kanalen die zijn ingeschakeld voor live codering, kunnen er video en audio haperingen in uw live gebeurtenis als u bepaalde functies van Tricaster, zoals snelle knippen tussen feeds of overschakelen van Smartphones gebruiken. De AMS-team werkt deze problemen tot die tijd is hersteld, is het niet raadzaam deze functies gebruiken.
>
>

## <a name="prerequisites"></a>Vereisten
* [Een Azure Media Services-account maken](media-services-portal-create-account.md)
* Zorg dat er een Streaming-eindpunt is uitgevoerd. Zie voor meer informatie [Streaming-eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)
* Installeer de nieuwste versie van de [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) hulpprogramma.
* Het hulpprogramma voor starten en verbinding maken met uw AMS-account.

## <a name="tips"></a>Tips
* Gebruik indien mogelijk een internetverbinding ' hardwired '.
* Een goede vuistregel bij het bepalen van de vereiste bandbreedte is voor de streaming bitsnelheden verdubbelen. Hoewel dit niet verplicht is, vermindert het de gevolgen van opstoppingen in het netwerk.
* Wanneer u coderingsprogramma's op basis van software, sluit u alle onnodige programma's.

## <a name="create-a-channel"></a>Een kanaal maken
1. Navigeer in het AMSE-hulpprogramma naar het **Live** tabblad en met de rechtermuisknop op het gebied van kanaal. Selecteer **kanaal maken...** in het menu.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Geef een kanaalnaam het beschrijvingsveld is optioneel. Selecteer onder instellingen voor kanaal **standaard** voor de optie Live Encoding met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen zoals is laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Het kanaal kan zo lang starten 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Omdat facturering begint zodra kanaal probeert het gereed. Zie voor meer informatie [van kanaal statussen](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_tricaster_rtmp></a>Het coderingsprogramma NewTek TriCaster configureren
In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie beschrijft de configuratiestappen in meer detail.

**Video**:

* Codec: H.264
* Profiel: Hoge (niveau 4.0)
* Bitrate: 5000 kbps
* Sleutelframe: 2 seconden (60 seconden)
* Frequentie frame: 30

**Audio**:

* Codec: AAC (LC)
* Bitrate: 192 kbps
* Samplefrequentie: 44,1 kHz

### <a name="configuration-steps"></a>Configuratiestappen
1. Maak een nieuwe **NewTek TriCaster** project, afhankelijk van welke video-invoerbron wordt gebruikt.
2. Eenmaal in dat project vinden de **stroom** en klik op het pictogram tandwielpictogram naast deze toegang krijgt tot het menu van de configuratie stream.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Zodra het menu heeft geopend, klikt u op **nieuw** onder de kop van de verbinding. Als u wordt gevraagd voor het verbindingstype, selecteer **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Klik op **OK**.
5. Een profiel FMLE kan nu worden geïmporteerd door te klikken op de vervolgkeuzepijl onder **Streaming profiel** en te navigeren naar **Bladeren**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Ga naar waar het geconfigureerde FMLE-profiel is opgeslagen.
7. Selecteer deze en druk op **OK**.

    Zodra het profiel is geüpload, gaat u verder met de volgende stap.
8. Get het kanaal de invoer-URL om te kunnen toewijzen aan de Tricaster **RTMP eindpunt**.

    Ga terug naar het AMSE-hulpprogramma, en controleren van de voltooiingsstatus van kanaal. Zodra de status is gewijzigd van **starten** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, klikt u met de rechtermuisknop op naam van het kanaal, navigeer naar aanwijzen via **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Plak deze informatie in de **locatie** veld onder **Flash Server** binnen het Tricaster-project. Wijs ook de Stroomnaam van een in het **stroom-ID** veld.

    Als stream informatie is toegevoegd aan het profiel FMLE, het kan ook worden geïmporteerd in deze sectie door te klikken op **importinstellingen**, navigeren naar de opgeslagen FMLE-profiel en te klikken op **OK**. De relevante Server Flash-velden moeten vullen met de gegevens van FMLE.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Wanneer u klaar bent, klikt u op **OK** aan de onderkant van het scherm. Wanneer de video en audio-invoer in de Tricaster klaar bent, moet u beginnen streaming aan AMS door te klikken op de **stroom** knop.

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Voordat u op **stroom**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet het kanaal in een status ready heeft verlaten zonder een invoer bijdrage feed langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen
Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview** en selecteer **met Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Als de stroom wordt weergegeven in de speler, is klikt u vervolgens het coderingsprogramma juist geconfigureerd voor verbinding met AMS.

Als een fout wordt ontvangen, wordt het kanaal moet opnieuw worden ingesteld en instellingen voor codering aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor instructies.  

## <a name="create-a-program"></a>Een programma maken
1. Nadat het kanaal afspelen is bevestigd, maak een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied programma en selecteer **nieuw programma maken**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Naam van het programma en wijzig indien nodig de **lengte van een archiefvenster** (die standaard vier uur). U kunt ook opgeven van een opslaglocatie of laat de standaardwaarde.  
3. Controleer de **Start het programma nu** vak.
4. Klik op **programma maken**.  

    >[!NOTE]
    >Maken van het programma kost minder tijd dan het maken van kanaal.
        
5. Zodra het programma wordt uitgevoerd, bevestigt u afspelen door met de rechtermuisknop op het programma en te navigeren naar **afspelen van de programma's** en selecteren **met Azure Media Player**.  
6. Zodra bevestigd, opnieuw met de rechtermuisknop op het programma en selecteer **Kopieer de URL van de uitvoer naar Klembord** (of het ophalen van deze informatie van de **programma gegevens en instellingen** optie in het menu).

De stroom is nu gereed om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor live weer te geven.  

## <a name="troubleshooting"></a>Problemen oplossen
Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor instructies.

## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
