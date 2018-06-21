---
title: Azure API management-beleid steekproef - mogelijkheden toevoegen aan een back-endservice | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie accepteren in plaats van de breedtegraad en lengtegraad in een weerprognose-API.
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
ms.openlocfilehash: a019eb4556dc7cde34d51af6858f576e8ea9abcf
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284528"
---
# <a name="add-capabilities-to-a-backend-service"></a>Mogelijkheden toevoegen aan een back-endservice

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie accepteren in plaats van de breedtegraad en lengtegraad in een weerprognose-API. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

