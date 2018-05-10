---
title: Azure API management-beleid voorbeeld - OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe verkrijgen van een toegangstoken van AAD en deze doorsturen naar de back-end.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 4d518d37997b3ef014b85a4371bb3842fcab4373
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u OAuth2 voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe verkrijgen van een toegangstoken van AAD en deze doorsturen naar de back-end. 

Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

Het volgende script maakt gebruik van eigenschappen die worden weergegeven in de {{de eigenschap}}. Zie voor meer informatie over eigenschappen en het gebruik ervan in API Management-beleidsregels, [dit](../api-management-howto-properties.md) onderwerp.
 
## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

