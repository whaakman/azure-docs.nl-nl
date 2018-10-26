---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 49ac1a7585ddf2a6500c7e9382880109c3f7f431
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133271"
---
## <a name="export-an-api-definition"></a>Een API-definitie exporteren
Hebt u een OpenAPI-definitie voor de functie van [OpenAPI-definitie voor een functie maken](../articles/azure-functions/functions-openapi-definition.md). De volgende stap in dit proces is de API-definitie exporteren, zodat PowerApps en Microsoft Flow kan worden gebruikt in een aangepaste API.

> [!IMPORTANT]
> Houd er rekening mee dat u moet worden aangemeld bij Azure met dezelfde referenties die u gebruikt voor uw PowerApps en Microsoft Flow-tenants. Hierdoor kunnen Azure voor het maken van de aangepaste API en beschikbaar maken voor PowerApps en Microsoft Flow.

1. In de [Azure-portal](https://portal.azure.com), klikt u op de naam van uw functie-app (zoals **function-demo-energy**) > **platformfuncties** > **API-definitie** .

    ![API-definitie](media/functions-export-api-definition/api-definition.png)

1. Klik op **exporteren naar PowerApps en Flow**.

    ![Bron van API-definitie](media/functions-export-api-definition/export-api-1.png)

1. Gebruik de instellingen zoals opgegeven in de tabel in het rechter deelvenster.

    |Instelling|Beschrijving|
    |--------|------------|
    |**Exportmodus**|Selecteer **Express** voor het automatisch genereren van de aangepaste API. Selecteren **handmatig** uitvoer de API-definitie, maar vervolgens u moet importeren in PowerApps en Microsoft Flow handmatig. Zie voor meer informatie, [exporteren naar PowerApps en Microsoft Flow](../articles/azure-functions/app-service-export-api-to-powerapps-and-flow.md).|
    |**Omgeving**|Selecteer de omgeving waarin de aangepaste API moet worden opgeslagen. Zie voor meer informatie, [overzicht van omgevingen (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) of [overzicht van omgevingen (Microsoft Flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/).|
    |**Naam van de aangepaste API**|Voer een naam in, zoals `Turbine Repair`.|
    |**De naam van de API-sleutel**|Voer de naam die Apps en stromen builders ziet in de gebruikersinterface van de aangepaste API. Houd er rekening mee dat het voorbeeld nuttige informatie bevat.|
 
    ![Exporteren naar PowerApps en Microsoft Flow](media/functions-export-api-definition/export-api-2.png)

1. Klik op **OK**. De aangepaste API is nu gemaakt en toegevoegd aan de omgeving die u hebt opgegeven.