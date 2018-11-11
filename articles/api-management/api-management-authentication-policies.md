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
ms.openlocfilehash: 4c4c03fffa5786bf3a50f4d2c03511f0a2de0f48
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250948"
---
# <a name="api-management-authentication-policies"></a>API Management-verificatiebeleid
Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Verificatiebeleid  
  
-   [Verifiëren met Basic](api-management-authentication-policies.md#Basic) -verificatie met een back-endservice basisverificatie wordt gebruikt.  
  
-   [Verifiëren met clientcertificaat](api-management-authentication-policies.md#ClientCertificate) -verificatie met een back endservice met behulp van clientcertificaten.  
  
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
  
|Naam|Beschrijving|Vereist|  
|----------|-----------------|--------------|  
|verificatie-basic|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|gebruikersnaam|Hiermee geeft u de gebruikersnaam van de Basic-referentie.|Ja|N/A|  
|wachtwoord|Hiermee geeft u het wachtwoord van de Basic-referentie.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
  
-   **Beleid bereiken:** API  
  
##  <a name="ClientCertificate"></a> Verifiëren met een clientcertificaat  
 Gebruik de `authentication-certificate` beleid om te verifiëren met een back-endservice-clientcertificaat gebruiken. Het certificaat moet worden [geïnstalleerd in API Management](https://go.microsoft.com/fwlink/?LinkID=511599) eerste en wordt aangeduid met de vingerafdruk.  
  
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
|certificaat voor clientverificatie|Root-element.|Ja|  
  
### <a name="attributes"></a>Kenmerken  
  
|Naam|Beschrijving|Vereist|Standaard|  
|----------|-----------------|--------------|-------------|  
|vingerafdruk|De vingerafdruk voor het clientcertificaat.|Ja|N/A|  
  
### <a name="usage"></a>Gebruik  
 Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Beleid secties:** inkomend  
  
-   **Beleid bereiken:** API  
  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   
