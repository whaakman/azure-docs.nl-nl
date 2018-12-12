---
title: Voorbeeld van Azure API management-beleid - een koptekst met een correlatie-id toevoegen | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u een koptekst met een correlatie-id op de inkomende aanvraag kunt toevoegen.
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
ms.openlocfilehash: 126701674b6dc529404f5a7854cda9b31c336170
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52868426"
---
# <a name="add-a-header-containing-a-correlation-id"></a>Een koptekst met een correlatie-id toevoegen

In dit artikel wordt een Azure API management-beleid voorbeeld die laat zien hoe u een koptekst met een correlatie-id op de inkomende aanvraag kunt toevoegen. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

