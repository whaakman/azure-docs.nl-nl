---
title: Azure API management-beleid voorbeeld - mogelijkheden toevoegen aan een back-endservice | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie accepteren in plaats van de breedtegraad en lengtegraad in een weerprognose-API.
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
ms.openlocfilehash: 7c9edbf4b2d231453cd336521a04ba6b7714b696
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873821"
---
# <a name="add-capabilities-to-a-backend-service"></a>Mogelijkheden toevoegen aan een back-endservice

In dit artikel wordt een Azure API management-beleid voorbeeld die laat zien hoe u mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie accepteren in plaats van de breedtegraad en lengtegraad in een weerprognose-API. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

