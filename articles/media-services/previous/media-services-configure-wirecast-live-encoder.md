---
title: Configureer de Telestream Wirecast-coderingsprogramma voor het verzenden van een single-bitrate live stream | Microsoft Docs
description: 'In dit onderwerp laat zien hoe het configureren van het live coderingsprogramma Wirecast voor het verzenden van een single-bitrate stream aan AMS-kanalen die zijn ingeschakeld voor live codering. '
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 6cc4c0b01511309766e48c3d671ee897e5d6f326
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669110"
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Gebruik de Wirecast-coderingsprogramma voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

In dit artikel laat zien hoe het configureren van de [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live coderingsprogramma voor het verzenden van een single-bitrate stream aan AMS kanalen die zijn ingeschakeld voor live codering.  Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

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

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Geef de naam van een kanaal, het beschrijvingsveld is optioneel. Selecteer onder instellingen voor Channel **Standard** voor de optie voor Live codering met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen omdat laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Het kanaal kan zo lang 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> De facturering begint zodra kanaal krijgt de status gereed. Zie voor meer informatie, [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfigurewirecastrtmp-aconfigure-the-telestream-wirecast-encoder"></a>< een id = "configure_wirecast_rtmp" /a > de Telestream Wirecast-coderingsprogramma configureren
In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie worden de configuratiestappen in meer detail beschreven.

**Video**:

* Codec: H.264
* Profiel: Hoog (niveau 4.0)
* Bitrate: 5000 kbps
* Sleutelframes: 2 seconden (60 seconden)
* Tarief frame: 30

**Audio**:

* Codec: AAC (LC)
* Bitrate: 192 kbps
* Samplefrequentie: 44,1 kHz

### <a name="configuration-steps"></a>Configuratiestappen
1. Open de Telestream Wirecast-toepassing op de computer die wordt gebruikt, en voor het streamen van RTMP instellen.
2. Configureren van de uitvoer door te navigeren naar de **uitvoer** tabblad en selecteren **uitvoerinstellingen...** .

    Zorg ervoor dat de **bestemming voor de uitvoer** is ingesteld op **RTMP-Server**.
3. Klik op **OK**.
4. Stel op de instellingenpagina van de **bestemming** veld **Azure Media Services**.

    Het profiel codering is vooraf geselecteerde naar **Azure H.264 720 p 16:9 (1280 x 720)**. Voor het aanpassen van deze instellingen, selecteer het tandwielpictogram rechts van de vervolgkeuzelijst en kies vervolgens **nieuwe voorinstelling**.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Voorinstellingen van het coderingsprogramma configureren.

    Naam van de vooraf gedefinieerde instellingen en controleren op de volgende aanbevolen instellingen:

    **Video**

   * Coderingsprogramma: MainConcept H.264
   * Frames per seconde: 30
   * Gemiddelde bitsnelheid: 5000 kbits per seconde (kan worden aangepast op basis van de beperkingen op het netwerk)
   * Profiel: Main
   * Belangrijkste frame elke: 60 frames

    **Audio**

   * Doelbitsnelheid: 192 kbits/sec
   * Samplefrequentie: 44,100 kHz

     ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Klik op **Opslaan**.

    Het veld Encoding heeft nu het zojuist gemaakte profiel beschikbaar voor selectie.

    Zorg ervoor dat het nieuwe profiel is geselecteerd.
7. Het kanaal Get invoer van URL om te kunnen toewijzen aan de Wirecast **RTMP-eindpunt**.

    Ga terug naar het AMSE-hulpprogramma en de voltooiingsstatus kanaal controleren. Zodra de status is gewijzigd van **vanaf** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, met de rechtermuisknop op de naam van het kanaal, navigeer naar de plaats de muisaanwijzer **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. In de Wirecast **uitvoerinstellingen** venster, plakt u deze informatie in de **adres** veld van de sectie uitvoer en de naam van een stream toewijzen.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Selecteer **OK**.
2. Op de hoofdpagina **Wirecast** scherm, invoerbronnen bevestigen voor video en audio gereed zijn en druk vervolgens op **Stream** in de bovenste linkerbenedenhoek weergegeven.

   ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Voordat u klikt op **Stream**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet laat het kanaal in een status gereed hebben zonder een invoer bijdrage feed voor langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview-versie** en selecteer **met Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout is ontvangen, wordt het kanaal moet opnieuw worden ingesteld en encoder-instellingen aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor hulp.  

## <a name="create-a-program"></a>Een programma maken
1. Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied van het programma en selecteert u **nieuw programma maken**.  

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
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

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
