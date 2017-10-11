---
title: Configureren van het coderingsprogramma FMLE voor het verzenden van een single-bitrate live stream | Microsoft Docs
description: Dit onderwerp leest hoe u configureert het coderingsprogramma Flash Media Live coderingsprogramma (FMLE) voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e831048f34ecf6e89595adc4bfd58b5977e04bdb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Het coderingsprogramma FMLE gebruiken voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Live elemental](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Dit onderwerp wordt beschreven hoe u configureert de [Media Flash Live coderingsprogramma](http://www.adobe.com/products/flash-media-encoder.html) (FMLE)-codering verzenden een single-bitrate stream naar AMS kanalen die zijn ingeschakeld voor live codering. Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u op Mac- of Linux, gebruikt u de Azure portal maken [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

Houd er rekening mee dat deze zelfstudie wordt beschreven hoe AAC. FMLE ondersteunt niet echter AAC standaard. U moet aanschaffen van een invoegtoepassing voor AAC codering bijvoorbeeld van MainConcept: [AAC-invoegtoepassing](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

## <a name="prerequisites"></a>Vereisten
* [Een Azure Media Services-account maken](media-services-portal-create-account.md)
* Zorg dat er een Streaming-eindpunt is uitgevoerd. Zie voor meer informatie [Streaming-eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)
* Installeer de nieuwste versie van de [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) hulpprogramma.
* Het hulpprogramma voor starten en verbinding maken met uw AMS-account.

## <a name="tips"></a>Tips
* Gebruik indien mogelijk een internetverbinding ' hardwired '.
* Een goede vuistregel bij het bepalen van de vereiste bandbreedte is voor de streaming bitsnelheden verdubbelen. Hoewel dit niet verplicht is, wordt deze verminderen de impact van opstoppingen in het netwerk.
* Wanneer met behulp van software gebaseerd coderingsprogramma's, sluit u alle onnodige programma's.

## <a name="create-a-channel"></a>Een kanaal maken
1. Navigeer in het AMSE-hulpprogramma naar het **Live** tabblad en klik met de rechtermuisknop in het gebied van kanaal. Selecteer **kanaal maken...** in het menu.

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Geef een kanaalnaam het beschrijvingsveld is optioneel. Selecteer onder instellingen voor kanaal **standaard** voor de optie Live Encoding met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen zoals is laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> Het kanaal kan zo lang starten 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart. u kunt [configureren van het coderingsprogramma](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

> [!IMPORTANT]
> Houd er rekening mee dat omdat facturering begint zodra kanaal probeert het gereed. Zie voor meer informatie [van kanaal statussen](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_fmle_rtmp></a>Het coderingsprogramma FMLE configureren
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
1. Navigeer naar het Flash Media Live coderingsprogramma van (FMLE) interface op de computer die wordt gebruikt.

    De interface is één hoofdpagina van instellingen. Let op de volgende instellingen om te beginnen met het streamen FMLE met aanbevolen.

   * Indeling: H.264 framesnelheid: 30,00
   * De grootte van de invoer: 1280 x 720
   * Bitsnelheid: 5000 Kbps (kan worden aangepast op basis van de beperkingen op het netwerk)  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     Bronnen met behulp van interlaced, neemt u de optie 'Zonder interliniëring' vinkje
2. Selecteer de moersleutelpictogram naast indeling, moeten deze extra instellingen:

   * Profiel: Main
   * Niveau: 4.0
   * Keyframe frequentie: 2 seconden

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. Stel de volgende belangrijke audio-instelling:

   * Indeling: AAC
   * Samplefrequentie: 44100 Hz
   * Bitrate: 192 Kbps

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. Get het kanaal de invoer-URL om te kunnen toewijzen aan de FMLE **RTMP eindpunt**.

    Ga terug naar het AMSE-hulpprogramma, en controleren van de voltooiingsstatus van kanaal. Zodra de status is gewijzigd van **starten** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, klikt u met de rechtermuisknop op naam van het kanaal, navigeer naar aanwijzen via **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. Plak deze informatie in de **FMS URL** veld van de sectie uitvoer, en wijst u de naam van een gegevensstroom.

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Herhaal deze stappen aan de secundaire invoer-URL voor extra redundantie.
6. Selecteer **Verbinden**.

> [!IMPORTANT]
> Voordat u op **Connect**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet het kanaal in een status ready heeft verlaten zonder een invoer bijdrage feed langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en klikt u met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview** en selecteer **met Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Als de stroom wordt weergegeven in de speler, is klikt u vervolgens het coderingsprogramma juist geconfigureerd voor verbinding met AMS.

Als een fout wordt ontvangen, wordt het kanaal moet opnieuw worden ingesteld en instellingen voor codering aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) onderwerp voor hulp.  

## <a name="create-a-program"></a>Een programma maken
1. Nadat het kanaal afspelen is bevestigd, maak een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, klik met de rechtermuisknop in het gebied van het programma en selecteer **nieuw programma maken**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. Naam van het programma en wijzig indien nodig de **lengte van een archiefvenster** (die standaard 4 uur). U kunt ook opgeven van een opslaglocatie of laat de standaardwaarde.  
3. Controleer de **Start het programma nu** vak.
4. Klik op **programma maken**.  

    >[!NOTE]
    >Maken van het programma kost minder tijd dan het maken van kanaal.
        
5. Zodra het programma wordt uitgevoerd, bevestigt u afspelen door te klikken met de rechtermuisknop op het programma en te navigeren naar **afspelen van de programma's** en selecteren **met Azure Media Player**.  
6. Zodra bevestigd, klik met de rechtermuisknop het programma opnieuw en selecteer **Kopieer de URL van de uitvoer naar Klembord** (of het ophalen van deze informatie van de **programma gegevens en instellingen** optie in het menu).

De stroom is nu gereed om te worden ingesloten in een speler of gedistribueerd naar een doelgroep voor live weer te geven.  

## <a name="troubleshooting"></a>Problemen oplossen
Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) onderwerp voor hulp.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
