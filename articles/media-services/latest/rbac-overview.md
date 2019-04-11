---
title: Op rollen gebaseerd toegangsbeheer voor Media Services-accounts - Azure | Microsoft Docs
description: In dit artikel wordt beschreven op rollen gebaseerd toegangsbeheer (RBAC) voor Azure Media Services-accounts.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 212d7f5352b58ee8f5b2c119bb1f5f828591f6bf
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471870"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Op rollen gebaseerd toegangsbeheer (RBAC) voor Media Services-accounts

Azure Media Services biedt op dit moment niet gedefinieerd voor de specifieke aangepaste rollen naar de service. Klanten kunnen gebruikmaken van de ingebouwde rollen van **eigenaar** of **Inzender** voor volledige toegang tot een Media Services-account. Het belangrijkste verschil tussen deze rollen is: de **eigenaar** kunt bepalen wie toegang heeft tot een resource en de **Inzender** kan niet. De ingebouwde gegevenslezer-account heeft alleen leestoegang tot het Media Services-account. 

## <a name="design-principles"></a>Ontwerpprincipes

Een van de belangrijkste principes van de API v3 is het beter beveiligen van de API. V3 API's retourneren geen geheimen of referenties op **ophalen** of **lijst** bewerkingen. De sleutels zijn altijd null, leeg of opgeschoond uit het antwoord. De gebruiker moet een aparte actie-methode voor het ophalen van geheimen of referenties aan te roepen. De **lezer** rol bewerkingen kan niet worden aangeroepen, zodat bewerkingen zoals Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets kan niet worden aangeroepen. Afzonderlijke acties kunt u meer gedetailleerde RBAC-beveiligingsrechten instellen in een aangepaste rol indien gewenst.

Als u de operations-Media Services ondersteunt, doen:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

De [ingebouwde roldefinities](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) artikel ziet u precies wat de rol, hebben. 

Zie de volgende artikelen voor meer informatie:

- [Klassieke abonnementsbeheerdersrollen, Azure RBAC-rollen en Azure AD-beheerdersrollen](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [Wat is er RBAC voor Azure-resources?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [RBAC gebruiken om toegang te beheren](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Inhoud sleutel beleid met behulp van Media Services .NET ophalen](get-content-key-policy-dotnet-howto.md)

## <a name="next-steps"></a>Volgende stappen

[Ontwikkelen met mediaservices v3 API 's](media-services-apis-overview.md)
