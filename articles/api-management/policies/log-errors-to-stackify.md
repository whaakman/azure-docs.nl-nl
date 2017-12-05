---
title: Azure API management-beleid voorbeeld - fouten verzenden naar Stackify voor logboekregistratie | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe u een beleid voor fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toevoegen...
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
ms.openlocfilehash: 34319e43789d0f2c7d3e48a0277926eb598e88fc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="send-errors-to-stackify-for-logging"></a>Fouten naar Stackify verzenden voor logboekregistratie

In dit artikel bevat een voorbeeld van Azure API management-beleid die laat zien hoe u een beleid fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toe te voegen. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **bij fouten** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

