---
title: De Haivision KB-coderingsprogramma voor het verzenden van een single-bitrate live stream naar Azure configureren | Microsoft Docs
description: In dit onderwerp laat zien hoe het configureren van het live coderingsprogramma Haivision KB voor het verzenden van een single-bitrate stream aan AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: ''
author: dbgeorge
manager: vsood
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako;dbgeorge
ms.openlocfilehash: 1672eb2ef0db36c9b30ca444fa4224eb1afbe828
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998481"
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Het live coderingsprogramma Haivision KB gebruiken voor het verzenden van een single-bitrate live stream  
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)

In dit onderwerp wordt beschreven hoe het configureren van de [Havision KB live coderingsprogramma](https://www.haivision.com/products/kb-series/) encoder voor het verzenden van een single-bitrate stream aan AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Vereisten
*   Toegang tot een Haivision KB-coderingsprogramma, ZW 5.01 uitgevoerd of hoger.
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
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Geef de naam van een kanaal, het beschrijvingsveld is optioneel. Selecteer onder instellingen voor Channel **Standard** voor de optie voor Live codering met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen omdat laten. Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.
3. Klik op **kanaal maken**.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> Het kanaal kan zo lang 20 minuten duren.

## <a name="configure-the-haivision-kb-encoder"></a>De Haivision KB-coderingsprogramma configureren
In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie worden de configuratiestappen in meer detail beschreven.

Video:
-   Codec: H.264
-   Profiel: Hoog (niveau 4.0)
-   Bitrate: 5000 kbps
-   Sleutelframes: 2 seconden (60 frames)
-   Framesnelheid: 30

Audio:
-   Codec: AAC (LC)
-   Bitrate: 192 kbps
-   Samplefrequentie: 44.1 kHz

## <a name="configuration-steps"></a>Configuratiestappen
1.  Meld u aan bij de gebruikersinterface Haivision KB.
2.  Klik op de **menuknop** in het beheercentrum kanaal en selecteer **kanaal toevoegen**  
    ![Schermafbeelding van 2017-08-14 bij 9.15.09 AM](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Type de **kanaalnaam** veld in de naam en klik op volgende.  
    ![Schermafbeelding van 2017-08-14 bij 9.19.07 AM](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Selecteer de **kanaal invoerbron** uit de **invoerbron** vervolgkeuzelijst en klik op volgende.
    ![Schermafbeelding van 2017-08-14 bij 9.20.44 AM](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Uit de **Encoder sjabloon** vervolgkeuzelijst kiezen **H264-720-AAC-192** en klik op volgende.
    ![Schermafbeelding van 2017-08-14 bij 9.23.15 AM](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Uit de **Selecteer nieuwe uitvoer** vervolgkeuzelijst kiezen **RTMP** en klik op volgende.  
    ![Schermafbeelding van 2017-08-14 bij 9.27.51 AM](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Uit de **kanaal uitvoer** venster vullen van de Azure stream-gegevens. Plak de **RTMP** koppeling van de instellingen voor het eerste kanaal in de **Server** gebied. In de **uitvoernaam** gebied Typ de naam van het kanaal. Gebruik de sjabloon RTMPStreamName_ % video_bitrate % om de naam van de stroom in de sjabloon van Stream.
    ![Schermafbeelding van 2017-08-14 bij 9.33.17 AM](./media/media-services-configure-kb-live-encoder/step7.png)
8.  Klik op volgende en vervolgens klikt u op gereed.
9.  Klik op de **knop afspelen** starten van het coderingsprogramma-kanaal.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Test afspelen
Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. Beweeg de muisaanwijzer over het afspelen van de Preview-versie in het menu en selecteer met Azure Media Player.

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout is ontvangen, wordt het kanaal moet opnieuw worden ingesteld en encoder-instellingen aangepast. Zie het artikel over probleemoplossing voor hulp.

## <a name="create-a-program"></a>Een programma maken
1.  Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Klik op het tabblad Live in het AMSE-hulpprogramma met de rechtermuisknop op het gebied van het programma en selecteer nieuw programma maken.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Naam van het programma en wijzig indien nodig, de het archiefvenster (die een standaardwaarde vier uur). U kunt ook opgeven van een opslaglocatie bevinden of behoud de standaardwaarde.
2.  Controleer de Start het programma nu-vak.
3.  Klik op programma maken.
4.  Zodra het programma wordt uitgevoerd, afspelen bevestigen met de rechtermuisknop op het programma en te navigeren naar de programma's af te spelen en vervolgens te selecteren met Azure Media Player.
5.  Zodra u hebt bevestigd, met de rechtermuisknop op het programma opnieuw uit en selecteert u de URL van de uitvoer naar Klembord kopiëren (of deze informatie ophalen van de programma-informatie en de optie in het menu instellingen).

De stroom is nu klaar om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor het weergeven van live.

> [!NOTE]
> Maken van een programma duurt minder lang dan het maken van kanalen.
