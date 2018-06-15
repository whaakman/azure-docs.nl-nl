---
title: Voorbeeld van Azure API management-beleid - toevoegen van een koptekst doorgestuurde | Microsoft Docs
description: Azure API management-beleid-voorbeeld - demonstreert hoe u een koptekst doorgestuurde toevoegen in de binnenkomende aanvraag waarmee de end-API om juiste URL's samen te stellen.
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
ms.openlocfilehash: 00c8ac567b476d0591069c83c371d987d651de9d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935079"
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
