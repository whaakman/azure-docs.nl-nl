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
ms.date: 05/30/2019
ms.author: apimpm
ms.openlocfilehash: 5427c4050b6b70c18da7a1899d16e448c41e81c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427343"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Hoe u met behulp van client-API's beveiligen certificaatverificatie in API Management

API Management biedt de mogelijkheid voor het beveiligen van toegang tot API's (dat wil zeggen, de client naar API Management) met behulp van clientcertificaten. U kunt binnenkomende certificaat valideren en controleer de certificaateigenschappen van het op basis van de gewenste waarden met behulp van beleidsexpressies.

Zie voor meer informatie over het beveiligen van toegang tot de back-end-service van een API met behulp van clientcertificaten (dat wil zeggen, API Management naar back-end) [over het beveiligen van back-endservices met behulp van client-certificaatverificatie](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

> [!IMPORTANT]
> Voor het ontvangen en controleer of clientcertificaten in de laag verbruik moet u eerst de op "Aanvraag client certificate" instellen op de blade 'Aangepaste domeinen', zoals hieronder wordt weergegeven.

![Clientcertificaat aanvragen](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Controle van de verlener en onderwerp

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren of de uitgever en het onderwerp van een clientcertificaat:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Om te controleren of lijst met gebruik van de certificaat intrekken uitschakelen `context.Request.Certificate.VerifyNoRevocation()` in plaats van `context.Request.Certificate.Verify()`.
> Als het clientcertificaat is zelf-ondertekend, root (of tussenliggende) CA-certifica(a)t(en) moet [geüpload](api-management-howto-ca-certificates.md) in API Management voor `context.Request.Certificate.Verify()` en `context.Request.Certificate.VerifyNoRevocation()` om te werken.

## <a name="checking-the-thumbprint"></a>De vingerafdruk controleren

Onderstaande beleidsregels kunnen worden geconfigureerd om te controleren of de vingerafdruk van een clientcertificaat:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Om te controleren of lijst met gebruik van de certificaat intrekken uitschakelen `context.Request.Certificate.VerifyNoRevocation()` in plaats van `context.Request.Certificate.Verify()`.
> Als het clientcertificaat is zelf-ondertekend, root (of tussenliggende) CA-certifica(a)t(en) moet [geüpload](api-management-howto-ca-certificates.md) in API Management voor `context.Request.Certificate.Verify()` en `context.Request.Certificate.VerifyNoRevocation()` om te werken.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Een vingerafdruk op basis van certificaten controleren geüpload naar de API Management

Het volgende voorbeeld laat zien hoe om te controleren of de vingerafdruk van een clientcertificaat op basis van certificaten die zijn geüpload naar de API Management:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Om te controleren of lijst met gebruik van de certificaat intrekken uitschakelen `context.Request.Certificate.VerifyNoRevocation()` in plaats van `context.Request.Certificate.Verify()`.
> Als het clientcertificaat is zelf-ondertekend, root (of tussenliggende) CA-certifica(a)t(en) moet [geüpload](api-management-howto-ca-certificates.md) in API Management voor `context.Request.Certificate.Verify()` en `context.Request.Certificate.VerifyNoRevocation()` om te werken.

> [!TIP]
> Client certificate impasse probleem dat wordt beschreven in dit [artikel](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) op verschillende manieren kunt worden manifest, zoals aanvragen blokkeert, resultaat van de aanvragen `403 Forbidden` statuscode na een time-out optreedt, `context.Request.Certificate` is `null`. Dit probleem treedt meestal op `POST` en `PUT` aanvragen met de lengte van de inhoud van ongeveer 60 KB of groter.
> Om te voorkomen dat dit probleem zich voordoet inschakelen voor 'Onderhandelen over client certificate'-instelling van de gewenste hostnamen op de blade 'Aangepaste domeinen', zoals hieronder weergegeven. Deze functie is niet beschikbaar in de laag verbruik.

![Clientcertificaat onderhandelen](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Volgende stappen

-   [Over het beveiligen van back-endservices met behulp van client-certificaatverificatie](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [Over het uploaden van certificaten](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
