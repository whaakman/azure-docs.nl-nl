---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: f98f750c14666400f41a249b3d8379b423e6dee2
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572047"
---
## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Hoe APIM Proxy Server reageert met SSL-certificaten in de TLS-handshake

### <a name="clients-calling-with-sni-header"></a>Clients aanroepen met SNI-header
Als de klant een of meer aangepaste domeinen die zijn geconfigureerd voor de Proxy heeft, APIM kunt reageren op HTTPS-aanvragen van de aangepaste domeinen (bijvoorbeeld contoso.com) en de standaard-domein (bijvoorbeeld apim-service-name.azure-api.net). Op basis van de informatie in de kop van de Server Name Indication (SNI), reageert APIM met de juiste server-certificaat.

### <a name="clients-calling-without-sni-header"></a>Clients aanroepen zonder SNI-header
Als de klant met behulp van een client, verzendt de [SNI](https://tools.ietf.org/html/rfc6066#section-3) header, antwoorden op basis van de volgende logica wordt gemaakt van APIM:

* Als de service slechts één aangepaste domein voor de Proxy is geconfigureerd heeft, is het standaard-certificaat het certificaat dat was uitgegeven aan het aangepaste domein van de Proxy.
* Als de service meerdere aangepaste domeinen voor de Proxy is geconfigureerd (alleen ondersteund in de **Premium** laag), de klant kunt opgeven welk certificaat moet het standaard-certificaat. Om in te stellen het standaard-certificaat, de [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#hostnameconfiguration) eigenschap moet worden ingesteld op ' True ' ("defaultSslBinding": "true"). Als de klant heeft niet de eigenschap is ingesteld, is het standaard-certificaat het certificaat verleend aan standaarddomein Proxy, dat wordt gehost op *.azure-api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Ondersteuning voor opslag/POST-aanvraag met grote nettolading

APIM-proxyserver ondersteunt aanvragen met grote nettolading bij het gebruik van clientcertificaten in HTTPS (bijvoorbeeld nettolading > 40 KB). Om te voorkomen dat de aanvraag van de server loopt vast, klanten stelt de eigenschap ["negotiateClientCertificate": "true"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#hostnameconfiguration) op de Proxy-hostnaam. Als de eigenschap is ingesteld op true, de client certificaat wordt aangevraagd bij verbindingstijd van SSL/TLS, voordat u een HTTP-aanvraag exchange. Omdat de instelling is van toepassing op de **Proxy-hostnaam** niveau, alle verbindingsaanvragen vragen voor het clientcertificaat. Klanten kunnen maximaal 20 aangepaste domeinen configureren voor Proxy (alleen ondersteund in de **Premium** laag) en om deze beperking.

