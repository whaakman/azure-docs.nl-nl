---
title: Voorbeeld van Azure API management-beleid - toevoegen van een header met een correlatie-id | Microsoft Docs
description: Azure API management-beleid-voorbeeld - kunt u zien hoe een header met een correlatie-id moet de binnenkomende aanvraag.
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
ms.openlocfilehash: 7d339ba08720d482f622aa9b708d3a6e057eaaa8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935108"
---
# <a name="add-a-header-containing-a-correlation-id"></a>Een header met een correlatie-id toevoegen

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat zien hoe u een koptekst met een correlatie-id moet de binnenkomende aanvraag kunt toevoegen. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

