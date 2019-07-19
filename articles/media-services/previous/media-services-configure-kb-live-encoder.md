---
title: De Haivision KB Encoder configureren om een live stream met één bitsnelheid naar Azure te verzenden | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe u de Haivision KB Live Encoder kunt configureren om een enkele bitrate stroom te verzenden naar AMS-kanalen die zijn ingeschakeld voor Live encoding.
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
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 7bb3db4861842e145689682035adc3c691538adf
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297799"
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>De Haivision KB Live Encoder gebruiken om een live stream met één bitsnelheid te verzenden  
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)

In dit onderwerp wordt uitgelegd hoe u de [HAVISION KB Live Encoder](https://www.haivision.com/products/kb-series/) encoder configureert om een enkele bitrate stroom te verzenden naar AMS-kanalen die zijn ingeschakeld voor Live encoding. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Vereisten
*   Toegang tot een Haivision KB-encoder, waarop SW v 5.01 of hoger wordt uitgevoerd.
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

## <a name="configure-the-haivision-kb-encoder"></a>De Haivision KB-Encoder configureren
In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie worden de configuratiestappen in meer detail beschreven.

Video:
-   Videocodec H.264
-   Uplinkpoortprofiel Hoog (niveau 4,0)
-   Bitsnelheid 5000 kbps
-   Keyframe: 2 seconden (60 frames)
-   Frame frequentie: 30

Audio:
-   Videocodec AAC (LC)
-   Bitsnelheid 192 kbps
-   Sample frequentie: 44,1 kHz

## <a name="configuration-steps"></a>Configuratiestappen
1.  Meld u aan bij de gebruikers interface van Haivision KB.
2.  Klik op de **menu knop** in het kanaal besturings centrum en selecteer **kanaal toevoegen**  
    ![Scherm afbeelding 2017-08-14 bij 9.15.09 uur](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Typ de **naam** van het kanaal in het veld naam en klik op volgende.  
    ![Scherm afbeelding 2017-08-14 bij 9.19.07 uur](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Selecteer de **invoer bron** voor het kanaal in de vervolg keuzelijst **invoer bron** en klik op volgende.
    ![Scherm afbeelding 2017-08-14 bij 9.20.44 uur](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Kies in de vervolg keuzelijst coderings **sjabloon** **H264-720-AAC-192** en klik op volgende.
    ![Scherm afbeelding 2017-08-14 bij 9.23.15 uur](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Kies in de vervolg keuzelijst **nieuwe uitvoer selecteren** **RTMP** en klik op volgende.  
    ![Scherm afbeelding 2017-08-14 bij 9.27.51 uur](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Vul in het venster **kanaal uitvoer** de gegevens van de Azure stream in. Plak de **RTMP** -koppeling vanuit de eerste kanaal installatie in het gebied **Server** . Typ in het gebied **uitvoer naam** de naam van het kanaal. Gebruik in het gebied sjabloon van de stroom naam de sjabloon RTMPStreamName_% video_bitrate% om de stroom een naam te benoemen.
    ![Scherm afbeelding 2017-08-14 bij 9.33.17 uur](./media/media-services-configure-kb-live-encoder/step7.png)
8.  Klik op volgende en klik vervolgens op gereed.
9.  Klik op de **afspeel knop** om het coderings kanaal te starten.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Test afspelen
Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. Klik in het menu met de muis aanwijzer op het afspelen van de preview-versie en selecteer met Azure Media Player.

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout is ontvangen, wordt het kanaal moet opnieuw worden ingesteld en encoder-instellingen aangepast. Raadpleeg het artikel over probleem oplossing voor hulp.

## <a name="create-a-program"></a>Een programma maken
1.  Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Klik onder het tabblad Live in het hulp programma AMSE met de rechter muisknop in het gebied Program en selecteer nieuw programma maken.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Geef het programma een naam en wijzig indien nodig de lengte van het archief venster (de standaard waarde is vier uur). U kunt ook opgeven van een opslaglocatie bevinden of behoud de standaardwaarde.
2.  Schakel het selectie vakje programma nu starten in.
3.  Klik op programma maken.
4.  Nadat het programma is uitgevoerd, kunt u het afspelen bevestigen door met de rechter muisknop op het programma te klikken en te navigeren om de Program ma's af te spelen en vervolgens te selecteren met Azure Media Player.
5.  Nadat deze is bevestigd, klikt u opnieuw met de rechter muisknop op het programma en selecteert u de uitvoer-URL naar het klem bord kopiëren (of deze gegevens ophalen uit de optie informatie en instellingen van het programma in het menu).

De stroom is nu klaar om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor het weergeven van live.

> [!NOTE]
> Maken van een programma duurt minder lang dan het maken van kanalen.
