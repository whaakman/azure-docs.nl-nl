---
title: Azure API management-beleid steekproef - mogelijkheden toevoegen aan een back-endservice | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie in plaats van de breedtegraad en lengtegraad in een prognose weer API accepteren.
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
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>Mogelijkheden toevoegen aan een back-endservice

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie in plaats van de breedtegraad en lengtegraad in een prognose weer API accepteren. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

