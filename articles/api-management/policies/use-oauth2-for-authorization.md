---
title: Azure API management-beleid-voorbeeld - OAuth2 gebruiken voor verificatie tussen de gateway en een back-end | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u OAuth2 gebruiken voor verificatie tussen de gateway en een back-end. Er wordt weergegeven hoe u een toegangstoken van AAD kunt verkrijgen en dit door kunt sturen naar de back-end.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 519233cb9e77bf48f67d869a54af771c17c7827e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874553"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 gebruiken voor verificatie tussen de gateway en een back-end

In dit artikel wordt een Azure API management-beleid voorbeeld over het gebruiken van OAuth2 voor verificatie tussen de gateway en een back-end. Er wordt weergegeven hoe u een toegangstoken van AAD kunt verkrijgen en dit door kunt sturen naar de back-end. 

Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

Het volgende script maakt gebruik van eigenschappen die worden weergegeven in {{de eigenschap}}. Zie voor meer informatie over eigenschappen en het gebruik ervan in API Management-beleidsregels, [dit](../api-management-howto-properties.md) onderwerp.
 
## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

