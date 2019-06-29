---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 734fce2c01614d5dca73f171fb59f25f39d13705
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461498"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Een omgevingsvariabele voor verificatie configureren

Toepassingen moeten verifiëren van toegang tot de Cognitive Services die ze gebruiken. Als u wilt verifiëren, wordt u aangeraden een omgevingsvariabele voor het opslaan van een sleutel in uw Azure-Resources te maken. 

Nadat u uw sleutel hebt, schrijft u deze naar een nieuwe omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. Als u de omgevingsvariabele wilt instellen, opent u een consolevenster en volgt u de aanwijzingen voor uw besturingssysteem. Vervang `your-key` door uw toegangssleutel voor de detectie van afwijkingen:

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
