---
title: Met behulp van client-API's beveiligen in API Management - Azure API Management-certificaatverificatie | Microsoft Docs
description: Meer informatie over het beveiligen van toegang tot API's met behulp van clientcertificaten
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 3307ea391734828cb83c927e8df8aca79685279a
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441533"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Hoe u met behulp van client-API's beveiligen certificaatverificatie in API Management

API Management biedt de mogelijkheid voor het beveiligen van toegang tot API's (dat wil zeggen, de client naar API Management) met behulp van clientcertificaten. Op dit moment kunt u de vingerafdruk van een certificaat met een gewenste waarde controleren. De vingerafdruk op basis van bestaande certificaten geüpload naar de API Management kunt u ook controleren.  

Zie voor meer informatie over het beveiligen van toegang tot de back-end-service van een API met behulp van clientcertificaten (dat wil zeggen, API Management naar back-end) [over het beveiligen van back-endservices met behulp van client-certificaatverificatie](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="checking-the-expiration-date"></a>De vervaldatum controleren

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren als het certificaat is verlopen:

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

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren of de uitgever en het onderwerp van een clientcertificaat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a>De vingerafdruk controleren

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren of de vingerafdruk van een clientcertificaat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Een vingerafdruk op basis van certificaten controleren geüpload naar de API Management

Het volgende voorbeeld laat zien hoe om te controleren of de vingerafdruk van een clientcertificaat op basis van certificaten die zijn geüpload naar de API Management: 

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

*  [Over het beveiligen van back-endservices met behulp van client-certificaatverificatie](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
*  [Over het uploaden van certificaten](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

