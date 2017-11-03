---
title: Beperkingen en bekende problemen in Azure API Management-API importeren | Microsoft Docs
description: Details van de bekende problemen en beperkingen voor het importeren in Azure API Management met de Open API, WSDL of WADL indelingen.
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: 4cb6ad53b59b81f906a85027f4ff988bbb78706a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>API importeren beperkingen en bekende problemen
## <a name="about-this-list"></a>Over deze lijst
Terwijl elke inspanning wordt gedaan om ervoor te zorgen dat uw API importeren in Azure API Management is als naadloze en foutloos mogelijk we zo nu en dan beperkingen opleggen of problemen die worden verholpen moet voordat u met succes kunt importeren. In dit artikel wordt deze georganiseerd door de importindeling van de API.

## <a name="open-api"></a>API/Swagger openen
In het algemeen als u uw document Open API importeren fouten ontvangt, Controleer u hebt deze - met de ontwerpfunctie voor in de nieuwe Azure-Portal (ontwerp - Front-End - API specificatie Editor openen), gevalideerd of met een 3rd partij hulpprogramma zoals <a href="http://www.swagger.io">Swagger Editor</a>.

* **Hostnaam** moet een name-hostnaamkenmerk.
* **Pad baseren** moet een kenmerk basispad.
* **Schema's** moet een matrix van het schema. 

## <a name="wsdl"></a>WSDL
WSDL-bestanden worden gebruikt voor het genereren van SOAP Pass Through-API's of fungeren als de back-end van een SOAP-REST-API.

* **WSDL: import** wordt momenteel niet ondersteund met behulp van dit kenmerk API's. Klanten moeten de geïmporteerde elementen samenvoegen in één document.
* **Berichten met meerdere onderdelen** worden momenteel niet ondersteund.
* **WCF-wsHttpBinding** SOAP-services die zijn gemaakt met Windows Communication Foundation basicHttpBinding moeten gebruiken - wsHttpBinding wordt niet ondersteund.
* **MTOM** MTOM-Services <em>kan</em> werken. Op dit moment is geen officiële ondersteuning geboden.
* **Recursie** typen die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar een matrix van zichzelf) worden niet ondersteund.

## <a name="wadl"></a>WADL
Er zijn momenteel geen bekende problemen van WADL importeren.


[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
