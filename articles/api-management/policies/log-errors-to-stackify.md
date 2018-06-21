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
ms.openlocfilehash: 46b6d391d6a1ee569dc27c31a0718b23a120c632
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286715"
---
# <a name="send-errors-to-stackify-for-logging"></a>Fouten naar Stackify verzenden voor logboekregistratie

In dit artikel bevat een voorbeeld van Azure API management-beleid die laat zien hoe u een beleid fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toe te voegen. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **bij fouten** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

