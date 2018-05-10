---
title: Ophalen van een Twitter-bericht met Azure Functions | Microsoft Docs
description: Gebruik de sensor beweging schudden detecteren en gebruiken van Azure Functions vinden van een willekeurige tweet met een hashtag die u opgeeft.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: 50ce34cb3289e346e7b637d917730dac8e8a097b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Schud, schud voor een Tweet--ophalen van een Twitter-bericht met Azure Functions!

In dit project leert u hoe u de sensor beweging gebruikt voor het activeren van een gebeurtenis met behulp van Azure Functions. De app haalt een willekeurige tweet met een #hashtag die u in uw tekening Arduino configureren. De tweet wordt weergegeven op het scherm DevKit.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) naar:

* Uw DevKit verbonden met Wi-Fi hebben.
* De ontwikkelomgeving voorbereiden.

Een actief Azure-abonnement. Als u niet hebt, kunt u via een van de volgende manieren registreren:

* Activeren van een [gratis 30-daagse evaluatieversie Microsoft Azure-account](https://azure.microsoft.com/free/)
* Claim uw [Azure-tegoeden](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u MSDN of Visual Studio-abonnee bent

## <a name="open-the-project-folder"></a>Open de projectmap

### <a name="start-vs-code"></a>VS Code starten

- Zorg ervoor dat uw DevKit **niet** aangesloten op uw computer.
- Start VS-Code.
- De DevKit aansluiten op uw computer.

> [!NOTE]
> Tijdens het starten van Code van de VS, verschijnt er een foutbericht weergegeven dat het Arduino IDE of verwante mededelingenbord pakket kan niet worden gevonden. Als deze fout optreedt, VS Code sluiten en de Arduino IDE opnieuw te starten. VS-Code moet het pad Arduino IDE nu correct vinden.

### <a name="open-arduino-examples-folder"></a>Voorbeelden van Arduino-map openen

Vouw de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **ShakeShake**. Een nieuw venster van de Code van de VS met een projectmap in deze wordt geopend.

![Mini solution-voorbeelden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

> [!NOTE]
> U kunt bijvoorbeeld ook openen vanuit opdracht palet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="provision-azure-services"></a>Azure-services inrichten

In het oplossingsvenster kunt u uw taak uitvoeren via `Ctrl+P` (Mac OS: `Cmd+P`) door te voeren `task cloud-provision`.

In de terminal VS-Code in leert een interactieve opdrachtregel u de vereiste Azure-services inrichten:

![cloud-inrichten](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Als de pagina reageert niet meer in de status geladen wanneer u probeert aan te melden bij Azure, raadpleegt u dit [Veelgestelde vragen over stap](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure).
 
## <a name="modify-the-hashtag"></a>De #hashtag wijzigen

Open `ShakeShake.ino` en zoekt u deze regel:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Vervang de tekenreeks `iot` tussen de accolades met uw voorkeur hashtag. DevKit haalt later een willekeurige tweet waarin de hashtag die u in deze stap opgeeft.

## <a name="deploy-azure-functions"></a>Azure Functions implementeren

Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task cloud-deploy` om te beginnen met het implementeren van de Azure Functions-code:

![cloud implementeren](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> In sommige gevallen kan wellicht de functie Azure werkt niet correct. U lost dit probleem wanneer deze zich voordoet, dit selectievakje inschakelt [Veelgestelde vragen over stap](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Maken en uploaden van de apparaatcode

### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.
2. De terminal vraagt u om de configuratie activeren. Dit doet u als volgt:

   * Houd een
   * Push- en release van de knop herstellen.

3. Het scherm wordt weergegeven voor de DevKit-ID en 'Configuration'.
4. Hiermee stelt u de verbindingsreeks die is opgehaald uit de `task cloud-provision` stap.
5. VS-Code is gestart, controleren en de Arduino uploaden werk aan uw DevKit: ![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
6. De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Kan een ' fout: AZ3166: onbekende pakket ' foutbericht. Deze fout treedt op wanneer de mededelingenbord package index correct is niet vernieuwd. U lost dit probleem door dit selectievakje inschakelt [Veelgestelde vragen over stap](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

### <a name="macos"></a>macOS

1. De DevKit in configuratiemodus geplaatst: houd knop A, en vervolgens push en release van de knop herstellen. Het scherm weergegeven 'Configuration'.
2. Gebruik `Cmd+P` om uit te voeren `task device-upload` om in te stellen van de verbindingsreeks die is opgehaald uit de `task cloud-provision` stap.
3. VS-Code is gestart, controleren en de Arduino uploaden werk aan uw DevKit: ![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
4. De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Kan een ' fout: AZ3166: onbekende pakket ' foutbericht. Deze fout treedt op wanneer de mededelingenbord package index correct is niet vernieuwd. U lost dit probleem door dit selectievakje inschakelt [Veelgestelde vragen over stap](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Na de initialisatie van de app, klikt u op en een release vervolgens voorzichtig Schud het mededelingenbord DevKit. Deze actie worden opgehaald van een willekeurige tweet waarin de hashtag die u eerder hebt opgegeven. Een tweet wordt binnen een paar seconden weergegeven op het scherm DevKit:

### <a name="arduino-application-initializing"></a>Arduino toepassing initialiseren...
![Het initialiseren van Arduino-toepassing](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Druk op A om door te schudden...
![Druk op een te schudden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Gereed om te schudden...
![Gereed om te schudden](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>Wordt verwerkt...
![Verwerken](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Druk op B lezen...
![Druk op B te lezen](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Een willekeurige tweet weergeven...
![Weergave een willekeurige tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Druk op de knop een opnieuw en klik vervolgens voor een nieuwe tweet schud.
- Druk op de knop B door de rest van de tweet moeten schuiven.

## <a name="how-it-works"></a>Hoe werkt het?

![Diagram](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Het schema Arduino verzendt een gebeurtenis met de Azure IoT Hub. Deze gebeurtenis wordt geactiveerd de Azure Functions-app. Apps van Azure Functions bevat de logica voor het verbinding maken met de Twitter-API en een tweet ophalen. Deze vervolgens de tekst tweet verpakt in een C2D bericht (Cloud-naar-apparaat) en stuurt deze terug naar het apparaat.

## <a name="optional-use-your-own-twitter-bearer-token"></a>Optioneel: Gebruik uw eigen Twitter bearer-token

Dit voorbeeldproject maakt gebruik van een vooraf geconfigureerde Twitter bearer-token voor testdoeleinden. Er is echter een [frequentielimiet](https://dev.twitter.com/rest/reference/get/search/tweets) voor elke Twitter-account. Als u uw eigen token kunt u overwegen wilt, volg deze stappen:

1. Ga naar [Twitter ontwikkelaarsportal](https://dev.twitter.com/) om een nieuwe Twitter-app te registreren.

2. [Ophalen van de consumentsleutel en consumenten geheimen](https://support.yapsody.com/hc/en-us/articles/203068116-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) van uw app.

3. Gebruik [sommige hulpprogramma](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) Twitter bearer-token genereren van deze twee sleutels.

4. In de [Azure-portal](https://portal.azure.com/){: target = "_blank"}, krijgen de **resourcegroep** en zoek de Azure-functie (Type: App Service) voor uw project 'Trillen, schud'. De naam bevat altijd 'schud...' tekenreeks.
  ![azure-function](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Bijwerken van de code voor `run.csx` binnen **functies > shakeshake cs** met uw eigen token:
  ```csharp
  ...
  string authHeader = "Bearer " + "[your own token]";
  ...
  ```
  ![Twitter-token](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Sla het bestand en klik op **uitvoeren**.


## <a name="problems-and-feedback"></a>Problemen en feedback

### <a name="the-screen-displays-no-tweets-while-every-step-has-run-successfully"></a>'Er is geen Tweets' in het scherm worden weergegeven terwijl elke stap is uitgevoerd

Deze voorwaarde gebeurt normaal gesproken de eerste keer dat u implementeert en de steekproef worden uitgevoerd omdat de functie app overal uit een aantal seconden aan zo veel 1 minuut voor koude start de app vereist. Of, wanneer de code wordt uitgevoerd, er zijn enkele blips die ertoe leiden dat een opnieuw starten van de app. Als dit probleem gebeurt, kan de app voor het apparaat een time-out voor het ophalen van de tweet ophalen. In dit geval kunt u een of beide van deze methoden voor het oplossen van dit probleem proberen:

1. Klik op de knop herstellen op de DevKit opnieuw uit te voeren de apparaat-app.

2. In de [Azure-portal](https://portal.azure.com/), de Azure Functions-app die u hebt gemaakt te zoeken en deze opnieuw starten: ![azure-functie-opnieuw opstarten](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Feedback

Als u andere problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact met ons opnemen uit de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe een DevKit apparaat aansluit op uw oplossingsverbetering Azure IoT externe controle en ophalen van een tweet, vindt hier u de voorgestelde volgende stappen uit:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
