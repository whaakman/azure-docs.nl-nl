---
title: Azure API management-beleid voorbeeld - OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe u een toegangstoken van AAD kunt verkrijgen en dit door kunt sturen naar de back-end.
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
ms.openlocfilehash: d064e918d514b9be1b9fa2dbf30c10edf5167908
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287817"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 gebruiken voor autorisatie tussen de gateway en een back-end

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u OAuth2 voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe u een toegangstoken van AAD kunt verkrijgen en dit door kunt sturen naar de back-end. 

Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

Het volgende script maakt gebruik van eigenschappen die worden weergegeven in de {{de eigenschap}}. Zie voor meer informatie over eigenschappen en het gebruik ervan in API Management-beleidsregels, [dit](../api-management-howto-properties.md) onderwerp.
 
## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

