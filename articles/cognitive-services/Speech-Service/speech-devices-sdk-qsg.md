---
title: Aan de slag met de SDK van de apparaten spraak - spraakservices
titleSuffix: Azure Cognitive Services
description: Vereisten en instructies voor het aan de slag met de SDK van de apparaten spraak.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 46f7762a815a7fa4aa4663d9ac6e7c6001ea345c
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097179"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Aan de slag met de SDK van de apparaten spraak

In dit artikel wordt beschreven hoe u uw PC-ontwikkeling en spraak apparaat development kit voor het ontwikkelen van spraak ingeschakelde apparaten met behulp van de SDK van spraak apparaten configureren. Vervolgens u bouw en implementeer een voorbeeld van toepassing op het apparaat.

De broncode voor de voorbeeldtoepassing is opgenomen in de SDK van de apparaten spraak. Het is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Voordat u begint met ontwikkelen met de SDK van de apparaten spraak, verzamel de informatie en de software die u nodig hebt:

* Krijgen een [development kit van ROOBO](http://ddk.roobo.com/). Kits zijn beschikbaar met lineaire of ronde microfoon matrix configuraties. Kies de juiste configuratie voor uw behoeften.

    |Development kit configuratie|Locatie van de spreker|
    |-----------------------------|------------|
    |Kringverwijzing|Elke richting van het apparaat|
    |Lineair|In het zicht van het apparaat|

* De meest recente versie van de SDK van de apparaten spraak, die voorziet in een Android-voorbeeld-app van de [site voor het downloaden van spraak Devices SDK](https://shares.datatransfer.microsoft.com/). Pak het ZIP-bestand naar een lokale map, zoals C:\SDSDK.

* Installeer [Android Studio](https://developer.android.com/studio/) en [Vysor](http://vysor.io/download/) op uw PC.

* Krijgen een [Speech-service-abonnementssleutel](get-started.md). U kunt een gratis proefversie van 30 dagen of u een sleutel van uw Azure-dashboard.

* Als u gebruiken van de spraakservice intentieherkenning wilt, zich abonneren op de [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) en [ophalen van een abonnementssleutel](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription).

    U kunt [maken een eenvoudige LUIS-model](https://docs.microsoft.com/azure/cognitive-services/luis/) of gebruik het voorbeeld LUIS-model, LUIS-example.json. Het voorbeeld LUIS-model beschikbaar via is de [site voor het downloaden van spraak Devices SDK](https://shares.datatransfer.microsoft.com/). Voor het uploaden van uw model JSON-bestand naar de [LUIS portal](https://www.luis.ai/home), selecteer **importeren nieuwe app**, en selecteer vervolgens het JSON-bestand.

## <a name="set-up-the-development-kit"></a>Instellen van de development kit

1. Verbinding maken met de development kit op een PC of het samenstellen van de adapter met behulp van een miniwerkbalk USB-kabel. Wanneer de kit is verbonden, beschikbaar een groene power-indicator onder het bovenste bord.

1. De development kit verbinden met een computer met behulp van een tweede mini USB-kabel.

    ![de dev kit verbinding te maken](media/speech-devices-sdk/qsg-1.png)

1. Uw development kit voor de configuratie van cirkelvormige of lineaire plaatsen.

    |Development kit configuratie|Afdrukstand|
    |-----------------------------|------------|
    |Kringverwijzing|Rechtop, met microfoons geconfronteerd met het maximum|
    |Lineair|Aan de zijde met microfoons gericht u (weergegeven in de volgende afbeelding)|

    ![lineaire dev kit afdrukstand](media/speech-devices-sdk/qsg-2.png)

1. Installeer de certificaten en het bestand wake word (trefwoord) tabel en stel de machtigingen van het apparaat. Typ de volgende opdrachten in een opdrachtpromptvenster:

   ```
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Deze opdrachten gebruiken de brug Android foutopsporing `adb.exe`, deze maakt deel uit van de Android Studio-installatie. Dit hulpprogramma bevindt zich in C:\Users\[gebruikersnaam] \AppData\Local\Android\Sdk\platform-hulpprogramma's. U kunt deze map toevoegen aan het pad naar het eenvoudig om aan te roepen kunnen `adb`. Anders moet u het volledige pad naar uw installatie van adb.exe in elke opdracht die wordt aangeroepen `adb`.

    > [!TIP]
    > De microfoon van uw PC en de spreker om ervoor te zorgen dat u werkt met de development kit microfoons dempen. Op deze manier wordt niet u per ongeluk het apparaat met audio van de PC activeren.

1.  Vysor op uw computer starten.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Uw apparaat moet worden weergegeven onder **Kies een apparaat**. Selecteer de **weergave** knop naast het apparaat.

1.  Verbinding maken met het draadloze netwerk door het mappictogram te selecteren en selecteer vervolgens **instellingen** > **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)

    > [!NOTE]
    > Als uw bedrijf beleid heeft over het verbinden van apparaten met het Wi-Fi-systeem, moet u het MAC-adres verkrijgen en neem contact op met uw IT-afdeling over het verbinden van uw bedrijf Wi-Fi.
    >
    > Als u het MAC-adres van de dev kit zoekt, selecteer het mappictogram op het bureaublad van de dev kit.
    >
    >  ![Bestandsmap Vysor](media/speech-devices-sdk/qsg-10.png)
    >
    > Selecteer **instellingen**. Zoek naar 'mac-adres' en selecteer vervolgens **Mac-adres** > **WLAN geavanceerde**. Noteer het MAC-adres dat wordt weergegeven aan de onderkant van het dialoogvenster.
    >
    > ![Vysor MAC-adres](media/speech-devices-sdk/qsg-11.png)
    >
    > Sommige bedrijven hebben mogelijk een tijdslimiet op hoe lang een apparaat kan zijn verbonden met de Wi-Fi-systeem. Mogelijk moet u de registratie van de dev kit met uw Wi-Fi-systeem uitbreiden na een bepaald aantal dagen.
    >
    > Als u wilt een spreker koppelen aan de dev kit, kunt u het verbinding maken met de lijn-audio uit. U moet een goede kwaliteit, 3.5-mm-spreker kiezen.
    >
    > ![Vysor audio](media/speech-devices-sdk/qsg-14.png)

## <a name="run-a-sample-application"></a>Een voorbeeld-App uitvoeren

De ROOBO tests uitvoeren en uw development kit valideren, bouwen en installeren van de voorbeeldtoepassing:

1.  Android Studio starten.

1.  Selecteer **Open een bestaand Android Studio project**.

    ![Android Studio - een bestaand project openen](media/speech-devices-sdk/qsg-5.png)

1.  Ga naar C:\SDSDK\Android-Sample-Release\example. Selecteer **OK** om de voorbeeldproject te openen.

1.  Uw abonnementssleutel spraak toevoegen aan de broncode. Als u proberen intentieherkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id.

    De toepassingsgegevens van uw sleutels en gaat u in de volgende regels in het bronbestand MainActivity.java:

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app ID]"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'. U kunt ook een van de opgegeven andere woorden, zoals 'Machine' of 'Assistent' activeren. De bronbestanden voor deze alternatieve wake-woorden zijn in de SDK van de spraak-apparaten in de map trefwoord. C:\SDSDK\Android-Sample-Release\keyword\Computer bevat bijvoorbeeld de bestanden die worden gebruikt voor de wake-woord 'Computer'.

    U kunt ook [maken van een aangepaste wake-woord](speech-devices-sdk-create-kws.md).

    Voor het installeren van het woord wake die u wilt gebruiken:

    * Maak een map sleutelwoord in de gegevensmap op het apparaat door het uitvoeren van de volgende opdrachten in een opdrachtpromptvenster:

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Kopieer de bestanden kws.table, kws_k.fst en words_kw.txt naar \data\keyword-map van het apparaat. Voer de volgende opdrachten in een opdrachtpromptvenster. Als u hebt gemaakt een [aangepaste wake word](speech-devices-sdk-create-kws.md), het kws.table-bestand gegenereerd op basis van de website is in dezelfde map als de bestanden kws.table kws_k.fst en words_kw.txt. Voor een aangepaste wake-woord, gebruikt u de `adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table /data/keyword` opdracht naar het bestand kws.table push naar de dev kit:

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```

    * Verwijzen naar deze bestanden in de voorbeeldtoepassing. De volgende regels in MainActivity.java vinden. Zorg ervoor dat het opgegeven trefwoord is de URL die u gebruikt en dat het pad verwijst naar de `kws.table` -bestand dat u naar het apparaat gepusht.

        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > In uw eigen code kunt u het bestand kws.table te maken van een sleutelwoord model exemplaar opname starten:
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  De volgende regels de microfoon matrix geometrie-instellingen bevatten bijwerken:

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```
    De volgende tabel beschrijft de beschikbare waarden:

    |Variabele|Betekenis|Beschikbare waarden|
    |--------|-------|----------------|
    |`DeviceGeometry`|Fysieke mic-configuratie|Voor een circulaire dev kit: `Circular6+1` |
    |||Voor een lineaire dev kit: `Linear4`|
    |`SelectedGeometry`|De softwareconfiguratie voor de mic|Voor een circulaire dev kit die gebruikmaakt van alle microfoon: `Circular6+1`|
    |||Voor een circulaire dev kit die gebruikmaakt van vier microfoon: `Circular3+1`|
    |||Voor een lineaire dev kit die gebruikmaakt van alle microfoon: `Linear4`|
    |||Voor een lineaire dev kit die gebruikmaakt van twee microfoon: `Linear2`|


1.  De toepassing te bouwen, op de **uitvoeren** in het menu **uitvoeren 'app'**. De **implementatiedoel Selecteer** in het dialoogvenster wordt weergegeven.

1. Selecteer uw apparaat en selecteer vervolgens **OK** om de toepassing op het apparaat te implementeren.

    ![Het dialoogvenster implementatiedoel selecteren](media/speech-devices-sdk/qsg-7.png)

1.  De voorbeeldtoepassing met spraak Devices SDK wordt gestart en worden de volgende opties weergegeven:

    ![Voorbeeld van de voorbeeldtoepassing spraak Devices SDK en opties](media/speech-devices-sdk/qsg-8.png)

1. Experiment!

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="certificate-failures"></a>Certificaatfouten

Als er certificaatfouten wanneer u de Speech-service gebruikt, zorg ervoor dat uw apparaat de juiste datum en tijd heeft:

1. Ga naar **instellingen**. Onder **System**, selecteer **datum en tijd**.

    ![Selecteer onder instellingen voor datum en tijd](media/speech-devices-sdk/qsg-12.png)

1. Houd de **automatische datum en tijd** optie is geselecteerd. Onder **Selecteer tijdzone**, selecteert u uw huidige tijdzone.

    ![Selecteer opties voor datum en tijdzone](media/speech-devices-sdk/qsg-13.png)

    Wanneer u ziet dat de ontwikkelingsset tijd overeenkomt met de tijd op uw PC, wordt de dev kit is verbonden met internet.

    Zie voor meer informatie over ontwikkelingen, de [ROOBO development guide](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

### <a name="audio"></a>Audio

ROOBO voorziet in een hulpprogramma waarmee alle audio Flash-geheugen wordt vastgelegd. Het kan helpen bij het oplossen van problemen met audio. Een versie van het hulpprogramma is opgegeven voor de configuratie van elke development kit. Op de [ROOBO site](http://ddk.roobo.com/), selecteer uw apparaat en selecteer vervolgens de **ROOBO extra** koppelen aan de onderkant van de pagina.
