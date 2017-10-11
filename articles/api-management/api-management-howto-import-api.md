---
title: Een API importeren in Azure API Management | Microsoft Docs
description: Informatie over het importeren van een API en bewerkingen in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Het importeren van de definitie van een API met bewerkingen in Azure API Management
Nieuwe API's kunnen worden gemaakt in API Management en de bewerkingen zijn handmatig toegevoegd of de API kan samen met de bewerkingen in één stap worden geïmporteerd.

API's en hun bewerkingen kunnen worden geïmporteerd met de volgende indelingen.

* WADL
* Swagger

Deze handleiding bevat een nieuwe API maken en importeren van bewerkingen in één stap. Zie voor informatie over het handmatig maken van een API en het toevoegen van bewerkingen, [API's maken] [ How to create APIs] en [bewerkingen toevoegen aan een API][How to add operations to an API].

## <a name="import-api"> </a>Een API importeren
API's zijn gemaakt en geconfigureerd in de publicatieportal. Voor toegang tot de publicatieportal bevindt, klikt u op **publicatieportal** in de Azure-Portal voor uw API Management-service. Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].

![Publicatieportal][api-management-management-console]

Klik op **API's** van de **API Management** menu aan de linkerkant en klik vervolgens op **importeren API**.

![API importeren][api-management-import-apis]

De **Import API** venster bestaat uit drie tabbladen die met de drie manieren overeenkomen om de API-specificatie.

* **Van Klembord** kunt u de API-specificatie plak in het tekstvak aangewezen.
* **Uit bestand** kunt u bladeren naar en selecteer het bestand waarin de API-specificatie.
* **Van URL** kunt u de URL de specificatie voor de API.

![Indeling van de API importeren][api-management-import-api-clipboard]

Gebruik de keuzerondjes aan de rechterkant om aan te geven van de indeling specificatie na het opgeven van de API-specificatie. De volgende indelingen worden ondersteund.

* WADL
* Swagger

Geef vervolgens een **achtervoegsel URL Web-API**. Dit wordt toegevoegd aan de basis-URL voor uw API management-service. De basis-URL is gemeenschappelijk voor alle API's die worden gehost op elk exemplaar van API Management-service. API Management API's onderscheidt door hun achtervoegsel en daarom het achtervoegsel moet uniek zijn voor elke API in een specifiek exemplaar van API management-service.

Nadat alle waarden zijn ingevoerd, klikt u op **opslaan** voor het maken van de API en de gekoppelde bewerkingen. 

> [!NOTE]
> Zie voor een zelfstudie over het importeren van de basisrekenmachine API in Swagger-indeling, [uw eerste API beheren in Azure API Management](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Exporteren van een API
Naast de nieuwe API's importeert, kunt u de definities van uw API's exporteren in de publicatieportal. Om dit te doen, klikt u op **exporteren API** van de **tabblad Samenvatting** van uw **API**.

![API exporteren][api-management-export-api]

API's kunnen worden geëxporteerd met WADL of Swagger. Selecteer de gewenste indeling, klikt u op **opslaan**, en kies de locatie op waar het bestand wilt opslaan.

![API-indeling voor exporteren][api-management-export-api-format]

## <a name="next-steps"> </a>Volgende stappen
Nadat een API is gemaakt en de bewerkingen die zijn geïmporteerd, die u kunt bekijken en configureren van eventuele aanvullende instellingen de API toevoegen aan een Product en publiceren zodat deze beschikbaar voor ontwikkelaars. Zie de volgende handleidingen voor meer informatie.

* [Het configureren van instellingen voor de API][How to configure API settings]
* [Het maken en een product publiceren][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
