---
title: Azure API management-beleid voorbeeld - OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe verkrijgen van een toegangstoken van AAD en deze doorsturen naar de back-end.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: e0aeec66f23033f916c782c8a895e725b0735b62
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u OAuth2 voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe verkrijgen van een toegangstoken van AAD en deze doorsturen naar de back-end. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get OAuth2 access token from AAD and forward it to the backend.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

