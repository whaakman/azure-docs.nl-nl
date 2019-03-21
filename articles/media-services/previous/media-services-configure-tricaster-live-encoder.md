---
title: NewTek TriCaster-coderingsprogramma voor het verzenden van een single-bitrate live stream configureren | Microsoft Docs
description: In dit onderwerp laat zien hoe het configureren van de live Tricaster-coderingsprogramma voor het verzenden van een single-bitrate stream aan AMS-kanalen die zijn ingeschakeld voor live codering.
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
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: adacce5c8307f3be972920bd3aef6d8ea912eb30
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57992003"
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>NewTek TriCaster-coderingsprogramma gebruiken voor het verzenden van een single-bitrate live stream  
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

In dit artikel laat zien hoe het configureren van de [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live coderingsprogramma voor het verzenden van een single-bitrate stream aan AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Wanneer u Tricaster gebruikt voor het verzenden van een bijdrage aan AMS-kanalen die zijn ingeschakeld voor live codering feed, kunnen er video/audio haperingen in uw live-gebeurtenis als u bepaalde functies van Tricaster, zoals snelle knippen tussen feeds of het overschakelen naar/van slates gebruikt. De AMS-team werkt aan een oplossing van deze problemen tot die tijd kun, wordt deze niet aanbevolen om deze functies te gebruiken.
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

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Geef de naam van een kanaal, het beschrijvingsveld is optioneel. Selecteer onder instellingen voor Channel **Standard** voor de optie voor Live codering met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen omdat laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Het kanaal kan zo lang 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> De facturering begint zodra kanaal krijgt de status gereed. Zie voor meer informatie, [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfiguretricasterrtmpconfigure-the-newtek-tricaster-encoder"></a><a id="configure_tricaster_rtmp"/>NewTek TriCaster-coderingsprogramma configureren

In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie worden de configuratiestappen in meer detail beschreven.

**Video**:

* Codec: H.264
* Profiel: Hoog (niveau 4.0)
* Bitrate: 5000 kbps
* Sleutelframes: 2 seconden (60 seconden)
* Framesnelheid: 30

**Audio**:

* Codec: AAC (LC)
* Bitrate: 192 kbps
* Samplefrequentie: 44.1 kHz

### <a name="configuration-steps"></a>Configuratiestappen

1. Maak een nieuwe **NewTek TriCaster** project, afhankelijk van welke video-invoerbron wordt gebruikt.
2. Eenmaal in dat project, vinden de **Stream** en klik op het tandwielpictogram ernaast voor toegang tot het menu van de configuratie van stream.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Nadat het menu is geopend, klikt u op **nieuw** onder de kop van de verbinding. Wanneer u hierom wordt gevraagd voor het verbindingstype, selecteert u **Adobe Flash**.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Klik op **OK**.
5. Een profiel FMLE kan nu worden geïmporteerd door te klikken op de vervolgkeuzepijl onder **Streaming profiel** en te navigeren naar **Bladeren**.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Ga naar waar het geconfigureerde FMLE-profiel is opgeslagen.
7. Selecteer deze en drukt u op **OK**.

    Zodra het profiel is geüpload, gaat u verder met de volgende stap.
8. Het kanaal Get invoer van URL om te kunnen toewijzen aan de Tricaster **RTMP-eindpunt**.

    Ga terug naar het AMSE-hulpprogramma en de voltooiingsstatus kanaal controleren. Zodra de status is gewijzigd van **vanaf** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, klikt u met de rechtermuisknop op naam van het kanaal, navigeer naar de plaats de muisaanwijzer **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Plak deze informatie in de **locatie** veld onder **Flash Server** binnen het Tricaster-project. Ook toewijzen de Stroomnaam van een in de **Stream ID** veld.

    Als informatie van de stroom is toegevoegd aan het profiel FMLE, het kan ook worden geïmporteerd in deze sectie door te klikken op **importinstellingen**, te navigeren naar de opgeslagen FMLE-profiel en te klikken op **OK**. De vereiste velden in de Flash-Server moeten met de gegevens uit FMLE invullen.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Wanneer u klaar bent, klikt u op **OK** aan de onderkant van het scherm. Wanneer video en audio-invoer in de Tricaster gereed zijn, beginnen met streamen aan AMS door te klikken op de **Stream** knop.

     ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Voordat u klikt op **Stream**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet laat het kanaal in een status gereed hebben zonder een invoer bijdrage feed voor langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview-versie** en selecteer **met Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout is ontvangen, wordt het kanaal moet opnieuw worden ingesteld en encoder-instellingen aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor hulp.  

## <a name="create-a-program"></a>Een programma maken

1. Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied van het programma en selecteert u **nieuw programma maken**.  

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
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
