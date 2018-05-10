---
title: Azure API management-beleid voorbeeld - fouten verzenden naar Stackify voor logboekregistratie | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe u een beleid voor fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toevoegen...
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
ms.openlocfilehash: e47a3736e814229e881a314c7cb05054369db1f0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="send-errors-to-stackify-for-logging"></a>Fouten naar Stackify verzenden voor logboekregistratie

In dit artikel bevat een voorbeeld van Azure API management-beleid die laat zien hoe u een beleid fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toe te voegen. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **bij fouten** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Log errors to Stackify.policy.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

