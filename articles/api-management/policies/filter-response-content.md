---
title: Azure API management-beleid voorbeeld - Filter antwoordinhoud | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe filteren gegevenselementen uit de nettolading van de reactie op basis van het product dat is gekoppeld aan de aanvraag.
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
ms.openlocfilehash: 26829dfc04acdac2a25fe0d4fdc3e95e4d219057
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637060"
---
# <a name="filter-response-content"></a>Filteren van inhoud van de reactie

In dit artikel wordt een Azure API management-beleid voorbeeld over het filteren van de gegevenselementen van de nettolading van de reactie op basis van het product dat is gekoppeld aan de aanvraag. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **uitgaande** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter response content based on product name.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

