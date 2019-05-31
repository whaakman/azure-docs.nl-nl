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
ms.date: 05/23/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 93b2cd3a2565b14ea07d6db6b14dd146e4223528
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236919"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Op rollen gebaseerd toegangsbeheer (RBAC) voor Media Services-accounts

Azure Media Services biedt op dit moment niet specifieke aangepaste rollen definiëren met de service. Als u volledige toegang tot het Media Services-account, klanten kunnen gebruikmaken van de ingebouwde rollen van **eigenaar** of **Inzender**. Het belangrijkste verschil tussen deze rollen is: de **eigenaar** kunt bepalen wie toegang heeft tot een resource en de **Inzender** kan niet. De ingebouwde **lezer** rol kan ook worden gebruikt, maar de gebruiker of toepassing wordt alleen leestoegang hebben tot de Media Services-API's. 

## <a name="design-principles"></a>Ontwerpprincipes

Een van de belangrijkste principes van de API v3 is het beter beveiligen van de API. V3 API's retourneren geen geheimen of referenties op **ophalen** of **lijst** bewerkingen. De sleutels zijn altijd null, leeg of opgeschoond uit het antwoord. De gebruiker moet een aparte actie-methode voor het ophalen van geheimen of referenties aan te roepen. The **Reader** role cannot call operations like Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets. Afzonderlijke acties kunt u meer gedetailleerde RBAC-beveiligingsrechten instellen in een aangepaste rol indien gewenst.

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
- [Media Services-resourceproviderbewerkingen](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftmedia)

## <a name="next-steps"></a>Volgende stappen

- [Ontwikkelen met mediaservices v3 API 's](media-services-apis-overview.md)
- [Inhoud sleutel beleid met behulp van Media Services .NET ophalen](get-content-key-policy-dotnet-howto.md)
