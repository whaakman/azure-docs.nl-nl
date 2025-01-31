---
title: FMLE-coderingsprogramma voor het verzenden van een single-bitrate live stream configureren | Microsoft Docs
description: In dit onderwerp laat zien hoe het configureren van het coderingsprogramma Flash Media Live Encoder (FMLE) voor het verzenden van een single-bitrate stream aan AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 01bb628a6520488dcebf49a1e868213b955abc31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465934"
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>FMLE-coderingsprogramma gebruiken voor het verzenden van een single-bitrate live stream 
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

In dit artikel laat zien hoe het configureren van de [Flash Media Live Encoder](https://www.adobe.com/products/flash-media-encoder.html) encoder (FMLE) voor het verzenden van een single-bitrate stream aan AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

In deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u van Mac- of Linux gebruikmaakt, de Azure portal gebruiken voor het maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

Deze zelfstudie wordt beschreven met behulp van AAC. FMLE ondersteunt niet echter AAC standaard. U moet een invoegtoepassing voor de AAC-codering zoals invoegtoepassing van MainConcept kopen: [AAC-invoegtoepassing](https://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

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

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Geef de naam van een kanaal, het beschrijvingsveld is optioneel. Selecteer onder instellingen voor Channel **Standard** voor de optie voor Live codering met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen omdat laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> Het kanaal kan zo lang 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-fmle-live-encoder.md).

> [!IMPORTANT]
> Houd er rekening mee dat de facturering begint zodra kanaal krijgt de status gereed. Zie voor meer informatie, [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_fmle_rtmp></a>FMLE-coderingsprogramma configureren
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
1. Navigeer naar de Flash Media Live Encoder van (FMLE) interface op de machine die wordt gebruikt.

    De interface is een startpagina van instellingen. Noteer de volgende instellingen om te beginnen met streamen met FMLE aanbevolen.

   * Indeling: Framesnelheid H.264: 30.00
   * Invoergrootte: 1280 x 720
   * Bitsnelheid: 5000 kbps (kan worden aangepast op basis van de beperkingen op het netwerk)  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     Wanneer met behulp van bronnen geïnterlinieerde, kunt u de optie 'Zonder interliniëring' is ingeschakeld
2. Selecteer de moersleutelpictogram naast indeling, moeten de volgende aanvullende instellingen:

   * Profiel: Hoofdvenster
   * Niveau: 4.0
   * Frequentie van sleutelframes: 2 seconden

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. Stel de volgende belangrijke audio instelling:

   * Indeling: AAC
   * Samplefrequentie: 44100 Hz
   * Bitrate: 192 Kbps

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. Het kanaal Get invoer van URL om te kunnen toewijzen aan van de FMLE **RTMP-eindpunt**.

    Ga terug naar het AMSE-hulpprogramma en de voltooiingsstatus kanaal controleren. Zodra de status is gewijzigd van **vanaf** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, met de rechtermuisknop op de naam van het kanaal, navigeer naar de plaats de muisaanwijzer **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. Plak deze informatie in de **FMS URL** veld van de sectie uitvoer en de naam van een stream toewijzen.

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Herhaal deze stappen met het secundaire invoer-URL voor extra redundantie.
6. Selecteer **Verbinden**.

> [!IMPORTANT]
> Voordat u klikt op **Connect**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet laat het kanaal in een status gereed hebben zonder een invoer bijdrage feed voor langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview-versie** en selecteer **met Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Als de stroom in de speler wordt weergegeven, is klikt u vervolgens het coderingsprogramma correct is geconfigureerd om te verbinden met AMS.

Als er een fout is ontvangen, wordt het kanaal moet opnieuw worden ingesteld en encoder-instellingen aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor hulp.  

## <a name="create-a-program"></a>Een programma maken
1. Wanneer het kanaal afspelen hebt bevestigd, maakt u een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied van het programma en selecteert u **nieuw programma maken**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. Naam van het programma en wijzig indien nodig, de **voor het archiefvenster** (die standaard 4 uur). U kunt ook opgeven van een opslaglocatie bevinden of behoud de standaardwaarde.  
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
