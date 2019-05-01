---
title: Azure API Management-verificatiebeleid | Microsoft Docs
description: Meer informatie over de verificatiebeleidsregels die beschikbaar zijn voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: c0f8da779ca656cf357c418b8766a53307643695
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708788"
---
# <a name="api-management-authentication-policies"></a>API Management-verificatiebeleid
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Verificatiebeleid  
  
-   [Verifiëren met Basic](api-management-authentication-policies.md#Basic) -verificatie met een back-endservice basisverificatie wordt gebruikt.  
  
-   [Verifiëren met clientcertificaat](api-management-authentication-policies.md#ClientCertificate) -verificatie met een back endservice met behulp van clientcertificaten.  

-   [Verifiëren met beheerde identiteit](api-management-authentication-policies.md#ManagedIdentity) -verificatie met de [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) voor de API Management-service.  
  
##  <a name="Basic"></a> Verifiëren met Basic  
 Gebruik de `authentication-basic` beleid om te verifiëren met een back-endservice basisverificatie wordt gebruikt. Dit beleid wordt de HTTP-autorisatie-header effectief ingesteld op de waarde die overeenkomt met de referenties die zijn opgegeven in het beleid.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|verificatie-basic|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Name|Description|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|gebruikersnaam|Hiermee geeft u de gebruikersnaam van de Basic-referentie.|Ja|N/A|  
|password|Hiermee geeft u het wachtwoord van de Basic-referentie.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
  
-   **Beleid bereiken:** API  
  
##  <a name="ClientCertificate"></a> Verifiëren met een clientcertificaat  
 Gebruik de `authentication-certificate` beleid om te verifiëren met een back-endservice-clientcertificaat gebruiken. Het certificaat moet worden [geïnstalleerd in API Management](https://go.microsoft.com/fwlink/?LinkID=511599) eerste en wordt aangeduid met de vingerafdruk.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<authentication-certificate thumbprint="thumbprint" certificate-id="resource name"/>  
```  
  
### <a name="examples"></a>Voorbeelden  
  
In dit voorbeeld van de client wordt certificaat aangeduid met de vingerafdruk.
```xml  
<authentication-certificate thumbprint="CA06F56B258B7A0D4F2B05470939478651151984" />  
``` 
In dit voorbeeld wordt clientcertificaat aangeduid met de resourcenaam van de.
```xml  
<authentication-certificate certificate-id="544fe9ddf3b8f30fb490d90f" />  
```  

### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|certificaat voor clientverificatie|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Name|Description|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|vingerafdruk|De vingerafdruk voor het clientcertificaat.|De `thumbprint` of `certificate-id` moet aanwezig zijn.|N/A|  
|certificaat-id|De naam van de certificaat-resource.|De `thumbprint` of `certificate-id` moet aanwezig zijn.|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
  
-   **Beleid bereiken:** API  

##  <a name="ManagedIdentity"></a> Verifiëren met beheerde identiteit  
 Gebruik de `authentication-managed-identity` beleid om te verifiëren met een back endservice met behulp van de beheerde identiteit van de API Management-service. De beheerde identiteit dit beleid effectief gebruikt voor het ophalen van een toegangstoken van Azure Active Directory voor toegang tot de opgegeven resource. 
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<authentication-managed-identity resource="resource" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<authentication-managed-identity resource="https://graph.windows.net" output-token-variable-name="test-access-token" ignore-error="true" /> 
```  
  
### <a name="elements"></a>Elementen  
  
|Name|Description|Vereist|  
|----------|-----------------|--------------|  
|verificatie-beheerde identiteit |Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Name|Description|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|Bron|tekenreeks. De App-ID-URI van de doel-web-API (beveiligde resource) in Azure Active Directory.|Ja|N/A|  
|uitvoer-token-variabele-naam|tekenreeks. Naam van de contextvariabele die token waarde als een objecttype ontvangt `string`.|Nee|N/A|  
|ignore-error|Booleaanse waarde. Indien ingesteld op `true`, de pijplijn beleid blijft om uit te voeren, zelfs als een toegangstoken is niet opgehaald.|Nee|false|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
  
-   **Beleid bereiken:** wereldwijd, product, API, bewerking  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
