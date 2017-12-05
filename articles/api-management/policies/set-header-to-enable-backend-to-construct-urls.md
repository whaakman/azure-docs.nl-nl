---
title: Voorbeeld van Azure API management-beleid - toevoegen van een koptekst doorgestuurde | Microsoft Docs
description: Azure API management-beleid-voorbeeld - demonstreert hoe u een koptekst doorgestuurde toevoegen in de binnenkomende aanvraag waarmee de end-API om juiste URL's samen te stellen.
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
ms.openlocfilehash: cc2df914532b6cda37c951b65b243e90b63d57cb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-forwarded-header"></a>Een doorgestuurde koptekst toevoegen

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat hoe u een koptekst doorgestuurde toevoegen in de binnenkomende aanvraag zien voor het toestaan van de back-end API's voor het maken van de juiste URL's. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="code"></a>Code

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)
