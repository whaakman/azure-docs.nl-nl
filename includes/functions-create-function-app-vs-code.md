---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: f9b853f0e86fc298ab35421928492ba813d85827
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444589"
---
## <a name="create-an-azure-functions-project"></a>Uw Functions-project met een functie maken 

De Azure Functions-projectsjabloon in Visual Studio Code maakt een project dat kan worden gepubliceerd in een functie-app in Azure. Met een functie-app kunt u functies groeperen in een logische eenheid, zodat u resources kunt beheren, implementeren en delen.

1. Druk op F1 voor het opdrachtenpalet te openen in Visual Studio Code. In het opdrachtenpalet, zoek en selecteer `Azure Functions: Create new project...`.

1. Kies een locatie voor de projectwerkruimte van uw en kies **Selecteer**.

    > [!NOTE]
    > Deze stappen zijn ontworpen om te worden uitgevoerd buiten een werkruimte. Selecteer in dit geval geen projectmap die deel uitmaakt van een werkruimte.

1. Geef de volgende informatie de aanwijzingen te volgen:

    | Vraag | Value | Description |
    | ------ | ----- | ----------- |
    | Selecteer een taal voor uw functie-app-project | C# of JavaScript | In dit artikel biedt ondersteuning voor C# en JavaScript. Zie voor Python, [in dit artikel Python](https://code.visualstudio.com/docs/python/tutorial-azure-functions), en voor PowerShell, Zie [in dit artikel PowerShell](../articles/azure-functions/functions-create-first-function-powershell.md).  |
    | Selecteer een sjabloon voor de eerste functie van uw project | HTTP-trigger | Een door HTTP geactiveerde functie maken in de nieuwe functie-app. |
    | Geef een functienaam | HttpTrigger | Druk op Enter de standaardnaam wilt gebruiken. |
    | Geef een naamruimte | My.Functions | (C# alleen) C# -klassebibliotheken moeten een naamruimte hebben.  |
    | Autorisatieniveau | Function | Vereist een [functietoets](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) voor het aanroepen van HTTP-eindpunt van de functie. |
    | Selecteer hoe u wilt uw project openen | aan werkruimte toevoegen | Hiermee maakt u de functie-app in de huidige werkruimte. |

Het functie-appproject wordt in een nieuwe werkruimte gemaakt. Het project bevat de configuratiebestanden [host.json](../articles/azure-functions/functions-host-json.md) en [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), plus eventuele taalspecifieke projectbestanden. 

Een nieuwe HTTP-geactiveerde functie wordt ook in de map HttpTrigger van de functie-app-project gemaakt.