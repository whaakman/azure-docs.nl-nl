---
title: Azure API management-beleid voorbeeld - patroon implementeren X-CSRF | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe X CSRF patroon dat wordt gebruikt door veel API's implementeren. Dit voorbeeld is specifiek voor SAP-Gateway.
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
ms.openlocfilehash: 3a2067836a1488d117dced96f3935f2d1f8b1b48
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946020"
---
# <a name="implement-x-csrf-pattern"></a>Implementeer X-CSRF patroon

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u X CSRF patroon dat wordt gebruikt door veel API's implementeren. Dit voorbeeld is specifiek voor SAP-Gateway. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

