---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 6d6451d50a00569eb1da8f5b0a0dc10d3c6b1115
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841392"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Een omgevingsvariabele voor verificatie configureren

Toepassingen moeten verifiëren van toegang tot de Cognitive Services die ze gebruiken. Als u wilt verifiëren, wordt u aangeraden een omgevingsvariabele voor het opslaan van de sleutels voor uw Azure-Resources te maken. 

Nadat u uw sleutel hebt, schrijft u deze naar een nieuwe omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. Als u de omgevingsvariabele wilt instellen, opent u een consolevenster en volgt u de aanwijzingen voor uw besturingssysteem. Vervang `your-key` met een van de sleutels voor uw resource.

* Windows

    ```console
    setx COGNITIVE_SERVICE_KEY "your-key"
    ```

    Nadat u de omgevingsvariabele hebt toegevoegd, moet u actieve programma's die de omgevingsvariabele moeten lezen, opnieuw starten. Start ook het consolevenster opnieuw. Bijvoorbeeld, als u Visual Studio als editor gebruikt, Visual Studio opnieuw voordat u het voorbeeld uitvoert.

* Linux
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Nadat u de omgevingsvariabele toevoegt, voert u `source ~/.bashrc` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden.
    
* macOS
    
    Bewerk uw .bash_profile en voeg de omgevingsvariabele toe:
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Nadat u de omgevingsvariabele toevoegt, voert u `source .bash_profile` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden.
