---
title: Azure API management-beleid voorbeeld - Route op basis van de aanvraag van de grootte van de hoofdtekst | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe aanvragen op basis van de grootte van hun instanties te routeren.
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
ms.openlocfilehash: a93e1d9fecea59ebb68c512b96c8381b5b1a9346
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284757"
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>De aanvraag op basis van de grootte van de hoofdtekst routeren

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u aanvragen op basis van de grootte van hun instanties. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

