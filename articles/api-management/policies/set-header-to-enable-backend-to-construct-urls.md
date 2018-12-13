---
title: Voorbeeld van Azure API management-beleid - een koptekst doorgestuurde toevoegen | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u een koptekst doorgestuurde toevoegen in de inkomende aanvraag voor het toestaan van de back-end-API voor het maken van de juiste URL's.
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
ms.openlocfilehash: b857d1780e9734ce891ce2c0ce4bedf50dfe13e9
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871452"
---
# <a name="add-a-forwarded-header"></a>Een koptekst doorgestuurde toevoegen

In dit artikel wordt een Azure API management-beleid voorbeeld die laat hoe u een koptekst doorgestuurde toevoegen in de inkomende aanvraag zien voor het toestaan van de back-end-API voor het maken van de juiste URL's. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="code"></a>Code

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)
