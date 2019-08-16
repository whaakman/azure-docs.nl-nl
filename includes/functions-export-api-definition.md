---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 49ac1a7585ddf2a6500c7e9382880109c3f7f431
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534270"
---
## <a name="export-an-api-definition"></a>Een API-definitie exporteren
U hebt een OpenAPI-definitie voor uw functie, van het [maken van een OpenAPI-definitie voor een functie](../articles/azure-functions/functions-openapi-definition.md). De volgende stap in dit proces is het exporteren van de API-definitie zodat PowerApps en Microsoft Flow deze kunnen gebruiken in een aangepaste API.

> [!IMPORTANT]
> Houd er rekening mee dat u moet zijn aangemeld bij Azure met dezelfde referenties die u gebruikt voor uw PowerApps-en Microsoft Flow-tenants. Hierdoor kan Azure de aangepaste API maken en deze beschikbaar maken voor PowerApps en Microsoft Flow.

1. Klik in de [Azure Portal](https://portal.azure.com)op de naam van uw functie-app (zoals **Function-Demo-energy**) > **platform functies** > **API-definitie**.

    ![API-definitie](media/functions-export-api-definition/api-definition.png)

1. Klik op **exporteren naar PowerApps en flow**.

    ![Bron van API-definitie](media/functions-export-api-definition/export-api-1.png)

1. Gebruik de instellingen die zijn opgegeven in de tabel in het rechterdeel venster.

    |Instelling|Description|
    |--------|------------|
    |**Export modus**|Selecteer **Express** om automatisch de aangepaste API te genereren. Als u **hand matig** de API-definitie exporteert selecteert, moet u deze hand matig importeren in PowerApps en Microsoft flow. Zie [exporteren naar PowerApps en Microsoft flow](../articles/azure-functions/app-service-export-api-to-powerapps-and-flow.md)voor meer informatie.|
    |**Omgeving**|Selecteer de omgeving waarin de aangepaste API moet worden opgeslagen. Zie overzicht van [omgevingen (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) of [environment Overview (Microsoft flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/)voor meer informatie.|
    |**Aangepaste API-naam**|Voer een naam in `Turbine Repair`, bijvoorbeeld.|
    |**API-sleutel naam**|Voer de naam in die door de app-en flow-opbouw functies moet worden weer gegeven in de aangepaste API-gebruikers interface. Houd er rekening mee dat het voor beeld nuttige informatie bevat.|
 
    ![Exporteren naar PowerApps en Microsoft Flow](media/functions-export-api-definition/export-api-2.png)

1. Klik op **OK**. De aangepaste API is nu gemaakt en toegevoegd aan de omgeving die u hebt opgegeven.