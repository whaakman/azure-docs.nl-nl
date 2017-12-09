---
title: Met behulp van client API's beveiligd tegen certificaatverificatie in API Management - Azure API Management | Microsoft Docs
description: Meer informatie over het beveiligen van toegang tot API's met behulp van clientcertificaten
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 841825923819bdb257e5b5983071d999cca805e9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Het beveiligen van API's met behulp van client certificaatverificatie in API Management

API Management biedt de mogelijkheid om toegang tot API's (dat wil zeggen, de client naar API Management) te beveiligen met behulp van clientcertificaten. Op dit moment kunt u de vingerafdruk van een certificaat met een waarde van de gewenste controleren. U kunt ook de vingerafdruk op basis van bestaande certificaten geüpload naar de API Management controleren.  

Zie voor meer informatie over het beveiligen van toegang tot de back-end-service van een API met behulp van clientcertificaten (dat wil zeggen, API Management aan back-end) [het beveiligen van back-end-services met behulp van client verificatie via certificaat](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

## <a name="checking-the-expiration-date"></a>Controleren of de vervaldatum

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren of het certificaat is verlopen:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a>Controle van de verlener en onderwerp

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren van de verlener en onderwerp van een certificaat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a>Controleren of de vingerafdruk

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren van de vingerafdruk van een certificaat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Controleren of een vingerafdruk tegen certificaten geüpload naar de API Management

Het volgende voorbeeld ziet u hoe u controleert de vingerafdruk van een clientcertificaat tegen certificaten geüpload naar de API Management: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Volgende stap

*  [Het beveiligen van back-end-services met behulp van client verificatie via certificaat](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
*  [Het uploaden van certificaten](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

