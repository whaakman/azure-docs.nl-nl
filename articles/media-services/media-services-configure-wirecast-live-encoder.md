---
title: Configureer de Telestream Wirecast-coderingsprogramma voor het verzenden van een single-bitrate live stream | Microsoft Docs
description: 'Dit onderwerp leest hoe u configureert het live coderingsprogramma Wirecast voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering. '
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 0e4fb0b7c915969da1760eaccc77aa399030752e
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>De Wirecast-coderingsprogramma gebruiken voor het verzenden van een single-bitrate live stream
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Live elemental](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Dit artikel laat zien hoe u configureert de [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live codering verzendt een single-bitrate stream naar AMS kanalen die zijn ingeschakeld voor live codering.  Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

Deze zelfstudie laat zien hoe Azure Media Services (AMS) beheren met Azure Media Services Explorer (AMSE)-hulpprogramma. Dit hulpprogramma wordt alleen uitgevoerd op Windows-PC. Als u op Mac- of Linux, gebruikt u de Azure portal maken [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md).

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

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Geef een kanaalnaam het beschrijvingsveld is optioneel. Selecteer onder instellingen voor kanaal **standaard** voor de optie Live Encoding met het invoer-Protocol die is ingesteld op **RTMP**. U kunt alle andere instellingen zoals is laten.

    Zorg ervoor dat de **nu starten van het nieuwe kanaal** is geselecteerd.

3. Klik op **kanaal maken**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Het kanaal kan zo lang starten 20 minuten duren.
>
>

Terwijl het kanaal wordt gestart, kunt u [configureren van het coderingsprogramma](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Omdat facturering begint zodra kanaal probeert het gereed. Zie voor meer informatie [van kanaal statussen](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>De Telestream Wirecast-coderingsprogramma configureren
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
1. Open de Telestream Wirecast-toepassing op de computer die wordt gebruikt en instellen voor het RTMP-streaming.
2. De uitvoer configureren door te navigeren naar de **uitvoer** tabblad en het selecteren van **uitvoerinstellingen... **.

    Zorg ervoor dat de **bestemming voor de uitvoer** is ingesteld op **RTMP Server**.
3. Klik op **OK**.
4. Op de instellingenpagina stelt de **bestemming** veld **Azure Media Services**.

    Het profiel codering is vooraf geselecteerd om te **Azure H.264 720 p 16:9 (1280 x 720)**. Selecteer het pictogram tandwielpictogram rechts van de vervolgkeuzelijst voor het aanpassen van deze instellingen en kies vervolgens **nieuwe voorinstelling**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Standaardinstellingen voor codering configureren.

    Naam van de definitie en controleren voor de volgende aanbevolen instellingen:

    **Video**

   * Codering: MainConcept H.264
   * Frames per seconde: 30
   * Gemiddelde bitsnelheid: 5000 kbits per seconde (kan worden aangepast op basis van de beperkingen op het netwerk)
   * Profiel: Main
   * Sleutelframe elke: 60 frames

    **Audio**

   * Doelbitsnelheid: 192 kbits per seconde
   * Samplefrequentie: 44,100 kHz

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Druk op **opslaan**.

    Het veld Encoding heeft nu het nieuwe profiel beschikbaar voor selectie.

    Zorg ervoor dat het nieuwe profiel is geselecteerd.
7. Get het kanaal de invoer-URL om te kunnen toewijzen aan de Wirecast **RTMP eindpunt**.

    Ga terug naar het AMSE-hulpprogramma, en controleren van de voltooiingsstatus van kanaal. Zodra de status is gewijzigd van **starten** naar **met**, krijgt u de invoer-URL.

    Wanneer het kanaal wordt uitgevoerd, met de rechtermuisknop op de naam van het kanaal, navigeer naar aanwijzen via **invoer-URL kopiëren naar Klembord** en selecteer vervolgens **primaire invoer-URL**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. In de Wirecast **uitvoerinstellingen** venster plakt u deze informatie in de **adres** veld van de sectie uitvoer, en wijst u de naam van een gegevensstroom.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Selecteer **OK**.
2. Op de hoofdblade **Wirecast** scherm, invoerbronnen bevestigen voor video en audio gereed zijn en klik vervolgens op **stroom** in de bovenhoek links.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Voordat u op **stroom**, u **moet** ervoor te zorgen dat het kanaal gereed is.
> Zorg ervoor dat u niet het kanaal in een status ready heeft verlaten zonder een invoer bijdrage feed langer dan 15 minuten >.
>
>

## <a name="test-playback"></a>Test afspelen

Navigeer naar het AMSE-hulpprogramma en met de rechtermuisknop op het kanaal moet worden getest. In het menu Beweeg de muisaanwijzer over **afspelen van de Preview** en selecteer **met Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Als de stroom wordt weergegeven in de speler, is klikt u vervolgens het coderingsprogramma juist geconfigureerd voor verbinding met AMS.

Als een fout wordt ontvangen, wordt het kanaal moet opnieuw worden ingesteld en instellingen voor codering aangepast. Zie de [probleemoplossing](media-services-troubleshooting-live-streaming.md) artikel voor instructies.  

## <a name="create-a-program"></a>Een programma maken
1. Nadat het kanaal afspelen is bevestigd, maak een programma. Onder de **Live** tabblad in het AMSE-hulpprogramma, met de rechtermuisknop op het gebied programma en selecteer **nieuw programma maken**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
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

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
