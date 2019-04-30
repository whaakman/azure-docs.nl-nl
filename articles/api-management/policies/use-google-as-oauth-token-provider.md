---
title: 'Azure API management-beleid voorbeeld: toestaan toegang met behulp van Google OAuth-token | Microsoft Docs'
description: Azure API management-beleid-voorbeeld - ziet u hoe u toegang verlenen aan uw eindpunten met behulp van Google als een provider van OAuth-token.
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
ms.openlocfilehash: 430f9e57df163ad345f0740e5bd5beca6e892a4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859139"
---
# <a name="authorize-access-using-google-oauth-token"></a>Toestaan van toegang met behulp van Google OAuth-token

In dit artikel wordt een Azure API management-beleid voorbeeld die laat zien hoe u toegang verlenen aan uw eindpunten met behulp van Google als een provider van OAuth-token. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

