---
title: 'Quickstart: Gesproken tekst herkennen in Python met behulp van de Speech Service SDK'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Python met behulp van de Speech Service SDK
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: chlandsi
ms.openlocfilehash: 7610b12b351b2652df7ade603711d4d92e587292
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53723906"
---
# <a name="quickstart-using-the-speech-service-from-python"></a>Quickstart: Speech Service van Python gebruiken

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel ziet u hoe u Speech Service kunt gebruiken via de Speech SDK voor Python. U krijgt informatie over hoe u spraak kunt herkennen uit de invoer van een microfoon.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* [Python 3.5 (64-bits)](https://www.python.org/downloads/) of later.
* Het Python Speech-SDK-pakket is beschikbaar voor Windows (x64), Mac (macOS X-versie 10.12 of later) en Linux (Ubuntu 16.04 of 18.04 op x64).
* In Ubuntu voert u de volgende opdrachten uit om de vereiste pakketten te installeren:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

* In Windows hebt u ook het [Microsoft Visual C++ Redistributable for Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) voor uw platform nodig.

## <a name="get-the-speech-sdk-python-package"></a>Het Python Speech SDK-pakket ophalen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Het Cognitive Services Speech SDK Python-pakket kan worden geïnstalleerd vanuit [PyPI](https://pypi.org/) met behulp van deze opdracht op de opdrachtregel:

```sh
pip install azure-cognitiveservices-speech
```

De huidige versie van de Speech SDK van Cognitive Services is `1.2.0`.

## <a name="support-and-updates"></a>Ondersteuning en updates

Updates voor het Speech SDK Python-pakket worden gedistribueerd via PyPI en aangekondigd op de pagina [Releaseopmerkingen](./releasenotes.md).
Als een nieuwe versie beschikbaar is, kunt u een update naar deze versie uitvoeren met behulp van de opdracht `pip install --upgrade azure-cognitiveservices-speech`.
U kunt controleren welke versie momenteel is geïnstalleerd door de variabele `azure.cognitiveservices.speech.__version__` te bekijken.

Raadpleeg de [ondersteuningspagina](./support.md) als u een probleem ondervindt of als een functie ontbreekt.

## <a name="create-a-python-application-using-the-speech-sdk"></a>Een Python-toepassing maken met behulp van de Speech SDK

### <a name="running-the-sample-in-a-terminal"></a>Het voorbeeld uitvoeren in een terminal

U kunt de [code](#quickstart-code) uit deze quickstart kopiëren naar een bronbestand `quickstart.py` en uitvoeren in uw IDE of in de console

```sh
python quickstart.py
```

of u kunt deze quickstart-zelfstudie downloaden als een [Jupyter](https://jupyter.org)-notitieblok uit de [opslagplaats met Cognitive Services Speech-voorbeelden](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) en uitvoeren als een notitieblok.

### <a name="quickstart-code"></a>Quickstart-code

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="installing-the-speech-sdk-python-package-and-running-the-sample-in-visual-studio-code"></a>Het Python Speech SDK-pakket installeren en het voorbeeld uitvoeren in Visual Studio Code

1. [Download](https://www.python.org/downloads/) en installeer een 64-bits versie (3.5 of later) van Python op de computer.
1. [Download](https://code.visualstudio.com/Download) en installeer Visual Studio Code.
1. Open Visual Studio Code en installeer de Python-extensie door **Bestand** > **Voorkeuren** > **Extensies** te selecteren in het menu en te zoeken naar 'Python'.
   ![Python-extensie installeren](media/sdk/qs-python-vscode-python-extension.png)
1. Maak een map om het project in op te slaan, bijvoorbeeld met behulp van Windows Verkenner.
1. Klik in Visual Studio op het pictogram **Bestand** en open vervolgens de map die u hebt gemaakt.
   ![Map openen](media/sdk/qs-python-vscode-python-open-folder.png)
1. Maak een nieuw Python-bronbestand `speechsdk.py` door te klikken op het pictogram Nieuw bestand.
   ![Bestand maken](media/sdk/qs-python-vscode-python-newfile.png)
1. Kopieer en plak de [Python-code](#quickstart-code) en sla deze op in het zojuist gemaakte bestand.
1. Voeg de gegevens van uw Speech Service-abonnement in.
1. Als er al een Python-interpreter is geselecteerd, wordt deze onder aan het venster weergegeven, links van de statusbalk.
   Anders kunt u een lijst met beschikbare Python-interpreters ophalen door het **Opdrachtpalet** (`Ctrl+Shift+P`) te openen en te typen: **Python: Interpreter selecteren**. Kies vervolgens een geschikte interpreter.
1. Als het Python Speech SDK-pakket nog niet is geïnstalleerd voor de Python-interpreter die u hebt geselecteerd, kunt u dit eenvoudig doen vanuit Visual Studio Code.
   Als u het Speech SDK-pakket wilt installeren, opent u een terminal door het Opdrachtpalet opnieuw te openen (`Ctrl+Shift+P`) en te typen: **Terminal: Nieuwe geïntegreerde terminal maken**.
   Voer in de terminal die wordt geopend, de opdracht `python -m pip install azure-cognitiveservices-speech` in, of de juiste opdracht voor uw systeem.
1. Als u de voorbeeldcode wilt uitvoeren, klikt u met de rechtermuisknop ergens in de editor en selecteert u **Python-bestand uitvoeren in terminal**.
   Spreek een aantal woorden wanneer u hierom wordt gevraagd. De getranscribeerde tekst wordt kort hierna weergegeven.
   ![Voorbeeld uitvoeren](media/sdk/qs-python-vscode-python-run.png)

Als zich problemen voordoen met deze instructies, raadpleegt u de uitgebreidere [Python-zelfstudie voor Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-voorbeelden in GitHub bekijken](https://aka.ms/csspeech/samples)
