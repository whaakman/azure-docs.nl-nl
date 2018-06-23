---
title: Aan de slag met de SDK van de apparaten spraak | Microsoft Docs
description: Vereisten en instructies voor het aan de slag met de SDK van de apparaten spraak.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 6cc5ff1c532d67c48beac1a2a10d034f5d9d7501
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345831"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Aan de slag met de SDK spraak-apparaten

Dit artikel wordt beschreven hoe u uw PC-ontwikkeling en uw spraak apparaat development kit voor het ontwikkelen van spraak-apparaten met behulp van de SDK van de apparaten spraak configureert. U wordt vervolgens bouwen en implementeren van een voorbeeld van toepassing op het apparaat. 

De broncode voor de voorbeeldtoepassing is opgenomen in de SDK van spraak apparaten en is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Verzamel de informatie en software die u nodig hebt voordat u begint ontwikkeling met de SDK-spraak, apparaten.

* Verkrijgen van een development kit [van Roobo](http://ddk.roobo.com/). Kits zijn beschikbaar met lineaire of ronde microfoon matrix configuraties. Kies de juiste is voor uw behoeften.

    |Development kit configuratie|Locatie van de spreker|
    |-----------------------------|------------|
    |Cirkelvormige|Een willekeurige richting van het apparaat|
    |Lineair|Voor het apparaat|

* De meest recente versie van de SDK spraak apparaten, met inbegrip van een Android-voorbeeld-app uit de spraak apparaten SDK [downloaden site](https://shares.datatransfer.microsoft.com/). Pak het ZIP-bestand naar een lokale map (zoals `C:\SDSDK`).

* Installeer [Android Studio](https://developer.android.com/studio/) en [Vysor](http://vysor.io/download/) op uw PC.

* Verkrijgen van een service spraak [abonnementssleutel](get-started.md). U kunt verkrijgen van een gratis proefversie van 30 dagen of ophalen van een sleutel van uw Azure-dashboard.

* Als u gebruiken van de service spraak opzet herkenning wilt, zich abonneren op de [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) en [ophalen van een abonnementssleutel](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    U kunt [een eenvoudige LUIS model maken](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) of gebruik het voorbeeld LUIS model `LUIS-example.json`, beschikbaar is via de SDK van de apparaten spraak [downloaden van de site](https://shares.datatransfer.microsoft.com/). Uploaden van uw model-JSON-bestand naar de [LUIS portal](https://www.lui.ai/applications) door te klikken op **importeren nieuwe app** en het kiezen van het JSON-bestand.

## <a name="set-up-the-development-kit"></a>Instellen van de development kit

1. Sluit de development kit stroomadapter. Een indicator groen power moet branden onder de bovenste mededelingenbord.

1. De development kit op een computer met een mini USB-kabel aansluiten

    ![verbinding maken met de Developer kit](media/speech-devices-sdk/qsg-1.jpg)

1. Uw development kit op de juiste wijze plaatsen.

    |Development kit configuratie|Afdrukstand|
    |-----------------------------|------------|
    |Cirkelvormige|Verticaal met een microfoon geconfronteerd met het maximum|
    |Lineair|Aan de zijde gerichte met een microfoon u (Zie hieronder)|

    ![lineaire Developer kit afdrukstand](media/speech-devices-sdk/qsg-2.jpg)

1. Installeer de certificaten en het bestand wake word (trefwoord) tabel en stel de machtigingen van het apparaat. Typ de volgende opdrachten in een opdrachtvenster.

    > [!NOTE]
    > Deze opdrachten gebruiken de brug Android-foutopsporing `adb.exe`, die deel uitmaakt van de installatie van de Android Studio. Dit hulpprogramma kunt u vinden in `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. U kunt deze map toevoegen aan het pad naar het eenvoudig om aan te roepen kunnen `adb`. Anders moet u het volledige pad op de installatie van `adb.exe` in elke opdracht die wordt aangeroepen `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > De microfoon van uw PC en de spreker dempen. Op deze manier kunt Wees zeker dat u met de development kit een microfoon werkt en u het apparaat met audio van de PC per ongeluk won't activeren.
    
1.  Start Vysor op uw computer.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Uw apparaat moet worden vermeld onder 'Kies een apparaat'. Klik op de **weergave** knop ernaast. 
 
1.  Verbinding maken met het draadloze netwerk door te klikken op **instellingen**, klikt u vervolgens **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
## <a name="run-a-sample-application"></a>Een voorbeeld van een toepassing uitvoeren

De Roobo tests uitvoeren en de instellingen van uw development kit valideren, bouwen en de voorbeeldtoepassing installeren.

1.  Android Studio starten.

1.  Kies een bestaand Android Studio-project openen.

    ![Android studio - project openen bestaande](media/speech-devices-sdk/qsg-5.png)
 
1.  Blader naar `C:\SDSDK\Android-Sample-Release\example`, klikt u vervolgens op **OK** de voorbeeldproject te openen.
 
1.  De sleutel van uw spraak abonnement toevoegen aan de broncode. Als u uitproberen opzet herkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id. 

    Uw sleutels en probeert het toepassingsinformatie in de volgende regels in het bronbestand `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'.  Indien gewenst, kunt u proberen een van de andere opgegeven wake woorden, 'Machine' en "-assistent." De bronbestanden voor deze alternatieve woorden vindt u in de SDK spraak-apparaten in de map 'sleutelwoord'. Bijvoorbeeld: `C:\SDSDK\Android-Sample-Release\keyword\Computer` bevat de bestanden die worden gebruikt voor 'Computer'.

    U kunt ook [maken van een aangepaste wake woord](speech-devices-sdk-create-kws.md).

    De gewenste wake word installeren:
 
    * Maak een `keyword` map in de map \data\ op het apparaat met de volgende opdrachten in het opdrachtvenster.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Kopieer de bestanden `kws.table`, `kws_g.fst`, `kws_k.fst`, en `words_kw.txt`) naar het apparaat \data\keyword\ map. Voer de volgende opdrachten in het opdrachtvenster o.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Verwijzen naar deze bestanden in de voorbeeldtoepassing. Zoek naar de volgende regels in `MainActivity.java`. Zorg ervoor dat het opgegeven trefwoord wordt de computer die u gebruikt en dat het pad naar verwijst de `kws.table` -bestand dat u naar het apparaat gepusht.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > In uw eigen code kunt u de `kws.table` bestand voor het maken van een sleutelwoord model-exemplaar en start erkenning als volgt.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  De volgende regels met de microfoon matrix geometrie-instellingen bijwerken.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variabele|Betekenis|Beschikbare waarden|
    |--------|-------|----------------|
    |`DeviceGeometry`|Fysieke mic-configuratie|`Circular6+1` voor circulaire Developer kit|
    ||| `Linear4` voor lineaire Developer kit|
    |`SelectedGeometry`|De softwareconfiguratie voor de mic|`Circular6+1` met behulp van alle microfoons voor circulaire Developer kit|
    |||`Circular3+1` met behulp van vier microfoons voor circulaire Developer kit|
    |||`Linear4` met behulp van alle microfoons voor lineaire Developer kit|
    |||`Linear2` met behulp van twee microfoons voor lineaire Developer kit|


1.  De toepassing bouwen door te kiezen **uitvoeren 'app'** in het menu uitvoeren. Het dialoogvenster implementatiedoel selecteren wordt weergegeven. Kies uw apparaat en klikt u op **OK** om de toepassing op het apparaat te implementeren.

    ![implementatiedoel selecteren](media/speech-devices-sdk/qsg-7.png)
 
1.  De voorbeeldtoepassing spraak apparaten SDK wordt gestart, het weergeven van de opties die hier worden weergegeven.

    ![voorbeeldtoepassing spraak apparaat](media/speech-devices-sdk/qsg-8.png)

1. Uitproberen rond!

## <a name="troubleshooting"></a>Probleemoplossing

Als u certificaat fouten krijgt bij het gebruik van de service spraak, zorg er dan voor dat het apparaat heeft de juiste datum en tijd.

Zie voor meer informatie over ontwikkelingen, van Roobo [ontwikkeling handleiding](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo voorziet in een hulpprogramma waarmee alle audio Flash-geheugen, wat helpen kan bij het oplossen van problemen met audio wordt vastgelegd. Een versie van het hulpprogramma is beschikbaar voor elke development kit-configuratie. Kies uw apparaat op [de site Roobo](http://ddk.roobo.com/), klikt u vervolgens op de **ROOBO extra** koppeling aan de onderkant van de pagina.
