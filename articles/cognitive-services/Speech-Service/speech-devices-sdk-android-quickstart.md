---
title: 'Quickstart: De spraak-apparaten SDK worden uitgevoerd op Android - spraakservices'
titleSuffix: Azure Cognitive Services
description: Vereisten en instructies voor het aan de slag met een spraak-SDK voor Android-apparaten.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 073166a594088bca04d81883247a5880fcbd1cb7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234494"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-android"></a>Quickstart: De voorbeeld-app SDK voor spraak-apparaten worden uitgevoerd op Android

In deze snelstartgids leert u hoe u de spraak-apparaten-SDK voor Android gebruiken om te maken van een product spraak ingeschakeld of worden gebruikt als een [conversatie transcriptie](conversation-transcription-service.md) apparaat.

Deze handleiding is vereist een [Azure Cognitive Services](get-started.md) -account met een resource Speech Services. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

De broncode voor de voorbeeldtoepassing is opgenomen in de SDK van de apparaten spraak. Het is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Voordat u met de SDK van de apparaten spraak begint, moet u naar:

* Volg de instructies met uw [development kit](get-speech-devices-sdk.md) voor het inschakelen van het apparaat.

* Download de nieuwste versie van de [spraak Devices SDK](https://aka.ms/sdsdk-download), en pak het ZIP naar uw werkmap.
   > [!NOTE]
   > Het Android-voorbeeld-Release.zip-bestand bevat de Android-voorbeeld-app en deze snelstartgids wordt ervan uitgegaan dat de app wordt uitgepakt naar C:\SDSDK\Android-Sample-Release

* Om op te halen een [sleutel van de Azure-abonnement voor spraakservices](get-started.md)

* Als u van plan bent te gebruiken van de conversatie transcriptie moet u een [circulaire microfoon apparaat](get-speech-devices-sdk.md) en de service is momenteel alleen beschikbaar voor 'en-US' en 'zh-CN' in de regio's, "centralus" en 'Oost-Aziatische'. U moet een spraak-sleutel in een van deze regio's te gebruiken van de conversatie transcriptie hebben.

* Als u van plan bent de Speech Services gebruiken om te identificeren van intents (of acties) van de gebruiker uitingen, moet u een [Language Understanding Service (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) abonnement. Zie voor meer informatie over LUIS en intentieherkenning [spraak intents met LUIS, herkent C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp).

    U kunt [maken een eenvoudige LUIS-model](https://docs.microsoft.com/azure/cognitive-services/luis/) of gebruik het voorbeeld LUIS-model, LUIS-example.json. Het voorbeeld LUIS-model beschikbaar via is de [site voor het downloaden van spraak Devices SDK](https://aka.ms/sdsdk-luis). Voor het uploaden van uw model JSON-bestand naar de [LUIS portal](https://www.luis.ai/home), selecteer **importeren nieuwe app**, en selecteer vervolgens het JSON-bestand.

* Installeer [Android Studio](https://developer.android.com/studio/) en [Vysor](https://vysor.io/download/) op uw PC.

## <a name="set-up-the-device"></a>Instellen van het apparaat

1. Vysor op uw computer starten.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1. Uw apparaat moet worden weergegeven onder **Kies een apparaat**. Selecteer de **weergave** knop naast het apparaat.

1. Verbinding maken met het draadloze netwerk door het mappictogram te selecteren en selecteer vervolgens **instellingen** > **WLAN**.

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

## <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

Als u wilt uw valideren development kit, bouwen en de voorbeeld-App installeren:

1. Android Studio starten.

1. Selecteer **Open een bestaand Android Studio project**.

   ![Android Studio - een bestaand project openen](media/speech-devices-sdk/qsg-5.png)

1. Ga naar C:\SDSDK\Android-Sample-Release\example. Selecteer **OK** om de voorbeeldproject te openen.

1. Uw abonnementssleutel spraak toevoegen aan de broncode. Als u proberen intentieherkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id.

   Uw gegevens krijgt MainActivity.java voor spraak en LUIS:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Als u conversatie transcriptie zijn ook uw sleutel en de regio spraakgegevens in conversation.java nodig:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'. U kunt ook een van de opgegeven andere woorden, zoals 'Machine' of 'Assistent' activeren. De bronbestanden voor deze alternatieve wake-woorden zijn in de SDK van de spraak-apparaten in de map trefwoord. C:\SDSDK\Android-Sample-Release\keyword\Computer bevat bijvoorbeeld de bestanden die worden gebruikt voor de wake-woord 'Computer'.

   > [!TIP]
   > U kunt ook [maken van een aangepaste wake-woord](speech-devices-sdk-create-kws.md).

    Voor het gebruik van een nieuwe wake-woord, werk de volgende twee regels in `MainActivity.java`, en het woord wake pakket kopiëren naar uw app. Bijvoorbeeld, u het wake-woord 'Computer' van de wake word pakket kws-machine.zip:

   * Kopieer het wake word-pakket naar de map 'C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\'.
   * Update de `MainActivity.java` met het sleutelwoord en naam van het pakket:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. De volgende regels de microfoon matrix geometrie-instellingen bevatten bijwerken:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   Deze tabel bevat de ondersteunde waarden:

   |Variabele|Betekenis|Beschikbare waarden|
   |--------|-------|----------------|
   |`DeviceGeometry`|Fysieke mic-configuratie|Voor een circulaire dev kit: `Circular6+1` |
   |||Voor een lineaire dev kit: `Linear4`|
   |`SelectedGeometry`|De softwareconfiguratie voor de mic|Voor een circulaire dev kit die gebruikmaakt van alle microfoon: `Circular6+1`|
   |||Voor een circulaire dev kit die gebruikmaakt van vier microfoon: `Circular3+1`|
   |||Voor een lineaire dev kit die gebruikmaakt van alle microfoon: `Linear4`|
   |||Voor een lineaire dev kit die gebruikmaakt van twee microfoon: `Linear2`|

1. De toepassing te bouwen, op de **uitvoeren** in het menu **uitvoeren 'app'** . De **implementatiedoel Selecteer** in het dialoogvenster wordt weergegeven.

1. Selecteer uw apparaat en selecteer vervolgens **OK** om de toepassing op het apparaat te implementeren.

    ![Het dialoogvenster implementatiedoel selecteren](media/speech-devices-sdk/qsg-7.png)

1. De voorbeeldtoepassing met spraak Devices SDK wordt gestart en worden de volgende opties weergegeven:

   ![Voorbeeld van de voorbeeldtoepassing spraak Devices SDK en opties](media/speech-devices-sdk/qsg-8.png)

1. Toegevoegde is de conversatie transcriptie demo. Start te transcriberen met sessie starten. Standaard is iedereen Gast, maar als u van deelnemer stem handtekeningen hebben ze in een bestand /video/participants.properties op het apparaat kunnen worden geplaatst. Voor het genereren van het uiterlijk van de handtekening stem op [(SDK) gesprekken transcriberen](how-to-use-conversation-transcription-service.md).

   ![Conversatie transcriptie demotoepassing](media/speech-devices-sdk/qsg-15.png)

1. Experiment!

## <a name="troubleshooting"></a>Problemen oplossen

   Als u geen verbinding met de spraak-apparaat maken. Typ de volgende opdracht in een opdrachtpromptvenster. Een lijst met apparaten wordt geretourneerd:

   ```powershell
    adb devices
   ```

   > [!NOTE]
   > Met deze opdracht maakt gebruik van de Android-Debug-brug, `adb.exe`, deze maakt deel uit van de Android Studio-installatie. Dit hulpprogramma bevindt zich in C:\Users\[gebruikersnaam] \AppData\Local\Android\Sdk\platform-hulpprogramma's. U kunt deze map toevoegen aan het pad naar het eenvoudig om aan te roepen kunnen `adb`. Anders moet u het volledige pad naar uw installatie van adb.exe in elke opdracht die wordt aangeroepen `adb`.
   >
   > Als er een fout `no devices/emulators found` en controleer uw USB-kabel is verbonden en controleer of de kabel van een hoge kwaliteit wordt gebruikt.
   >

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Opmerkingen bij de release bekijken](devices-sdk-release-notes.md)
