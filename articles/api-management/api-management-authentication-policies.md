---
title: Azure API Management-verificatiebeleid | Microsoft Docs
description: Meer informatie over het verificatiebeleid dat beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4d13d9dbea9da9db5bfe9a9af85fdbf9eab1ae84
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-authentication-policies"></a>API Management-beleidsregels voor verificatie
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a>Verificatiebeleid  
  
-   [Verificatie met Basic](api-management-authentication-policies.md#Basic) -verificatie met een back-endservice basisverificatie wordt gebruikt.  
  
-   [Verificatie met een clientcertificaat](api-management-authentication-policies.md#ClientCertificate) -verificatie met een back-endservice met behulp van clientcertificaten.  
  
##  <a name="Basic"></a>Verificatie met Basic  
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
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|verificatie-basis|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|gebruikersnaam|Hiermee geeft u de gebruikersnaam van de algemene referentie.|Ja|N.v.t.|  
|wachtwoord|Hiermee geeft u het wachtwoord van de algemene referentie.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** API  
  
##  <a name="ClientCertificate"></a>Verificatie met een clientcertificaat  
 Gebruik de `authentication-certificate` beleid om te verifiëren met een back-endservice clientcertificaat. Het certificaat moet worden [geïnstalleerd in API Management](http://go.microsoft.com/fwlink/?LinkID=511599) eerste en wordt geïdentificeerd door de vingerafdruk.  
  
### <a name="policy-statement"></a>Beleidsverklaring  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Voorbeeld  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementen  
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|certificaat voor clientverificatie|Hoofdelement.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|vingerafdruk|De vingerafdruk van het clientcertificaat.|Ja|N.v.t.|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomende  
  
-   **Beleid scopes:** API  
  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
