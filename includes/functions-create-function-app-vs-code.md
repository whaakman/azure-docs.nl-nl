---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 79dbee33928fbc7560d0ea27be3af25cc510e996
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132286"
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

1. Kies **Add to workspace** (Aan werkruimte toevoegen) als daarom wordt gevraagd.

Het functie-appproject wordt in een nieuwe werkruimte gemaakt. Het project bevat de configuratiebestanden [host.json](../articles/azure-functions/functions-host-json.md) en [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), plus eventuele taalspecifieke projectbestanden. In de projectmap ziet u ook een nieuwe Git-opslagplaats.