---
title: Ophalen van een Twitter-bericht met Azure Functions | Microsoft Docs
description: De sensor beweging gebruiken om te detecteren heen en weer schudden en Azure Functions gebruiken voor het vinden van een willekeurige tweet met een hashtag die u opgeeft
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: 722f350c4f11648753465e302e84949fc340e281
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42054243"
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Schud, laat een Tweet--ophalen van een Twitter-bericht met Azure Functions

In dit project leert u hoe u kunt de sensor beweging gebruiken voor het activeren van een gebeurtenis met behulp van Azure Functions. De app worden opgehaald van een willekeurige tweet met een #hashtag die u in uw schets Arduino configureren. De tweet wordt weergegeven op het scherm DevKit.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) aan:

* Uw DevKit zijn verbonden met Wi-Fi.
* De ontwikkelomgeving voorbereiden.

Een actief Azure-abonnement. Als u niet hebt, kunt u registreren via een van deze methoden:

* Activeer een [gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/)
* Claim uw [Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u een MSDN of Visual Studio-abonnee

## <a name="open-the-project-folder"></a>Open de projectmap

Start de projectmap te openen. 

### <a name="start-vs-code"></a>Start VS Code

* Zorg ervoor dat uw DevKit op uw computer is aangesloten.

* Start VS Code.

* De DevKit verbinden met uw computer.

   > [!NOTE]
   > Bij het starten van VS Code, verschijnt een foutmelding krijgen dat de Arduino IDE of gerelateerde Bord pakket kan niet worden gevonden. Als deze fout treedt op, sluit u VS Code en start de Arduino IDE opnieuw. VS Code moet het pad met Arduino IDE nu correct vinden.

### <a name="open-the-arduino-examples-folder"></a>Open de map Arduino-voorbeelden

Vouw aan de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **ShakeShake**. Een nieuwe VS Code-venster wordt geopend, het weergeven van de projectmap. Als u de sectie MXCHIP AZ3166 niet ziet, zorg ervoor dat uw apparaat goed is aangesloten en Visual Studio Code opnieuw.  
de ![mini-solution-voorbeelden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

U kunt het voorbeeldproject ook openen vanuit het opdrachtenpalet. Klik op `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="provision-azure-services"></a>Azure-services inrichten

In het venster van de oplossing, kunt u uw taak uitvoeren via `Ctrl+P` (Mac OS: `Cmd+P`) door in te voeren `task cloud-provision`.

In de terminal van VS Code en helpt een interactieve vanaf de opdrachtregel u de vereiste Azure-services inrichten:

![cloud-inrichten](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Als de pagina reageert niet meer in de status van de geladen bij het aanmelden bij Azure, raadpleegt u de ["aanmeldingspagina loopt vast" stap in de veelgestelde vragen over het IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure).
 
## <a name="modify-the-hashtag"></a>De #hashtag wijzigen

Open `ShakeShake.ino` en zoekt u deze regel code:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Vervang de tekenreeks `iot` binnen de accolades door uw voorkeur hashtag. De DevKit opgehaald later een willekeurige tweet met de hashtag die u in deze stap opgeeft.

## <a name="deploy-azure-functions"></a>Azure Functions implementeren

Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task cloud-deploy` om te beginnen met het implementeren van de Azure Functions-code:

![cloud implementeren](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Van tijd tot tijd, de Azure-functie mogelijk niet goed. U lost dit probleem wanneer deze zich voordoet, Controleer de ['Fout bij schemacompilatie'-sectie van de IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

Vervolgens maken en uploaden van de apparaatcode.

### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.

2. De terminal vraagt u om in te voeren van de configuratiemodus. Dit doet u als volgt:

   * Houd een

   * Push- en release van de knop opnieuw instellen.

3. Het scherm wordt weergegeven de DevKit-ID en 'Configuration'.

### <a name="macos"></a>macOS

1. De DevKit in configuratiemodus geplaatst:

   Houd ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven dat 'Configuration'.

2. Gebruik `Cmd+P` om uit te voeren `task device-upload` om in te stellen van de verbindingsreeks die is opgehaald uit de `task cloud-provision` stap.

### <a name="verify-upload-and-run"></a>Controleren, uploaden en uitvoeren

Nu de verbindingsreeks is ingesteld, het wordt gecontroleerd en uploadt de app, en vervolgens wordt uitgevoerd. 

1. VS Code begint te controleren en de schets Arduino uploaden naar uw DevKit:

   ![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)

2. De DevKit opnieuw wordt opgestart en de code wordt gestart.

Krijgt u mogelijk een ' fout: AZ3166: onbekend pakket "foutbericht. Deze fout treedt op wanneer de bord package index correct is niet vernieuwd. U lost dit probleem, Controleer de ['Onbekend pakket'-fout in de veelgestelde vragen over het IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Na de initialisatie van de app, klikt u op en een release vervolgens voorzichtig Schud het bord DevKit. Deze actie haalt u een willekeurige tweet, waarin de hashtag die u eerder hebt opgegeven. Een tweet wordt binnen een paar seconden weergegeven op het scherm DevKit:

### <a name="arduino-application-initializing"></a>Arduino toepassing initialiseren...

![Het initialiseren van Arduino-toepassing](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Druk op een te schudden...

![Druk op-een-op-schudden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Gereed om te schudden...

![Gereed om te schudden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>Wordt verwerkt...

![Verwerken](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Op B drukken om te lezen...

![Druk op B te lezen](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Een willekeurige tweet weergeven...

![Weergave een willekeurige tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Druk op de knop een opnieuw en klik vervolgens schudden voor een nieuwe tweet.
- Druk op de knop B om door de rest van de tweet te bladeren.

## <a name="how-it-works"></a>Hoe werkt het?

![Diagram](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

De schets Arduino Hiermee verzendt u een gebeurtenis voor de Azure IoT Hub. Deze gebeurtenis wordt geactiveerd voor de Azure Functions-app. De Azure Functions-app bevat de logica voor het verbinding maken met de Twitter-API en op te halen van een tweet. Vervolgens wordt de tekst van de tweet verpakt in een C2D bericht (Cloud-naar-apparaat) en stuurt deze terug naar het apparaat.

## <a name="optional-use-your-own-twitter-bearer-token"></a>Optioneel: Gebruik uw eigen Twitter bearer-token

Dit voorbeeldproject maakt gebruik van een vooraf geconfigureerde Twitter bearer-token voor testdoeleinden. Er is echter een [frequentielimiet](https://dev.twitter.com/rest/reference/get/search/tweets) voor elke Twitter-account. Als u uw eigen token kunt u overwegen wilt, volg deze stappen:

1. Ga naar [Twitter-ontwikkelaarsportal](https://dev.twitter.com/) om een nieuwe Twitter-app te registreren.

2. [Consumentsleutel en consumenten geheimen ophalen](https://support.yapsody.com/hc/en-us/articles/360003291573-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) van uw app.

3. Gebruik [enkele hulpprogramma](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) voor het genereren van een Twitter-bearer-token van deze twee sleutels.

4. In de [Azure-portal](https://portal.azure.com/){: target = "_blank"}, krijgen de **resourcegroep** en zoek de Azure-functie (Type: App Service) voor uw project "Schudden, door te schudden". De naam bevat altijd 'schudden...' string.

   ![Azure-functie](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Bijwerken van de code voor `run.csx` binnen **functies > shakeshake cs** met uw eigen token:

   ```csharp
   string authHeader = "Bearer " + "[your own token]";
  ```
  
  ![Twitter-token](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Sla het bestand en klik op **uitvoeren**.

## <a name="problems-and-feedback"></a>Problemen en feedback

Informatie over het oplossen van problemen of feedback geven. 

### <a name="problems"></a>Problemen met

Een probleem dat u kan zien als het scherm 'geen Tweets, wordt terwijl elke stap is uitgevoerd. Deze voorwaarde gebeurt normaal gesproken de eerste keer dat u implementeert en de steekproef worden uitgevoerd omdat de functie-app vereist overal in een paar seconden met minder dan één minuut koude start de app dat. 

Of, wanneer de code wordt uitgevoerd, er zijn enkele blips die ertoe leiden dat een opnieuw opstarten van de app. Als dit probleem gebeurt, krijgt de apparaat-app is een time-out voor het ophalen van de tweet. In dit geval kunt u een of beide van deze methoden voor het oplossen van het probleem proberen:

1. Klik op de knop opnieuw instellen op de DevKit opnieuw uit te voeren de apparaat-app.

2. In de [Azure-portal](https://portal.azure.com/), zoek de Azure Functions-app die u hebt gemaakt en start deze opnieuw:

   ![Azure-functie-opnieuw opstarten](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Feedback

Als u andere problemen ondervindt, raadpleegt u de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of neem contact op met ons opnemen met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een apparaat DevKit verbinden met uw externe controle van Azure IoT-oplossingsversnellers en ophalen van een tweet, vindt hier u de voorgestelde volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)