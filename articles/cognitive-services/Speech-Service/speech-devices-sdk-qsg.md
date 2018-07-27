---
title: Aan de slag met de SDK van de apparaten spraak
description: Vereisten en instructies voor het aan de slag met de SDK van de apparaten spraak.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 266315a731eec8a2c0ab0a880ce9e1db58331184
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283133"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Aan de slag met de SDK van de apparaten spraak

In dit artikel wordt beschreven hoe u ontwikkeling van uw PC en uw spraak apparaat development kit voor het ontwikkelen van spraak ingeschakelde apparaten met behulp van de SDK van spraak apparaten configureren. U wordt vervolgens bouwen en implementeren van een voorbeeld van toepassing op het apparaat. 

De broncode voor de voorbeeldtoepassing is opgenomen in de SDK van de apparaten spraak en is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Verzamel de informatie en de software u moet, voordat u begint met ontwikkelen met de SDK van de apparaten spraak.

* Verkrijgen van een development kit [van Roobo](http://ddk.roobo.com/). Kits zijn beschikbaar voor lineaire of ronde microfoon matrix configuraties. Kies de juiste oplossing voor uw behoeften.

    |Development kit configuratie|Locatie van de spreker|
    |-----------------------------|------------|
    |Kringverwijzing|Elke richting van het apparaat|
    |Lineair|In het zicht van het apparaat|

* De meest recente versie van de apparaten SDK, spraak, met inbegrip van een Android-voorbeeld-app van de spraakherkenning apparaten SDK [downloadsite](https://shares.datatransfer.microsoft.com/). Pak het ZIP-bestand naar een lokale map (zoals `C:\SDSDK`).

* Installeer [Android Studio](https://developer.android.com/studio/) en [Vysor](http://vysor.io/download/) op uw PC.

* Een spraakservice ophalen [abonnementssleutel](get-started.md). U kunt verkrijgen van een gratis proefversie van 30 dagen of ophalen van een sleutel in uw Azure-dashboard.

* Als u gebruiken van de spraakservice intentieherkenning wilt, zich abonneren op de [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) en [ophalen van een abonnementssleutel](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    U kunt [maken een eenvoudige LUIS-model](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) of gebruik het voorbeeld LUIS-model, `LUIS-example.json`, beschikbaar is via de SDK van de apparaten spraak [site downloaden](https://shares.datatransfer.microsoft.com/). Uploaden van uw model JSON-bestand naar de [LUIS portal](https://www.luis.ai/home) door te klikken op **importeren nieuwe app** en het kiezen van het JSON-bestand.

## <a name="set-up-the-development-kit"></a>Instellen van de development kit

1. Sluit de development kit power-adapter. Een indicator groen power moet onder het bovenste bord beschikbaar stellen.

1. De development kit verbinden met een computer met behulp van een mini USB-kabel.

    ![de dev kit verbinding te maken](media/speech-devices-sdk/qsg-1.jpg)

1. Plaatsen op de juiste wijze uw development kit.

    |Development kit configuratie|Afdrukstand|
    |-----------------------------|------------|
    |Kringverwijzing|Rechtop, met microfoons geconfronteerd met het maximum|
    |Lineair|Aan de zijde, met microfoons u (Zie hieronder)|

    ![lineaire dev kit afdrukstand](media/speech-devices-sdk/qsg-2.jpg)

1. Installeer de certificaten en het bestand wake word (trefwoord) tabel en stel de machtigingen van het apparaat. Typ de volgende opdrachten in een opdrachtvenster.

    > [!NOTE]
    > Deze opdrachten gebruiken de brug Android foutopsporing `adb.exe`, deze maakt deel uit van de Android Studio-installatie. Dit hulpprogramma kunt u vinden in `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. U kunt deze map toevoegen aan uw path zodat het eenvoudiger om aan te roepen `adb`. Anders moet u het volledige pad naar uw installatie van `adb.exe` in elke opdracht die wordt aangeroepen `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > De microfoon van uw PC en de spreker dempen. Op deze manier kunt u zeker zijn u werkt met de development kit microfoons, en u het apparaat met audio van de PC per ongeluk wordt niet activeren.
    
1.  Start Vysor op uw computer.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Uw apparaat moet worden weergegeven onder 'Kies een apparaat'. Klik op de **weergave** knop ernaast. 
 
1.  Verbinding maken met het draadloze netwerk door te klikken op **instellingen**, klikt u vervolgens **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
## <a name="run-a-sample-application"></a>Een voorbeeld-App uitvoeren

Als u wilt de Roobo tests uitvoeren en uw development kit valideren, bouwen en de voorbeeld-App installeren.

1.  Android Studio starten.

1.  Kies een bestaand Android Studio-project openen.

    ![Android studio - project openen bestaande](media/speech-devices-sdk/qsg-5.png)
 
1.  Blader naar `C:\SDSDK\Android-Sample-Release\example`, klikt u vervolgens op **OK** om de voorbeeldproject te openen.
 
1.  Uw abonnementssleutel spraak toevoegen aan de broncode. Als u uitproberen intentieherkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id. 

    Uw sleutels en toepassingsgegevens wordt in de volgende regels in het bronbestand `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'.  Als u dat wilt, kunt u proberen een van de andere opgegeven wake woorden, 'Machine' en 'Assistent'. De bronbestanden voor deze alternatieve woorden kunnen worden gevonden in de SDK van de apparaten spraak in de map 'sleutelwoord'. Bijvoorbeeld, `C:\SDSDK\Android-Sample-Release\keyword\Computer` bevat de bestanden die worden gebruikt voor 'Computer'.

    U kunt ook [maken van een aangepaste wake-woord](speech-devices-sdk-create-kws.md).

    Voor het installeren van de gewenste wake word:
 
    * Maak een `keyword` in de map \data\ op het apparaat door het uitvoeren van de volgende opdrachten in het opdrachtvenster.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Kopieer de bestanden `kws.table`, `kws_g.fst`, `kws_k.fst`, en `words_kw.txt`) van het apparaat \data\keyword\ map. Voer de volgende opdrachten in het opdrachtvenster o.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Verwijzen naar deze bestanden in de voorbeeldtoepassing. Zoek naar de volgende regels in `MainActivity.java`. Zorg ervoor dat het opgegeven trefwoord is de URL die u gebruikt en dat het pad naar verwijst de `kws.table` -bestand dat u naar het apparaat gepusht.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > In uw eigen code kunt u de `kws.table` dat u wilt maken van een sleutelwoord model-exemplaar en start erkenning als volgt.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Werk de volgende regels met de microfoon matrix geometrie-instellingen.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variabele|Betekenis|Beschikbare waarden|
    |--------|-------|----------------|
    |`DeviceGeometry`|Fysieke mic-configuratie|`Circular6+1` voor de ronde dev kit|
    ||| `Linear4` voor lineaire dev kit|
    |`SelectedGeometry`|De softwareconfiguratie voor de mic|`Circular6+1` met behulp van alle microfoons voor circulaire dev kit|
    |||`Circular3+1` met behulp van vier microfoons voor circulaire dev kit|
    |||`Linear4` met behulp van alle microfoons voor lineaire dev kit|
    |||`Linear2` met behulp van twee microfoons voor lineaire dev kit|


1.  Maken van de toepassing door te kiezen **uitvoeren 'app'** in het menu uitvoeren. Het implementatiedoel selecteert u het dialoogvenster wordt weergegeven. Kies uw apparaat en klikt u op **OK** om de toepassing op het apparaat te implementeren.

    ![implementatiedoel selecteren](media/speech-devices-sdk/qsg-7.png)
 
1.  De voorbeeldtoepassing met spraak Devices SDK wordt gestart, voor het weergeven van de opties die hier worden weergegeven.

    ![voorbeeldapparaattoepassing spraak](media/speech-devices-sdk/qsg-8.png)

1. U kunt experimenteren met het!

## <a name="troubleshooting"></a>Problemen oplossen

Als er certificaatfouten bij het gebruik van de spraakservice, zorg er dan voor dat het apparaat heeft de juiste datum en tijd.

Zie voor meer informatie over ontwikkelingen, van Roobo [development guide](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo voorziet in een hulpprogramma waarmee alle audio Flash-geheugen, die helpen kan bij het oplossen van problemen met audio wordt vastgelegd. Een versie van het hulpprogramma is opgegeven voor de configuratie van elke development kit. Kies uw apparaat bij [de site Roobo](http://ddk.roobo.com/), klikt u vervolgens op de **ROOBO extra** koppelen aan de onderkant van de pagina.
