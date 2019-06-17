---
title: Azure API management-beleid voorbeeld - toegang op basis van JWT-claims verlenen | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u verleent toegang tot specifieke HTTP-methoden voor een API op basis van JWT-claims.
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
ms.openlocfilehash: d656cf7c7bed1d40bbde654f9c2484efcc5df25d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61062149"
---
# <a name="authorize-access-based-on-jwt-claims"></a>Toegang op basis van JWT-claims verlenen

In dit artikel wordt een Azure API management-beleid voorbeeld die laat zien hoe u toegang verlenen aan specifieke HTTP-methoden voor een API op basis van JWT-claims. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Pre-authorize requests based on HTTP method with validate-jwt.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

