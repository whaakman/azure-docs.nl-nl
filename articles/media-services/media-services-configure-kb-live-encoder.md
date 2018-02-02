---
title: Het coderingsprogramma Haivision KB voor het verzenden van een single-bitrate live stream naar Azure configureren | Microsoft Docs
description: Dit onderwerp leest hoe u configureert het live coderingsprogramma Haivision KB voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: 
author: dbgeorge
manager: vsood
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/02/2018
ms.author: juliako;dbgeorge
ms.openlocfilehash: 25077cd9338a2764c6dff9e755812033685f6641
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Het live coderingsprogramma Haivision KB gebruiken voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [Live elemental](media-services-configure-elemental-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)

Dit onderwerp wordt beschreven hoe u configureert de [Havision KB live coderingsprogramma](https://www.haivision.com/products/kb-series/) codering verzendt een single-bitrate stream naar AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u op Mac- of Linux, gebruikt u de Azure portal maken [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Vereisten
*   Toegang tot een encoder Haivision KB, SW 5.01 uitgevoerd of hoger.
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
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Geef een kanaalnaam het beschrijvingsveld is optioneel. Selecteer onder instellingen voor kanaal **standaard** voor de optie Live Encoding met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen zoals is laten. Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.
3. Klik op **kanaal maken**.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> Het kanaal kan zo lang starten 20 minuten duren.

## <a name="configure-the-haivision-kb-encoder"></a>Het coderingsprogramma Haivision KB configureren
In deze zelfstudie worden de volgende uitvoerinstellingen gebruikt. De rest van deze sectie beschrijft de configuratiestappen in meer detail.

Video:
-   Codec: H.264
-   Profiel: Hoge (niveau 4.0)
-   Bitrate: 5000 kbps
-   Sleutelframe: 2 seconden (60 seconden)
-   Frequentie frame: 30

Audio:
-   Codec: AAC (LC)
-   Bitrate: 192 kbps
-   Samplefrequentie: 44,1 kHz

## <a name="configuration-steps"></a>Configuratiestappen
1.  Aanmelden bij de gebruikersinterface Haivision KB.
2.  Klik op de **menuknop** in het beheercentrum kanaal en selecteer **kanaal toevoegen**  
    ![Schermopname 2017-08-14 op 9.15.09 AM.png](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Typ de **kanaalnaam** veld in de naam en klik op volgende.  
    ![Schermopname 2017-08-14 op 9.19.07 AM.png](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Selecteer de **kanaal invoerbron** van de **invoerbron** vervolgkeuzelijst en klik op volgende.
    ![Schermopname 2017-08-14 op 9.20.44 AM.png](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Van de **Encoder sjabloon** vervolgkeuzelijst kiezen **standaardinstelling H264 720-AAC 192** en klik op volgende.
    ![Schermopname 2017-08-14 op 9.23.15 AM.png](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Van de **nieuwe uitvoer selecteren** vervolgkeuzelijst kiezen **RTMP** en klik op volgende.  
    ![Schermopname 2017-08-14 op 9.27.51 AM.png](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Van de **kanaal uitvoer** venster vullen van de Azure stream-informatie. Plak de **RTMP** koppeling van de instellingen voor het eerste kanaal in de **Server** gebied. In de **uitvoernaam** gebied Typ de naam van het kanaal. Gebruik de sjabloon RTMPStreamName_ % video_bitrate % naam van de stroom in het gedeelte stroom naam sjabloon.
    ![Schermopname 2017-08-14 op 9.33.17 AM.png](./media/media-services-configure-kb-live-encoder/step7.png)
8.  Klik op volgende en klik op gereed.
9.  Klik op de **knop afspelen** starten van het kanaal encoder.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Test afspelen
Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. Beweeg de muisaanwijzer over het afspelen van de Preview-versie in het menu en selecteer met Azure Media Player.

Als de stroom wordt weergegeven in de speler, is klikt u vervolgens het coderingsprogramma juist geconfigureerd voor verbinding met AMS.

Als een fout wordt ontvangen, wordt het kanaal moet opnieuw worden ingesteld en instellingen voor codering aangepast. Zie het artikel over probleemoplossing voor hulp.

## <a name="create-a-program"></a>Een programma maken
1.  Nadat het kanaal afspelen is bevestigd, maak een programma. Klik op het tabblad Live in het AMSE-hulpprogramma met de rechtermuisknop op het gebied programma en selecteer nieuw programma maken.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Naam van het programma en wijzig indien nodig, de lengte van een archiefvenster (die een standaardwaarde vier uur). U kunt ook opgeven van een opslaglocatie of laat de standaardwaarde.
2.  Selectievakje de Start het programma nu.
3.  Klik op maken programma.
4.  Zodra het programma wordt uitgevoerd, afspelen bevestigen met de rechtermuisknop op het programma en navigeren naar de programma's af te spelen en vervolgens met Azure Media Player te selecteren.
5.  Zodra bevestigd, opnieuw met de rechtermuisknop op het programma en selecteert u de URL van de uitvoer naar Klembord kopiëren (of deze informatie ophalen van de programma-informatie en de optie in het menu instellingen).

De stroom is nu gereed om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor live weer te geven.

> [!NOTE]
> Maken van het programma kost minder tijd dan het maken van kanaal.