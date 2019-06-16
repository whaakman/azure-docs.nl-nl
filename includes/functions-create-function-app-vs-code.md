---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 894ca0e78dfb75dffc124d3d25aa7a8e72adf627
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065551"
---
## <a name="create-an-azure-functions-project"></a>Een Azure Functions-project maken

De Azure Functions-projectsjabloon in Visual Studio Code maakt een project dat kan worden gepubliceerd in een functie-app in Azure. Met een functie-app kunt u functies groeperen in een logische eenheid, zodat u resources kunt beheren, implementeren en delen.

1. Selecteer het Azure-logo om weer te geven in Visual Studio Code, de **Azure: Functies** vlak- en selecteer vervolgens het pictogram Nieuw Project maken.

    ![Een functie-appproject maken](./media/functions-create-function-app-vs-code/create-function-app-project.png)

1. Kies een locatie voor de werkruimte van uw project en kies **Select** (Selecteren).

    > [!NOTE]
    > Dit artikel is bedoeld om buiten een werkruimte te worden voltooid. Selecteer in dit geval geen projectmap die deel uitmaakt van een werkruimte.

1. Selecteer de taal voor uw functie-appproject. In dit artikel wordt JavaScript gebruikt.
    ![Projecttaal kiezen](./media/functions-create-function-app-vs-code/create-function-app-project-language.png)

1. Selecteer een sjabloon voor de eerste functie voor uw project. Geef een naam voor uw functie.
    ![Eerste-functie kiezen](./media/functions-create-function-app-vs-code/create-function-app-project-first-function.png)

1. Kies **Add to workspace** (Aan werkruimte toevoegen) als daarom wordt gevraagd.

Het functie-appproject wordt in een nieuwe werkruimte gemaakt. Het project bevat de configuratiebestanden [host.json](../articles/azure-functions/functions-host-json.md) en [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), plus eventuele taalspecifieke projectbestanden. In de projectmap ziet u ook een nieuwe Git-opslagplaats.