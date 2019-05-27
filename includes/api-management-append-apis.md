---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: 2bfa356deeede1c16bd5a464ea7081132a67faf6
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238089"
---
## <a name="append-other-apis"></a>Andere API's toevoegen

Een API kan bestaan uit de API's die worden weergegeven door andere services, met inbegrip van de OpenAPI-specificatie, een SOAP-API, de API Apps-functie van Azure App Service, Azure Function App, Azure Logic Apps en Azure Service Fabric.

![Een API importeren](./media/api-management-append-apis/import.png)

Als u een andere API wilt toevoegen aan uw bestaande API, voert u de volgende stappen uit. Wanneer u een andere API importeert, worden de bewerkingen toegevoegd aan uw huidige API.

1. Ga naar uw Azure API Management-exemplaar in Azure Portal.
2. Selecteer **API's** in het menu aan de linkerkant.
3. Selecteer het beletselsteken ( **...** ) naast de API waaraan u een andere API wilt toevoegen.
4. Selecteer **Importeren** in de vervolgkeuzelijst.
5. Selecteer de service waaruit een API moet worden geïmporteerd.