---
title: 'Quickstart: Spraak herkennen, Python - Speech Services'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een consoletoepassing voor spraak-naar-tekst te maken met de Speech-SDK voor Python. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: d6b7cc275fc50fefbe0057620d315d1484c47745
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603020"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-python"></a>Quickstart: Spraak herkennen met de Speech-SDK voor Python

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dit artikel leest hoe u met de Speech Services via de spraak-SDK voor Python. U krijgt informatie over hoe u spraak kunt herkennen uit de invoer van een microfoon.

## <a name="prerequisites"></a>Vereisten

* De sleutel van een Azure-abonnement voor de spraakservices. [Gratis downloaden](get-started.md).
* [Python 3.5](https://www.python.org/downloads/) of hoger.
* Het Python Speech-SDK-pakket is beschikbaar voor deze besturingssystemen:
    * Windows: x64 en x86.
    * Mac: Mac OS X-versie 10.12 of hoger.
    * Linux: Ubuntu 16.04, Ubuntu 18.04, Debian 9 op x64.
* Voer deze opdrachten om de vereiste pakketten te installeren op Linux:

  * Op Ubuntu:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

  * Op Debian 9:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.2 libasound2
    ```

* Op Windows, moet u de [Microsoft Visual C++ Redistributable voor Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) voor uw platform.

## <a name="install-the-speech-sdk"></a>De Speech-SDK installeren

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Met deze opdracht installeert u het Python-pakket van [PyPI](https://pypi.org/) voor de Speech-SDK:

```sh
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Ondersteuning en updates

Updates voor het Python Speech-SDK-pakket worden gedistribueerd via PyPI en aangekondigd in de [Releaseopmerkingen](./releasenotes.md).
Als een nieuwe versie beschikbaar is, kunt u een update naar deze versie uitvoeren met behulp van de opdracht `pip install --upgrade azure-cognitiveservices-speech`.
U kunt controleren welke versie momenteel is geïnstalleerd door de variabele `azure.cognitiveservices.speech.__version__` te bekijken.

Zie [opties voor ondersteuning en hulp](./support.md) als er een probleem is of als er een functie ontbreekt.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Een Python-toepassing maken dat de Speech-SDK gebruikt

### <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

U kunt de [voorbeeldcode](#sample-code) uit deze quickstart kopiëren naar een `quickstart.py`-bronbestand en de voorbeeldcode uitvoeren in uw IDE of in de console:

```sh
python quickstart.py
```

Of u kunt deze quickstart-zelfstudie downloaden als een [Jupyter](https://jupyter.org)-notebook uit de [opslagplaats met Speech-SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) en de quickstart uitvoeren als een notitieblok.

### <a name="sample-code"></a>Voorbeeldcode

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>De Speech-SDK installeren en gebruiken met Visual Studio Code

1. Download en installeer een 64-bits versie (3.5 of hoger) van [Python](https://www.python.org/downloads/) op de computer.
1. Download en installeer [Visual Studio Code](https://code.visualstudio.com/Download).
1. Open Visual Studio Code en installeer de Python-extensie. Selecteer **Bestand** > **Voorkeuren** > **Extensies** in het menu. Zoek naar **Python**.

   ![De Python-extensie installeren](media/sdk/qs-python-vscode-python-extension.png)

1. Maak een map om daar het project in op te slaan. Dat kunt u bijvoorbeeld doen met Windows Verkenner.
1. Selecteer in Visual Studio Code het pictogram **Bestand**. Open vervolgens de map die u hebt gemaakt.

   ![Een map openen](media/sdk/qs-python-vscode-python-open-folder.png)

1. Maak een nieuw Python-bronbestand, `speechsdk.py`, door het pictogram Nieuw bestand te selecteren.

   ![Een bestand maken](media/sdk/qs-python-vscode-python-newfile.png)

1. Kopieer en plak de [Python-code](#sample-code) en sla deze op in het zojuist gemaakte bestand.
1. Plaats uw abonnementsgegevens Speech Services.
1. Als er een Python-interpreter is geselecteerd, wordt deze onder aan het venster weergegeven, links van de statusbalk.
   Anders wordt er een lijst met beschikbare Python-interpreters geopend. Open het opdrachtenpalet (Ctrl+Shift+P) en voer **Python: Interpreter selecteren** in. Kies de juiste versie.
1. U kunt het Python Speech-SDK-pakket installeren vanuit Visual Studio Code. Doe dat als het pakket nog niet is geïnstalleerd voor de Python-interpreter die u hebt geselecteerd.
   Open een terminal voor het installeren van het Speech-SDK-pakket. Open het opdrachtenpalet opnieuw (met Ctrl+Shift+P) en voer **Terminal: Nieuwe geïntegreerde terminal maken**.
   Voer in de terminal die wordt geopend, de opdracht `python -m pip install azure-cognitiveservices-speech` in, of de juiste opdracht voor uw systeem.
1. Als u de voorbeeldcode wilt uitvoeren, klikt u met de rechtermuisknop ergens in de editor. Selecteer **Python-bestand uitvoeren in de Terminal**.
   Zeg enkele woorden wanneer u daarom wordt gevraagd. De getranscribeerde tekst wordt kort daarna weergegeven.

   ![Een voorbeeld uitvoeren](media/sdk/qs-python-vscode-python-run.png)

Als er zich problemen voordoen met deze instructies, raadpleegt u de uitgebreidere [Python-zelfstudie voor Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-voorbeelden in GitHub bekijken](https://aka.ms/csspeech/samples)
