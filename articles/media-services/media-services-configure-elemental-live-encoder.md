---
title: Configureer het elementaire Live coderingsprogramma voor het verzenden van een single-bitrate live stream | Microsoft Docs
description: Dit onderwerp leest hoe u configureert het elementaire Live coderingsprogramma voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 668a3ab46a70c0ee25fa87031d27c0f4333ec89c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Het elementaire Live coderingsprogramma gebruiken voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [Live elemental](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Dit onderwerp wordt beschreven hoe u configureert de [elementair Live](http://www.elementaltechnologies.com/products/elemental-live) codering verzendt een single-bitrate stream naar AMS kanalen die zijn ingeschakeld voor live codering.  Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u op Mac- of Linux, gebruikt u de Azure portal maken [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Vereisten
* Moet een werkende kennis hebben van elementaire Live web-interface gebruiken voor het maken van live gebeurtenissen.
* [Een Azure Media Services-account maken](media-services-portal-create-account.md)
* Zorg dat er een Streaming-eindpunt is uitgevoerd. Zie voor meer informatie [Streaming-eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md).
* Installeer de nieuwste versie van de [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) hulpprogramma.
* Het hulpprogramma voor starten en verbinding maken met uw AMS-account.

## <a name="tips"></a>Tips
* Gebruik indien mogelijk een internetverbinding ' hardwired '.
* Een goede vuistregel bij het bepalen van de vereiste bandbreedte is voor de streaming bitsnelheden verdubbelen. Hoewel dit niet verplicht is, wordt deze verminderen de impact van opstoppingen in het netwerk.
* Wanneer met behulp van software gebaseerd coderingsprogramma's, sluit u alle onnodige programma's.

## <a name="elemental-live-with-rtp-ingest"></a>Elementaire Live met RTP opnemen
Deze sectie toont hoe u configureert het elementaire Live coderingsprogramma dat een single-bitrate live stream via RTP verzendt.  Zie voor meer informatie [MPEG-TS-stream via RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Een kanaal maken

1. Navigeer in het AMSE-hulpprogramma naar het **Live** tabblad en klik met de rechtermuisknop in het gebied van kanaal. Selecteer **kanaal maken...** in het menu.

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Geef een kanaalnaam het beschrijvingsveld is optioneel. Selecteer onder instellingen voor kanaal **standaard** voor de optie Live Encoding met het invoer-Protocol die is ingesteld op **RTP (MPEG-TS)**. U kunt alle andere instellingen zoals is laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> Het kanaal kan zo lang starten 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart. u kunt [configureren van het coderingsprogramma](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

> [!IMPORTANT]
> Houd er rekening mee dat omdat facturering begint zodra kanaal probeert het gereed. Zie voor meer informatie [van kanaal statussen](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

### <a id=configure_elemental_rtp></a>Het elementaire Live coderingsprogramma configureren
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

#### <a name="configuration-steps"></a>Configuratiestappen
1. Navigeer naar de **elementair Live** webinterface en instellen van het coderingsprogramma voor **UDP/TS** streaming.
2. Wanneer een nieuwe gebeurtenis is gemaakt, bladert u omlaag naar de uitvoer-groepen en toevoegen de **UDP/TS** uitvoer groep.
3. De uitvoer van een nieuwe maken door te selecteren **nieuwe Stream** en vervolgens te klikken op **uitvoer toevoegen**.  

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > Het wordt aanbevolen dat de elementair gebeurtenis de tijdcode ingesteld op 'Systeemklok heeft' om u te helpen bij het coderingsprogramma opnieuw verbinding maken in het geval van een storing van de stroom.
   >
   >
4. Nu dat de uitvoer is gemaakt, klikt u op **stroom toevoegen**. De uitvoerinstellingen kunnen nu worden geconfigureerd.
5. Schuif omlaag naar 'Stream 1' die is gemaakt, klikt u op de **Video** tabblad aan de linkerkant en vouw de **Geavanceerd** gedeelte instellingen.

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Terwijl elementair Live een breed scala heeft aan beschikbare aanpassen, worden de volgende instellingen worden aanbevolen voor aan de slag met streaming aan AMS.

   * Oplossing: 1280 x 720
   * Framesnelheid: 30
   * GOP grootte: 60 frames
   * Modus interliniëren: progressief
   * Bitrate: 5000000 bits/s (dit kan worden aangepast op basis van de beperkingen op het netwerk)

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. Ophalen van het kanaal invoer-URL.

    Ga terug naar het AMSE-hulpprogramma, en controleren van de voltooiingsstatus van kanaal. Zodra de status is gewijzigd van **starten** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, klikt u met de rechtermuisknop op naam van het kanaal, navigeer naar aanwijzen via **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. Plak deze informatie in de **primaire doel** veld van de vorm van het element. Alle andere instellingen kunnen blijven de standaardwaarde.

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Herhaal deze stappen aan de secundaire invoer-URL voor het maken van een afzonderlijke tabblad 'Uitvoer' voor het UDP/TS Streaming voor extra redundantie.
3. Klik op **maken** (als een nieuwe gebeurtenis is gemaakt) of **Update** (als een bestaande gebeurtenis bewerken) en ga verder met het starten van de encoder.

> [!IMPORTANT]
> Voordat u op **Start** op de webinterface elementair Live u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat niet het kanaal in een status gereed zonder een gebeurtenis voor langer dan 15 minuten >.
>
>

Nadat de stroom actief is geweest gedurende 30 seconden, gaat u terug naar het AMSE-hulpprogramma's en test afspelen.  

### <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en klikt u met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview** en selecteer **met Azure Media Player**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Als de stroom wordt weergegeven in de speler, is klikt u vervolgens het coderingsprogramma juist geconfigureerd voor verbinding met AMS.

Als een fout wordt ontvangen, wordt het kanaal moet opnieuw worden ingesteld en instellingen voor codering aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) onderwerp voor hulp.   

### <a name="create-a-program"></a>Een programma maken
1. Nadat het kanaal afspelen is bevestigd, maak een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, klik met de rechtermuisknop in het gebied van het programma en selecteer **nieuw programma maken**.  

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. Naam van het programma en wijzig indien nodig de **lengte van een archiefvenster** (die standaard 4 uur). U kunt ook opgeven van een opslaglocatie of laat de standaardwaarde.  
3. Controleer de **Start het programma nu** vak.
4. Klik op **programma maken**.  

    >[!NOTE]
    > Maken van het programma kost minder tijd dan het maken van kanaal.   
      
5. Zodra het programma wordt uitgevoerd, bevestigt u afspelen door te klikken met de rechtermuisknop op het programma en te navigeren naar **afspelen van de programma's** en selecteren **met Azure Media Player**.  
6. Zodra bevestigd, klik met de rechtermuisknop het programma opnieuw en selecteer **Kopieer de URL van de uitvoer naar Klembord** (of het ophalen van deze informatie van de **programma gegevens en instellingen** optie in het menu).

De stroom is nu gereed om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor live weer te geven.  

## <a name="troubleshooting"></a>Problemen oplossen
Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) onderwerp voor hulp.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
